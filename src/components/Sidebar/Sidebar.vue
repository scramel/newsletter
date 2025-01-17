<template>
  <v-navigation-drawer v-model="drawer" :location="$vuetify.display.mobile ? 'bottom' : 'right'" width="300">

    <!-- CLOSE BUTTON -->
    <!-- <v-list density="compact" nav>
      <v-list-item append-icon="mdi-window-close" title="Cerrar" value="close" @click="drawer = false"></v-list-item>
    </v-list>
    <v-divider></v-divider> -->

    <!-- GLOBAL STYLES -->
    <v-list density="compact" nav>
      <v-list-group value="Global">
        <template v-slot:activator="{ props }">
          <v-list-item
            v-bind="props"
            prepend-icon="mdi-earth"
            title="Global"
          ></v-list-item>
        </template>
        <!-- BACKGROUND COLOR -->
        <v-list-item append-icon="mdi-format-color-fill" title="Color de fondo" ></v-list-item>
        <v-color-picker v-model="global.backgroundColor" :modes="['rgb', 'hsl', 'hex']" mode="hex" canvas-height="120" width="100%" elevation="0" ></v-color-picker>
        <!-- TITLE FONT -->
        <v-list-item append-icon="mdi-format-title" title="Fuente de título" ></v-list-item>
        <v-select v-model="global.titleFont" label="Seleccione" :items="fontOptions" variant="outlined">
          <template v-slot:item="{ props, item }">
            <v-list-item v-bind="props" :style="{fontFamily: item.raw}"></v-list-item>
          </template>
        </v-select>
        <!-- TITLE SIZE -->
        <v-list-item append-icon="mdi-format-size" title="Tamaño de título" ></v-list-item>
        <v-slider v-model="global.titleSize" :max="maxFontSize" :min="minFontSize" :step="1"  class="ma-4" hide-details>
          <template v-slot:append>
            <v-text-field
              v-model="global.titleSize"
              @update:modelValue="global.titleSize = validateFontSize(global.titleSize)"
              :max="maxFontSize"
              :min="minFontSize"
              density="compact"
              style="width: 80px"
              type="number"
              variant="outlined"
              hide-details
            ></v-text-field>
          </template>
        </v-slider>
        <!-- TITLE COLOR -->
        <v-list-item append-icon="mdi-format-color-fill" title="Color de título" ></v-list-item>
        <v-color-picker v-model="global.titleColor" :modes="['rgb', 'hsl', 'hex']" mode="hex" canvas-height="120" width="100%" elevation="0" ></v-color-picker>
        <!-- TEXT FONT -->
        <v-list-item append-icon="mdi-format-text" title="Fuente de texto" ></v-list-item>
        <v-select v-model="global.textFont" label="Seleccione" :items="fontOptions" variant="outlined">
          <template v-slot:item="{ props, item }">
            <v-list-item v-bind="props" :style="{fontFamily: item.raw}"></v-list-item>
          </template>
        </v-select>
        <!-- TEXT SIZE -->
        <v-list-item append-icon="mdi-format-size" title="Tamaño de texto" ></v-list-item>
        <v-slider v-model="global.textSize" :max="maxFontSize" :min="minFontSize" :step="1"  class="ma-4" hide-details>
          <template v-slot:append>
            <v-text-field
              v-model="global.textSize"
              @update:modelValue="global.textSize = validateFontSize(global.textSize)"
              :max="maxFontSize"
              :min="minFontSize"
              density="compact"
              style="width: 80px"
              type="number"
              variant="outlined"
              hide-details
            ></v-text-field>
          </template>
        </v-slider>
        <!-- TEXT COLOR -->
        <v-list-item append-icon="mdi-format-color-fill" title="Color de texto" ></v-list-item>
        <v-color-picker v-model="global.textColor" :modes="['rgb', 'hsl', 'hex']" mode="hex" canvas-height="120" width="100%" elevation="0" ></v-color-picker>
      </v-list-group>

      <!-- NEWSLETTER SECTIONS -->
      <v-list-group v-for="(section, index) in newsletters" :value="index">
        <template v-slot:activator="{ props }">
          <v-list-item
            v-bind="props"
            :prepend-icon="getListGroupIcon(section.type, index)"
            :title="getListGroupTitle(section.type, index)"
          ></v-list-item>
        </template>
        <!-- RESET -->
        <v-list-item append-icon="mdi-refresh" title="Usar estilos globales" @click="section.localStyles = {}"></v-list-item>
        <!-- FONT FAMILY -->
        <v-list-item append-icon="mdi-format-text" title="Fuente de texto" ></v-list-item>
        <v-select v-model="section.localStyles.fontFamily" label="Seleccione" :items="fontOptions" variant="outlined">
          <template v-slot:item="{ props, item }">
            <v-list-item v-bind="props" :style="{fontFamily: item.raw}"></v-list-item>
          </template>
        </v-select>
        <!-- FONT SIZE -->
        <v-list-item append-icon="mdi-format-size" title="Tamaño de texto" ></v-list-item>
        <v-slider v-model="section.localStyles.fontSize" :max="maxFontSize" :min="minFontSize" :step="1"  class="ma-4" hide-details>
          <template v-slot:append>
            <v-text-field
              v-model="section.localStyles.fontSize"
              @update:modelValue="section.localStyles.fontSize = validateFontSize(section.localStyles.fontSize)"
              :max="maxFontSize"
              :min="minFontSize"
              density="compact"
              style="width: 80px"
              type="number"
              variant="outlined"
              hide-details
            ></v-text-field>
          </template>
        </v-slider>
        <!-- COLOR -->
        <v-list-item append-icon="mdi-format-color-fill" title="Color de texto" ></v-list-item>
        <v-color-picker v-model="section.localStyles.color" :modes="['rgb', 'hsl', 'hex']" mode="hex" canvas-height="120" width="100%" elevation="0" ></v-color-picker>
      </v-list-group>
    </v-list>
    <!-- <v-divider></v-divider>
    <v-list density="compact" nav>
      <v-list-item append-icon="mdi-plus" title="Añadir…" value="add"></v-list-item>
    </v-list> -->
  </v-navigation-drawer>
</template>

<script setup>
import { ref, watch } from 'vue'

// PROPS
const props = defineProps({
  globalStyles: {
    type: Object,
    required: true
  },
  newsletterSections: {
    type: Array,
    default: () => {}
  }
})

// CONSTS
const global = reactive({ ...props.globalStyles }) // Global styles are destructured so it doesn't affect the original object in memory
const newsletters = reactive({ ...props.newsletterSections }) // This will later replace a section's styling if edited
const drawer = ref(true) // Sidebar toggle
const fontOptions = ["Roboto", "Kalam", "Dancing Script"]
const maxFontSize = 96
const minFontSize = 6

// EMIT FUNCTION
const emit = defineEmits(["onGlobalStylesChange", "onLocalStylesChange"])
watch(global, (n) => { emit("onGlobalStylesChange", n) })
watch(newsletters, (n) => { emit("onLocalStylesChange", n) })

// METHODS
function getListGroupTitle(sectionType, index) {
  switch (true) {
    case index == 0:
      return "Encabezado"
    case index == props.newsletterSections.length - 1:
      return "Pie de página";
    case sectionType == 0:
      return `${props.newsletterSections[index].content || "Texto"}`
    case sectionType == 1:
      return `${props.newsletterSections[index].content || "Título"}`
    default:
      return "Indefinido"
  }
}
function getListGroupIcon(sectionType, index) {
  switch (true) {
    case index == 0:
      return "mdi-page-layout-header"
    case index == props.newsletterSections.length - 1:
      return "mdi-page-layout-footer"
    case sectionType == 0:
      return "mdi-format-text"
    case sectionType == 1:
      return "mdi-format-title"
    default:
      return "mdi-alert-circle"
  }
}
function validateFontSize(value) {
  switch (true) {
    case value < minFontSize:
      return minFontSize
    case value > maxFontSize:
      return maxFontSize
    default:
      return value
  }
}
</script>