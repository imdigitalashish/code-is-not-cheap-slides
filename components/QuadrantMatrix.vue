<script setup lang="ts">
interface Cell { pct: string; desc: string }
withDefaults(defineProps<{
  tl: Cell
  tr: Cell
  bl: Cell
  br: Cell
  caption?: string
  subcaption?: string
}>(), {
  caption: 'Software Engineering Productivity Increases from AI Use',
  subcaption: 'Orientative Guidelines',
})
</script>

<template>
  <div class="qm">
    <div class="qm-cap">
      <div class="c1">{{ caption }}</div>
      <div class="c2">{{ subcaption }}</div>
    </div>

    <div class="qm-main">
      <div class="qm-ylabel">Task Complexity</div>
      <div class="qm-yticks"><span>High</span><span>Low</span></div>

      <div class="qm-right">
        <div class="qm-grid">
          <div class="qm-cell tl"><div class="pct">{{ tl.pct }}</div><div class="desc">{{ tl.desc }}</div></div>
          <div class="qm-cell tr"><div class="pct">{{ tr.pct }}</div><div class="desc">{{ tr.desc }}</div></div>
          <div class="qm-cell bl"><div class="pct">{{ bl.pct }}</div><div class="desc">{{ bl.desc }}</div></div>
          <div class="qm-cell br"><div class="pct">{{ br.pct }}</div><div class="desc">{{ br.desc }}</div></div>
        </div>
        <div class="qm-xticks"><span>Greenfield</span><span>Brownfield</span></div>
        <div class="qm-xlabel">Project Maturity</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.qm { width: 760px; margin: 0.4rem auto 0; }
.qm-cap { text-align: center; margin-bottom: 0.7rem; }
.qm-cap .c1 { font-size: 1.05rem; font-weight: 600; color: #ededed; }
.qm-cap .c2 { font-size: 0.76rem; color: #8a8a8f; margin-top: 0.1rem; }

.qm-main { display: flex; align-items: stretch; justify-content: center; }
.qm-ylabel {
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  font-size: 0.82rem;
  font-weight: 600;
  color: #ededed;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 270px;
  padding-right: 2px;
}
.qm-yticks {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  height: 270px;
  padding-right: 10px;
  font-size: 0.74rem;
  color: #8a8a8f;
}
.qm-grid {
  box-sizing: border-box;
  width: 620px;
  height: 270px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  border-left: 2px solid #5a5a60;
  border-bottom: 2px solid #5a5a60;
}
.qm-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 0.8rem 1.3rem;
}
.qm-cell.tl { border-right: 1px dashed #4a4a50; border-bottom: 1px dashed #4a4a50; }
.qm-cell.tr { border-bottom: 1px dashed #4a4a50; }
.qm-cell.bl { border-right: 1px dashed #4a4a50; }
.pct { font-size: 1.35rem; font-weight: 700; color: #ededed; margin-bottom: 0.35rem; }
.desc { font-size: 0.82rem; color: #b4b4b9; line-height: 1.3; }

.qm-xticks {
  display: flex;
  justify-content: space-around;
  width: 620px;
  margin-top: 8px;
  font-size: 0.8rem;
  color: #ededed;
}
.qm-xlabel {
  width: 620px;
  text-align: center;
  margin-top: 4px;
  font-size: 0.82rem;
  font-weight: 600;
  color: #ededed;
}
</style>
