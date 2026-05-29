<script setup lang="ts">
withDefaults(defineProps<{ step?: number }>(), { step: 0 })
</script>

<template>
  <div class="dz">
    <div class="bar-wrap">
      <!-- 64k marker -->
      <div class="marker" :class="{ shown: step >= 2 }">
        <div class="m-lab">64k tokens<br /><span class="r">quality falls off a cliff</span></div>
        <div class="m-line"></div>
      </div>

      <div class="bar">
        <div class="seg s-sharp" :class="{ grow: step >= 1 }" style="flex-basis: 40%">
          <div class="z-name g">Sharp</div>
          <div class="z-rng">0–40%</div>
        </div>
        <div class="seg s-fuzzy" :class="{ grow: step >= 1 }" style="flex-basis: 20%; transition-delay: 0.12s">
          <div class="z-name y">Fuzzy</div>
          <div class="z-rng">40–60%</div>
        </div>
        <div class="seg s-dumb" :class="{ grow: step >= 1 }" style="flex-basis: 40%; transition-delay: 0.24s">
          <div class="z-name r">Dumb zone</div>
          <div class="z-rng">60–100%</div>
        </div>
      </div>

      <div class="ticks">
        <span style="left: 0%">0</span>
        <span style="left: 40%">80k</span>
        <span style="left: 60%">120k</span>
        <span style="left: 100%; transform: translateX(-100%)">200k tokens</span>
      </div>
    </div>

    <!-- low-fill stats -->
    <div class="stats" :class="{ shown: step >= 3 }">
      <div class="s-cap">At 8% fill — just 16k / 200k tokens</div>
      <div class="s-row">
        <div class="sc"><div class="n g">94%</div><div class="l">Task success</div></div>
        <div class="sc"><div class="n g">96%</div><div class="l">Instruction recall</div></div>
        <div class="sc"><div class="n r">4%</div><div class="l">Omission rate</div></div>
        <div class="sc"><div class="n r">2%</div><div class="l">Contradiction rate</div></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dz { width: 840px; margin: 1.4rem auto 0; }

.bar-wrap { position: relative; padding-top: 58px; }
.bar { display: flex; height: 76px; border-radius: 8px; overflow: hidden; }
.seg {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.55s cubic-bezier(0.22, 1, 0.36, 1);
}
.seg.grow { transform: scaleX(1); }
.s-sharp { background: rgba(87, 166, 74, 0.20); }
.s-fuzzy { background: rgba(224, 175, 104, 0.20); }
.s-dumb  { background: rgba(247, 118, 142, 0.22); }
.z-name { font-size: 1.05rem; font-weight: 700; }
.z-rng { font-size: 0.78rem; color: #8a8a8f; margin-top: 0.1rem; }
.g { color: #9ece6a; }
.y { color: #e0af68; }
.r { color: #f7768e; }

.ticks { position: relative; height: 1.4rem; margin-top: 0.4rem; }
.ticks span {
  position: absolute;
  font-size: 0.74rem;
  color: #6a6a6f;
  font-variant-numeric: tabular-nums;
}

.marker {
  position: absolute;
  left: 32%;
  top: 0;
  bottom: 32px;
  opacity: 0;
  transform: translateY(-6px);
  transition: opacity 0.45s ease, transform 0.45s ease;
}
.marker.shown { opacity: 1; transform: translateY(0); }
.m-line { position: absolute; top: 50px; bottom: 0; width: 2px; background: #f7768e; }
.m-lab {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
  text-align: center;
  font-size: 0.82rem;
  font-weight: 600;
  color: #ededed;
  line-height: 1.25;
}

.stats { margin-top: 2rem; opacity: 0; transition: opacity 0.5s ease; }
.stats.shown { opacity: 1; }
.s-cap { text-align: center; font-size: 0.82rem; color: #8a8a8f; margin-bottom: 0.8rem; }
.s-row { display: flex; gap: 1.5rem; justify-content: center; }
.sc {
  flex: 1;
  max-width: 180px;
  text-align: center;
  padding: 1rem 0.8rem;
  background: #121214;
  border: 1px solid #222225;
  border-radius: 10px;
}
.sc .n { font-size: 1.9rem; font-weight: 700; line-height: 1; }
.sc .l { font-size: 0.8rem; color: #b4b4b9; margin-top: 0.45rem; }
</style>
