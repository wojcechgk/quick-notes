<script setup lang="ts">
import {nextTick, onMounted, onUnmounted, ref} from "vue";
import LineItem from "@/components/LineItem.vue";
const minLineItems = 10
const mainContainerElement = ref<HTMLElement>()
const handleKeyDown = (e: KeyboardEvent) => {
  // console.log(e.key)
  if (e.key === 'ArrowUp') {
    e.preventDefault()
    if(activeIndex.value === -1) return
    activeIndex.value = activeIndex.value - 1
  }
  if (e.key === 'ArrowDown') {
    e.preventDefault()
    if (activeIndex.value + 1 === lineItems.value.length) {
      lineItems.value.push(undefined)
    }
    activeIndex.value = activeIndex.value + 1
  }
  if (e.key === 'Backspace') {
    lineItems.value.splice(activeIndex.value, 1)
    nextTick(() => {
      if(activeIndex.value === -1) {
        activeIndex.value = lineItems.value.length - 1
      } else {
        activeIndex.value = activeIndex.value - 1
      }
      if (lineItems.value.length < minLineItems) {
        lineItems.value.push(undefined)
      }
    })
  }
}
onMounted(() => {
  mainContainerElement.value.addEventListener("keydown", handleKeyDown);
})
onUnmounted(() => {
  mainContainerElement.value.removeEventListener("keydown", handleKeyDown);
})

const activeIndex = ref(-1)
const lineItems = ref(Array(minLineItems))
</script>

<template>
  {{ activeIndex }}
  <div ref="mainContainerElement" class="main-container">
    <LineItem
      v-for="(item, index) in lineItems"
      key="index"
      @click="activeIndex = index"
      :content="item"
      :is-active="activeIndex === index"
    />
  </div>
</template>

<style scoped>
.main-container {
  border-radius: 12px;
  width: 100%;
  max-width: 1200px;
  background: #17242d;
  margin: 8rem auto;
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 0.5rem;
}
</style>
