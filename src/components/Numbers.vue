<script setup lang="ts">
import { ref, computed } from 'vue';

const limit = ref(100);
const activeDivisors = ref(new Set<number>());


const shuffledNumbers = computed(() => {
  const arr = Array.from({ length: limit.value }, (_, i) => i);
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
});

function getDivisors(n: number) {
  const divisors = new Set<number>();
  for (let i = 1; i <= n; i++) {
    if (n % i === 0) divisors.add(i);
  }
  return divisors;
}

function handleHover(number: number) {
  activeDivisors.value = getDivisors(number);
}

function reset() {
  activeDivisors.value.clear();
}
</script>

<template>
  <div>
    <input 
      type="number" 
      v-model.number="limit" 
      min="1" 
      class="limit-input"
    /><br><br>
    
    <div class="numbers-container">
      <div
        v-for="number in shuffledNumbers"
        :key="number"
        class="number"
        :class="{ active: activeDivisors.has(number) }"
        @mouseover="handleHover(number)"
        @mouseout="reset"
      >
        {{ number }}
      </div>
    </div>
  </div>
</template>

<style>
.limit-input {
  padding: 5px;
  margin-bottom: 1rem;
}

.numbers-container {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
}

.number {
  display: inline-block;
  padding: 4px 8px;
  background-color: #e0e0e0;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.number.active {
  background-color: #ff4444;
  color: white;
}

.number:hover {
  background-color: #bdbdbd;
}
</style>