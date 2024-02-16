<template>
  <div class="grid-lines">
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
  </div>
  <main class="smol-breakout-grid"></main>
</template>
<script setup>
import { ref, computed } from 'vue'
const columns = ref(null)

const gridLines = ref({
  content: {
    width: 900,
    columnCount: 3,
  },
  breakout: {
    width: 100,
  },
  maxWidth: 1400,
})

const columnsRule = computed(() => {
  return `repeat(${gridLines.value.content.columnCount}, --one-column)`
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.smol-breakout-grid,
.grid-lines {
  --padding-inline: 1rem;
  --content-max-width: v-bind(gridLines.content.width + 'px');
  --breakout-max-width: v-bind(gridLines.maxWidth + 'px');
  --one-column: calc((100% - (var(--padding-inline) * 2)) / 3),
    calc(var(--content-max-width) / 3);
  /* prettier-ignore */
  --columns: 
    min(var(--one-column)) 
    [block-left-end wide-left-end]
    min(var(--one-column)) 
    [wide-right-start] 
    min(var(--one-column));
  /* prettier */
  --breakout-size: v-bind(gridLines.breakout.width + 'px');
  /* prettier-ignore */
  --site-columns:
  [max-width-area-start]
    minmax(var(--padding-inline), 1fr)
      [wide-start wide-left-start]
      minmax(0, var(--breakout-size))
        [content-block-start]
          var(--columns)
        [content-block-end]
      minmax(0, var(--breakout-size))
      [wide-end wide-right-end]
      minmax(var(--padding-inline), 1fr)
  [max-width-area-end];
  /* prettier */

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
  inset: 0;
  pointer-events: none;
}
.grid-line {
  height: 100%;
  border-color: rgba(238, 0, 255, 0.342);
  border-style: dashed;
  border-width: 1px;
}
</style>
