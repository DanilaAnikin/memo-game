<template>
    <div class="w-full flex flex-col items-center max-w-4xl px-4 text-center">
      <div class="grid md:grid-cols-6 gap-4 grid-cols-4">
        <Card v-for="card in cards" :key="card.id" :card="card" @click="cardClicked(card)" />
      </div>
      <button class="mt-8 bg-blue-500 text-white px-4 py-2 rounded" @click="restartGame">Restart</button>
      <p v-if="isGameWon" class="mt-4 text-green-600 text-xl font-bold">Well done! You've got nice memory</p>
    </div>
</template>
  
<script setup lang="ts">
import { ref, computed } from 'vue';
import Card from './Card.vue';
  
interface CardType {
    id: number;
    type: number;
    img: string;
    flipped: boolean;
    matched: boolean;
}
  
const images = [
    new URL('../images/google.webp', import.meta.url).href,
    new URL('../images/facebook.png', import.meta.url).href,
    new URL('../images/instagram.png', import.meta.url).href,
    new URL('../images/linkedin.png', import.meta.url).href,
    new URL('../images/messenger.png', import.meta.url).href,
    new URL('../images/snapchat.png', import.meta.url).href,
    new URL('../images/whatsapp.webp', import.meta.url).href,
    new URL('../images/telegram.webp', import.meta.url).href,
    new URL('../images/discord.png', import.meta.url).href,
    new URL('../images/tiktok.webp', import.meta.url).href,
    new URL('../images/twitch.png', import.meta.url).href,
    new URL('../images/spotify.png', import.meta.url).href,
];
  
const cards = ref<CardType[]>([]);
const firstCard = ref<CardType | null>(null);
const disableBoard = ref(false);  

const shuffleArray = (array: CardType[]) => {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));

    [array[i], array[j]] = [array[j], array[i]];
  }
  
  return array;
};
  
const initializeCards = () => {
  let tempCards: CardType[] = [];
  let id = 0;
  
  for (let i = 0; i < images.length; i++) {
    tempCards.push({ id: id++, type: i, img: images[i], flipped: false, matched: false });
    tempCards.push({ id: id++, type: i, img: images[i], flipped: false, matched: false });
  }
  
  tempCards = shuffleArray(tempCards);
  cards.value = tempCards;
};
  
const cardClicked = (card: CardType) => {
  if (disableBoard.value) return;
  if (card.flipped || card.matched) return;
  
  card.flipped = true;
  
  if (firstCard.value === null) {
    firstCard.value = card;
  } else {
    disableBoard.value = true;
    
    if (firstCard.value.type === card.type) {
      firstCard.value.matched = true;
      card.matched = true;
      firstCard.value = null;
      disableBoard.value = false;
    } else {
      setTimeout(() => {
        if (firstCard.value) firstCard.value.flipped = false;
  
        card.flipped = false;
        firstCard.value = null;
        disableBoard.value = false;
  
      }, 700);
    }
  }
};
  
const restartGame = () => {
    firstCard.value = null;
    disableBoard.value = false;

    initializeCards();
};
  
const isGameWon = computed(() => cards.value.every(card => card.matched));

initializeCards();
</script>