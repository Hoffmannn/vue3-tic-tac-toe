<script setup lang="ts">
import { defineProps } from 'vue'

const { value, handleClick } = defineProps({
  value: {
    type: String,
    required: true
  },
  handleClick: {
    type: Function,
    required: true
  }
})
</script>

<template>
  <div class="square" v-on:click="handleClick(value)">
    {{ value }}
  </div>
</template>

<style>
.square {
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3rem;
  cursor: pointer;

  &:hover {
    background-color: #ffffff11;
  }
}
</style>