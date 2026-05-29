<script setup lang="ts">
withDefaults(defineProps<{ step?: number }>(), { step: 0 })
</script>

<template>
  <div class="wf-chart">
    <!-- dashed connectors -->
    <div class="conn c1" :class="{ shown: step >= 2 }"></div>
    <div class="conn c2" :class="{ shown: step >= 3 }"></div>

    <div class="wf-plot">
      <div class="wf-col">
        <div class="wf-val" :class="{ shown: step >= 1 }" style="bottom: 236px">30–40%</div>
        <div class="wf-bar bar-green" :class="{ shown: step >= 1 }" style="height: 230px; bottom: 0"></div>
        <div class="wf-x" :class="{ shown: step >= 1 }">New Code</div>
      </div>

      <div class="wf-col">
        <div class="wf-bar bar-red" :class="{ shown: step >= 2 }" style="height: 108px; bottom: 122px">
          <div class="wf-val-in">15–25%</div>
        </div>
        <div class="wf-x" :class="{ shown: step >= 2 }">Rework</div>
      </div>

      <div class="wf-col">
        <div class="wf-val" :class="{ shown: step >= 3 }" style="bottom: 132px">15–20%</div>
        <div class="wf-bar bar-blue" :class="{ shown: step >= 3 }" style="height: 126px; bottom: 0"></div>
        <div class="wf-x" :class="{ shown: step >= 3 }">Net Productivity<br />Gains from AI</div>
      </div>
    </div>

    <div class="wf-callout co-newcode" :class="{ shown: step >= 1 }">
      AI makes it easy to generate &amp; understand code
    </div>
    <div class="wf-callout co-rework" :class="{ shown: step >= 2 }">
      However, a lot of that new code has bugs and must be re-worked
    </div>
    <div class="wf-callout co-net" :class="{ shown: step >= 3 }">
      The net overall software engineering productivity gains from AI are <strong>~15–20%</strong>
    </div>
  </div>
</template>

<style scoped>
.wf-chart {
  position: relative;
  width: 880px;
  height: 340px;
  margin: 0.8rem auto 0;
}
.wf-plot {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 50px;
  display: flex;
  gap: 80px;
  align-items: flex-end;
  height: 230px;
  border-bottom: 2px solid #3a3a40;
}
.wf-col { position: relative; width: 76px; height: 100%; }

.wf-bar {
  position: absolute;
  left: 0;
  width: 100%;
  border-radius: 3px;
  transform: scaleY(0);
  transform-origin: bottom;
  opacity: 0;
  transition: transform 0.6s cubic-bezier(0.22, 1, 0.36, 1), opacity 0.3s ease;
}
.wf-bar.shown { transform: scaleY(1); opacity: 1; }
.bar-green { background: #57a64a; }
.bar-red   { background: #c43d2f; }
.bar-blue  { background: #3b5169; }

.wf-val {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
  font-size: 0.88rem;
  font-weight: 700;
  color: #ededed;
  opacity: 0;
  transition: opacity 0.4s ease 0.25s;
}
.wf-val.shown { opacity: 1; }
.wf-val-in {
  position: absolute;
  top: 50%;
  left: 0; right: 0;
  transform: translateY(-50%);
  text-align: center;
  font-size: 0.88rem;
  font-weight: 700;
  color: #fff;
}
.wf-x {
  position: absolute;
  top: calc(100% + 10px);
  left: 50%;
  transform: translateX(-50%);
  width: 140px;
  text-align: center;
  font-size: 0.76rem;
  font-weight: 600;
  color: #ededed;
  line-height: 1.2;
  opacity: 0;
  transition: opacity 0.4s ease 0.25s;
}
.wf-x.shown { opacity: 1; }

.conn {
  position: absolute;
  border-top: 2px dashed #6a6a6f;
  opacity: 0;
  transition: opacity 0.4s ease;
}
.conn.shown { opacity: 1; }
.c1 { left: 322px; top: 60px; width: 80px; }
.c2 { left: 478px; top: 166px; width: 80px; }

.wf-callout {
  position: absolute;
  width: 180px;
  background: #6e1717;
  color: #f4e6e6;
  font-size: 0.78rem;
  line-height: 1.3;
  padding: 0.6rem 0.75rem;
  border-radius: 8px;
  opacity: 0;
  transition: opacity 0.45s ease;
}
.wf-callout.shown { opacity: 1; }
.wf-callout::after {
  content: "";
  position: absolute;
  border: 8px solid transparent;
}
.co-newcode { left: 0; top: 70px; text-align: right; }
.co-newcode::after { right: -14px; top: 22px; border-left-color: #6e1717; }
.co-rework { right: 0; top: 48px; }
.co-rework::after { left: -14px; top: 22px; border-right-color: #6e1717; }
.co-net { right: 0; bottom: 56px; }
.co-net::after { left: -14px; top: 26px; border-right-color: #6e1717; }
</style>
