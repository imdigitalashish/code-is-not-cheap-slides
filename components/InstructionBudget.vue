<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

const lines = [
  '# SYSTEM PROMPT',
  '01  Ask clarifying questions',
  '02  Follow the style guide',
  '03  Write tests first',
  '04  Keep functions small',
  '05  No new dependencies',
  '06  Update the changelog',
  '07  Handle errors explicitly',
  '08  Keep the PR small',
  '…  +192 more',
]
const full = lines.join('\n')
const typed = ref('')
const mounted = ref(false)
const progress = computed(() => (full.length ? typed.value.length / full.length : 0))
let i = 0
let timer: number | undefined
let pause: number | undefined

function tick() {
  if (i <= full.length) {
    typed.value = full.slice(0, i)
    i++
  } else {
    window.clearInterval(timer)
    pause = window.setTimeout(() => {
      i = 0
      timer = window.setInterval(tick, 26)
    }, 2000)
  }
}

onMounted(() => {
  mounted.value = true
  timer = window.setInterval(tick, 26)
})
onBeforeUnmount(() => {
  window.clearInterval(timer)
  window.clearTimeout(pause)
})
</script>

<template>
  <div class="ib" :class="{ in: mounted }">
    <!-- LEFT: the writing animation -->
    <div class="ib-left">
      <div class="ib-cap">You write the prompt</div>
      <div class="editor">
        <div class="bar"><span></span><span></span><span></span></div>
        <pre class="code">{{ typed }}<span class="cursor">▍</span></pre>
      </div>
    </div>

    <!-- RIGHT: U-shaped attention, rotated 90° -->
    <div class="ib-right">
      <div class="ib-cap">…the model attends like this</div>
      <svg viewBox="0 0 300 330" class="ib-svg">
        <defs>
          <linearGradient id="ibgrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#57a64a" />
            <stop offset="30%" stop-color="#57a64a" />
            <stop offset="50%" stop-color="#f7768e" />
            <stop offset="70%" stop-color="#57a64a" />
            <stop offset="100%" stop-color="#57a64a" />
          </linearGradient>
        </defs>

        <!-- context axis (position: top = start, bottom = end) -->
        <line x1="44" y1="26" x2="44" y2="300" stroke="#3a3a40" stroke-width="1.5" />

        <!-- rotated-U attention curve — grows downward with the typing -->
        <path class="curve" :style="{ strokeDashoffset: 1 - progress }"
              d="M250,30 C 80,110 80,216 250,296" pathLength="1" />

        <!-- labels -->
        <text class="ax" x="40" y="20" text-anchor="end">start</text>
        <text class="ax" x="40" y="312" text-anchor="end">end</text>
        <text class="lab g" :class="{ show: progress > 0.12 }" x="210" y="62" text-anchor="end">remembered</text>
        <text class="lab r" :class="{ show: progress > 0.5 }" x="140" y="168">forgotten</text>
        <text class="lab g" :class="{ show: progress > 0.92 }" x="210" y="280" text-anchor="end">remembered</text>
      </svg>
    </div>
  </div>
</template>

<style scoped>
.ib {
  display: flex;
  gap: 56px;
  align-items: stretch;
  justify-content: center;
  width: 880px;
  margin: 1.3rem auto 0;
}
.ib-cap {
  font-size: 0.8rem;
  color: #8a8a8f;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  margin-bottom: 0.6rem;
}

/* left editor */
.ib-left { width: 440px; }
.editor {
  height: 300px;
  background: #0f1016;
  border: 1px solid #222225;
  border-radius: 10px;
  overflow: hidden;
}
.bar {
  display: flex;
  gap: 7px;
  padding: 0.7rem 0.9rem;
  border-bottom: 1px solid #1c1c22;
}
.bar span { width: 10px; height: 10px; border-radius: 50%; background: #2a2a31; }
.code {
  margin: 0;
  padding: 0.9rem 1.1rem;
  font-family: 'JetBrains Mono', ui-monospace, monospace;
  font-size: 0.92rem;
  line-height: 1.55;
  color: #d4d4d8;
  white-space: pre-wrap;
}
.cursor {
  color: #7dcfff;
  animation: blink 1s steps(1) infinite;
}
@keyframes blink { 50% { opacity: 0; } }

/* right curve */
.ib-right { width: 300px; }
.ib-svg { width: 100%; height: auto; display: block; }
.curve {
  fill: none;
  stroke: url(#ibgrad);
  stroke-width: 6;
  stroke-linecap: round;
  stroke-dasharray: 1;
  stroke-dashoffset: 1;
  transition: stroke-dashoffset 0.12s linear;
}
.ax { fill: #8a8a8f; font-size: 12px; }
.lab { font-size: 13px; font-weight: 600; opacity: 0; transition: opacity 0.4s ease; }
.lab.show { opacity: 1; }
.lab.g { fill: #57a64a; }
.lab.r { fill: #f7768e; font-size: 15px; }
</style>
