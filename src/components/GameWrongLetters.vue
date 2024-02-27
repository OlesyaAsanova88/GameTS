<script setup lang="ts">
interface Props {
  wrongleters: string[]
}

defineProps<Props>()
</script>

<template>
    <div v-if="wrongleters.length > 0" class="wrong-letters-container">
        <div class="wrong-letters">
          <p>Ошибки</p>
          <span 
          v-for="(letter, index) in wrongleters"
          :key="letter"
          >
            {{index === wrongleters.length - 1 ? letter : letter + ', ' }} </span>
          
        </div>
    </div>
</template>