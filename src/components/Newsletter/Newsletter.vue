<template>
  <v-sheet border="md" class="newsletter pa-6 mx-auto" :style="{ backgroundColor: props.globalStyles.backgroundColor }" max-width="800">
    <NewsletterSection
      v-for="section in newsletterSections"
      :globalStyles="globalStyles"
      :localStyles="section.localStyles"
      :type="section.type"
      :content="section.content"
    />
  </v-sheet>
</template>

<script setup>
// PROPS
const props = defineProps({
  globalStyles: {
    type: Object,
    required: true
  },
  newsletterSections: {
    type: Array,
    required: true
  }
})
</script>

<style lang="scss">
.newsletter > h1:not(:last-child),
.newsletter > p:not(:last-child) {
  padding-bottom: 20px;
}
</style>