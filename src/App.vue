<script setup>
import { ref } from 'vue'
const columns = ref(null)

const gridLines = ref({
  content: {
    width: 900,
    columnCount: 3,
  },
  breakout: {
    width: +100,
  },
  maxWidth: 1400,
})
</script>

<template>
  <div class="grid-lines">
    <div class="grid-line"></div>
    <div class="grid-line"></div>
    <div class="grid-line"></div>
  </div>
  <main class="smol-breakout-grid"></main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.smol-breakout-grid,
.grid-lines {
  --max-content-width: 50ch;
  --breakout-difference: 0.2;

  /*  Compute total allowed grid width to `--breakout-difference` 
      larger than content area  */
  --breakout-grid-width: calc(
    var(--max-content-width) +
      (var(--max-content-width) * var(--breakout-difference))
  );

  display: grid;
  grid-template-columns:
    [grid-start] 1fr
    [content-start]
    v-bind('gridLines.content.width')
    [breakout]
    v-bind('gridLines.breakout.width')
    minmax(min(100%, var(--max-content-width)), 1fr)
    [content-end] 1fr
    [grid-end];
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
