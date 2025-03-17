<template>
    <div
      class="lg:w-36 lg:h-36 md:w-28 md:h-28 w-24 h-24 flex justify-center items-center rounded shadow cursor-pointer border border-gray-400"
      @click="$emit('click')"
    >
      <img
        v-if="card.flipped || card.matched"
        :src="card.img"
        class="lg:w-32 lg:h-32 md:w-24 md:h-24 w-20 h-20 object-cover"
        />
        <div
            v-else
            class="object-cover rounded"
        >
            <ArrowPathIcon class="h-8 w-8" />
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import { ArrowPathIcon } from '@heroicons/vue/24/solid';

interface CardType {
    id: number;
    type: number;
    img: string;
    flipped: boolean;
    matched: boolean;
}

const props = defineProps<{ card: CardType }>();
const emits = defineEmits(['click']);
</script>
