<script setup lang="ts">
withDefaults(defineProps<{ step?: number }>(), { step: 0 })
</script>

<template>
  <div class="loop">
    <div class="node a-blue shown">You give AI a feature</div>

    <div class="arr" :class="{ shown: step >= 1 }"><span class="ar">↓</span></div>
    <div class="node a-green" :class="{ shown: step >= 1 }">It produces code</div>

    <div class="arr" :class="{ shown: step >= 2 }"><span class="al">you iterate</span><span class="ar">↓</span></div>
    <div class="node a-orange" :class="{ shown: step >= 2 }">It produces bad code</div>

    <div class="arr" :class="{ shown: step >= 3 }"><span class="al">you iterate again</span><span class="ar">↓</span></div>
    <div class="node a-red" :class="{ shown: step >= 3 }">It produces worse code</div>
  </div>
</template>

<style scoped>
.loop {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.35rem;
  margin-top: 1.4rem;
}
.node {
  width: 360px;
  text-align: center;
  padding: 0.7rem 1.2rem;
  border-radius: 9px;
  background: #14151a;
  border: 1px solid #222225;
  border-left: 4px solid #565f89;
  font-size: 1.08rem;
  font-weight: 500;
  color: #ededed;
  opacity: 0;
  transform: translateY(10px);
  transition: opacity 0.45s ease, transform 0.45s ease;
}
.node.shown { opacity: 1; transform: translateY(0); }
.a-blue   { border-left-color: #7aa2f7; }
.a-green  { border-left-color: #57a64a; }
.a-orange { border-left-color: #ff9e64; color: #f0d9c4; }
.a-red    { border-left-color: #f7768e; color: #f1c7cd; }

.arr {
  position: relative;
  height: 1.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.4s ease;
}
.arr.shown { opacity: 1; }
.arr .ar { color: #6a6a6f; font-size: 1.2rem; line-height: 1; }
.arr .al {
  position: absolute;
  left: calc(50% + 16px);
  font-size: 0.8rem;
  font-style: italic;
  color: #8a8a8f;
  white-space: nowrap;
}
</style>
