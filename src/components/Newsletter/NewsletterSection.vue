<template>
  <!-- TITLE -->
  <h1 :style="{
    color: localStyles.color || globalStyles.titleColor,
    fontFamily: localStyles.fontFamily || globalStyles.titleFont,
    fontSize: localStyles.fontSize ? localStyles.fontSize + 'px' : globalStyles.titleSize + 'px'
  }" v-if="type == 1">{{ content }}</h1>
  <!-- TEXT -->
  <p v-else :style="{
    color: localStyles.color || globalStyles.textColor,
    fontFamily: localStyles.fontFamily || globalStyles.textFont,
    fontSize: localStyles.fontSize ? localStyles.fontSize + 'px' : globalStyles.textSize + 'px'
  }">{{ content }}</p>
</template>

<script setup>
const props = defineProps({
  globalStyles: {
    type: Object,
    default: () => {}
  },
  localStyles: {
    type: Object,
    default: undefined,
  },
  type: {
    type: Number,
    default: 0
  },
  content: {
    type: String,
    required: true
  }
})
</script>