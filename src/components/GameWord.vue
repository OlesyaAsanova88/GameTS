<script setup lang="ts">
interface Props {
    word: string,
    correctleters: string[]
}

defineProps<Props>()
</script>

<template>
    <div class="word">
        <span 
            v-for="(letter, index) in word" 
            :key="index" 
            class="letter">
            {{ correctleters.includes(letter) ? letter : '' }}
        </span>
    </div>
</template>