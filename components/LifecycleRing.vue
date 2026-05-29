<script setup lang="ts">
withDefaults(defineProps<{ step?: number }>(), { step: 0 })
</script>

<template>
  <div class="ring">
    <!-- nodes -->
    <div class="node n-top shown">Define the spec</div>
    <div class="node n-right" :class="{ shown: step >= 1 }">Plan</div>
    <div class="node n-bottom" :class="{ shown: step >= 2 }">Review</div>
    <div class="node n-left" :class="{ shown: step >= 3 }">Push</div>

    <!-- clockwise arrows -->
    <div class="arrow a-tr" :class="{ shown: step >= 1 }">↘</div>
    <div class="arrow a-rb" :class="{ shown: step >= 2 }">↙</div>
    <div class="arrow a-bl" :class="{ shown: step >= 3 }">↖</div>
    <div class="arrow a-lt" :class="{ shown: step >= 4 }">↗</div>

    <!-- center question -->
    <div class="center" :class="{ shown: step >= 4 }">
      <div class="q">so why is this<br />happening?</div>
    </div>
  </div>
</template>

<style scoped>
.ring {
  position: relative;
  width: 520px;
  height: 330px;
  margin: 1.5rem auto 0;
}
.node {
  position: absolute;
  width: 150px;
  text-align: center;
  padding: 0.55rem 1rem;
  border-radius: 9px;
  background: #14151a;
  border: 1px solid #222225;
  border-left: 4px solid #7aa2f7;
  font-size: 1.02rem;
  font-weight: 500;
  color: #ededed;
  transform: translate(-50%, -50%) translateY(8px);
  opacity: 0;
  transition: opacity 0.45s ease, transform 0.45s ease;
}
.node.shown { opacity: 1; transform: translate(-50%, -50%); }
.n-top    { left: 260px; top: 34px; }
.n-right  { left: 440px; top: 165px; }
.n-bottom { left: 260px; top: 296px; }
.n-left   { left: 80px;  top: 165px; }

.arrow {
  position: absolute;
  font-size: 1.7rem;
  color: #6a6a6f;
  transform: translate(-50%, -50%);
  opacity: 0;
  transition: opacity 0.4s ease;
}
.arrow.shown { opacity: 1; }
.a-tr { left: 375px; top: 92px; }
.a-rb { left: 375px; top: 238px; }
.a-bl { left: 145px; top: 238px; }
.a-lt { left: 145px; top: 92px; }

.center {
  position: absolute;
  left: 260px;
  top: 165px;
  transform: translate(-50%, -50%) scale(0.92);
  width: 190px;
  text-align: center;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
}
.center.shown { opacity: 1; transform: translate(-50%, -50%) scale(1); }
.center .q {
  font-size: 1.35rem;
  font-weight: 600;
  color: #ff9e64;
  line-height: 1.25;
}
</style>
