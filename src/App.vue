<template>
  <label
    >Columns: <input v-model="inputColumnCount" type="number" min="1" max="12"
  /></label>
  <label
    >Content Width:<input
      v-model="inputcontentWidth"
      type="number"
      min="500"
      max="1400"
  /></label>
  <label
    >Breakout Width:<input
      v-model="inputBreakouSize"
      type="number"
      min="0"
      :max="(gridLines.maxWidth - gridLines.content.width) / 2"
  /></label>
  <label
    >Max Width:<input
      v-model="inputMaxWidth"
      type="number"
      :min="inputcontentWidth"
  /></label>
  <label>Max Width:<input v-model="inputGap" type="number" /></label>
  <div class="grid-lines">
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div v-for="columns in columngrid" :key="columns" class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
  </div>
  <main class="smol-breakout-grid"></main>
  <div>{{ columnsRule }}</div>
</template>
<script setup>
import { ref, computed } from 'vue'
const inputColumnCount = ref(3)
const inputcontentWidth = ref(900)
const inputBreakouSize = ref(100)
const inputMaxWidth = ref(1200)
const inputGap = ref(0)
const gridLines = computed(() => {
  return {
    content: {
      width: ref(inputcontentWidth.value),
      columnCount: ref(inputColumnCount.value),
    },
    breakout: {
      width: ref(inputBreakouSize.value),
    },
    maxWidth: ref(inputMaxWidth.value),
  }
})

const columnsRule = computed(() => {
  let rule = ''
  rule += `
  [max-width-area-start]
    minmax(0,1fr)
      [wide-start wide-left-start]
      minmax( var(--padding-inline),var(--breakout-size))
      [content-block-start `
  for (let i = 1; i < gridLines.value.content.columnCount.value + 1; i++) {
    rule += `col-${i}-start] min(var(--one-column)) [col-${i}-end `
  }
  rule += `content-block-end]
        minmax(var(--padding-inline),var(--breakout-size))
      [wide-end wide-right-end]
      minmax(0,1fr)
  [max-width-area-end]`
  return rule
})
const columngrid = computed(() => {
  let rule = {}
  for (let i = 0; i < gridLines.value.content.columnCount.value; i++) {
    rule[i] = '<div class="grid-line"></div>'
  }
  return rule
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  padding: 2px;
}
.smol-breakout-grid,
.grid-lines {
  --padding-inline: 1rem;
  --content-max-width: v-bind(gridLines.content.width.value + 'px');
  --breakout-max-width: v-bind(gridLines.maxWidth.value + 'px');
  gap: v-bind(inputGap + 'px');
  --one-column: calc(
      (100% - (var(--padding-inline) * 2)) /
        v-bind(gridLines.content.columnCount.value)
    ),
    calc(var(--content-max-width) / v-bind(gridLines.content.columnCount.value));

  --breakout-size: v-bind(gridLines.breakout.width.value + 'px');

  --site-columns: v-bind(columnsRule);

  display: grid;
  grid-template-columns: var(--site-columns);
  width: min(100% - 2rem, var(--breakout-grid-width));
  row-gap: 1rem;
  margin: 5vb auto;
}

.smol-breakout-grid > *:not(.breakout, .grid-lines) {
  grid-column: content;
  border: 1px solid black;
}

.smol-breakout-grid > .breakout {
  grid-column: grid;
  border: 1px solid red;
}

.grid-lines {
  position: absolute;
  inset: 2px;
  pointer-events: none;
}
.grid-line {
  height: 100%;
  border-color: rgba(238, 0, 255, 0.342);
  border-style: dashed;
  border-width: 1px;
}
</style>
