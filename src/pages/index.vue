<template>
  <!-- SIDEBAR -->
  <Sidebar
    :globalStyles="globalStyles" 
    @onGlobalStylesChange="Object.assign(globalStyles, $event)"
    @onLocalStylesChange="Object.assign(newsletterSections, $event)"
    :newsletterSections="newsletterSections"
  />

  <!-- NEWSLETTER -->
  <Newsletter
    :globalStyles="globalStyles"
    :newsletterSections="newsletterSections"
    class="mt-8 mb-8"
  />
</template>

<script setup>
import { reactive, watch, onBeforeMount } from 'vue'

// GLOBAL STYLES
const globalStyles = reactive({
  backgroundColor: "#FFF",
  titleSize: 32,
  titleFont: "Roboto",
  titleColor: "#000",
  textSize: 16,
  textFont: "Roboto",
  textColor: "#000"
})

// DOCUMENT SECTIONS
// 0 = TEXT
// 1 = TITLE
// 2 = IMAGE
// 3 = VIDEO
const newsletterSections = reactive([{
  localStyles: {}, // If the individual component's styles get replaced, an object with styles information is stored here
  type: 1, // First element becomes "Header" by automatically.
  content: "Hatsune Miku Caught Lip-Syncing During Concert",
}, {
  localStyles: {},
  type: 0,
  content: `"I was absolutely floored," confessed 16-year-old Mitsuki Tanaka, a die-hard Miku fan. "I've seen her live countless times, and her voice was always so powerful, so crystal-clear. But this time... it just sounded... off."`,
}, {
  localStyles: {},
  type: 0,
  content: `Suspicions arose after eagle-eyed fans noticed subtle inconsistencies between Miku's on-stage vocals and the pre-recorded tracks played during the show. "The timing was slightly off," explained Kenji Sato, a vocaloid enthusiast and self-proclaimed "Mikuologist." "And there were moments where her mouth movements didn't quite match the lyrics."`,
}, {
  localStyles: {},
  type: 1,
  content: `Caught red-handed…`,
}, {
  localStyles: {},
  type: 0,
  content: `Gasps echoed through the arena as fans, who had shelled out exorbitant sums for holographic tickets, witnessed the impossible: Miku's perfectly synthesized vocals, those iconic vocaloid melodies, seemed to drift in from a distant speaker, while her animated lips flapped along with a comical lack of conviction.`,
}, {
  localStyles: {},
  type: 0, // Last element becomes "Footer" automatically.
  content: `"It was… underwhelming," confessed 12-year-old Kenji, clutching his Miku plushie. "I thought she'd be, you know, actually singing. Like, with her soul."`,
}])

// WATCHERS
watch(globalStyles, (n) => { window.localStorage.setItem("globalStyles", JSON.stringify(n)) })
watch(newsletterSections, (n) => { window.localStorage.setItem("localStyles", JSON.stringify(n)) })

// LIFECYCLE HOOKS
onBeforeMount(() => {
  const storedGlobal = localStorage.getItem("globalStyles")
  if (storedGlobal) Object.assign(globalStyles, JSON.parse(storedGlobal))
  const storedLocal = localStorage.getItem("localStyles")
  if (storedLocal) Object.assign(newsletterSections, JSON.parse(storedLocal))
})
</script>
