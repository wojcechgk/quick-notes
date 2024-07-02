<script setup lang="ts">
import {ref, toRefs, watch} from "vue";

const props = withDefaults(defineProps<{
  content?: string
  isActive?: boolean
}>(), {
  isActive: false
})

const { content, isActive } = toRefs(props)

const inputValue = ref(props.content || '')
const uniqueId = ref(new Date().valueOf())

watch(
  isActive,
  (newVal) => {
    if(newVal) {
      document.getElementById(uniqueId)?.focus()
    }
  },
  { immediate: true }
)

</script>

<template>
  <component :id="uniqueId" :is="isActive ? 'input' : 'div'" class="line-item" tabindex="0">
  </component>
</template>

<style scoped>
.line-item {
  height: 22px;
  width: 100%;
  background: rgba(0,0,0,0.1);
}
.line-item:focus {
  background: red;
}
</style>
