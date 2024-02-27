<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import GameHeader from './components/GameHeader.vue';
import GameFigure from './components/GameFigure.vue';
import GameWrongLetters from './components/GameWrongLetters.vue';
import GameWord from './components/GameWord.vue';
import GamePopap from './components/GamePopap.vue';
import GameNotification from './components/GameNotification.vue';
import { useRandomWord } from './composables/useRandomWord';

const { word, getRandomWord } = useRandomWord();

const letters = ref<string[]>([]);
const notification = ref<InstanceType<typeof GameNotification> | null>(null);
const popup = ref<InstanceType<typeof GamePopap> | null>(null);

const correctleters = computed(() => {
  return letters.value.filter(letter => word.value.includes(letter))
});

const wrongleters = computed(() => {
  return letters.value.filter(letter => !word.value.includes(letter))
});
const isLose = computed(() => wrongleters.value.length === 6);
const isWin = computed(() => [...word.value].every(x => correctleters.value.includes(x)));

  watch(wrongleters, () => {
    if(isLose.value) {
      popup.value?.open('lose');
    }
  });

watch(correctleters, () => {
  if(isWin.value) {
    popup.value?.open('win');
  }
});


window.addEventListener('keydown', ({ key }) => {
  if(isWin.value || isLose.value) return;

  if(letters.value.includes(key)) {
    notification.value?.open();
    setTimeout(() => notification.value?.close(), 2000);
    return;
  }

  if(/[а-яА-ЯёЁ]/.test(key)){
    letters.value.push(key.toLowerCase());
  }
});

const restart = async () => {
  await getRandomWord()
  letters.value = [];
  popup.value?.close();
}

</script>

<template>
    <div id="app">

      <GameHeader />
      <div class="game-container">
       
        <GameFigure 
        :wrongleters-count="wrongleters.length"/>
  
        <GameWrongLetters 
        :wrongleters="wrongleters"
        />
  
        <GameWord 
        :word="word" 
        :correctleters="correctleters"
        />
      </div>
  
      <!-- Container for final message -->
      
        <GamePopap 
        :word="word"  
        @restart="restart" 
        ref="popup"
        />
      
  
      <!-- Notification -->
      <GameNotification ref="notification" />
    </div>
 
</template>

<style scoped lang="scss">

</style>
