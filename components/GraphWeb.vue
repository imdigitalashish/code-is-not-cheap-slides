<script setup lang="ts">
withDefaults(defineProps<{ step?: number }>(), { step: 0 })

// hand-placed layout in a 900 x 420 space
const nodes = [
  { id: 'Spec',   x: 450, y: 210, step: 0, hub: true },
  { id: 'API',    x: 268, y: 96,  step: 0 },
  { id: 'DB',     x: 648, y: 112, step: 0 },
  { id: 'Auth',   x: 262, y: 326, step: 0 },
  { id: 'UI',     x: 116, y: 206, step: 1 },
  { id: 'Tests',  x: 472, y: 372, step: 1 },
  { id: 'Utils',  x: 408, y: 64,  step: 1 },
  { id: 'Config', x: 706, y: 300, step: 2 },
  { id: 'Cache',  x: 812, y: 172, step: 2 },
  { id: 'Queue',  x: 600, y: 50,  step: 2 },
  { id: 'Docs',   x: 648, y: 372, step: 2 },
]
const pos = Object.fromEntries(nodes.map(n => [n.id, n]))

const edges = [
  // step 0 — the clean core
  { a: 'Spec', b: 'API', step: 0 },
  { a: 'Spec', b: 'DB', step: 0 },
  { a: 'Spec', b: 'Auth', step: 0 },
  // step 1
  { a: 'API', b: 'UI', step: 1 },
  { a: 'Auth', b: 'Tests', step: 1 },
  { a: 'Utils', b: 'API', step: 1 },
  { a: 'Spec', b: 'Tests', step: 1 },
  // step 2
  { a: 'DB', b: 'Cache', step: 2 },
  { a: 'DB', b: 'Config', step: 2 },
  { a: 'API', b: 'Queue', step: 2 },
  { a: 'Tests', b: 'Docs', step: 2 },
  { a: 'DB', b: 'Queue', step: 2 },
  // step 3 — the tangle
  { a: 'UI', b: 'Tests', step: 3, t: true },
  { a: 'Utils', b: 'Config', step: 3, t: true },
  { a: 'Cache', b: 'Queue', step: 3, t: true },
  { a: 'Auth', b: 'Config', step: 3, t: true },
  { a: 'UI', b: 'Utils', step: 3, t: true },
  { a: 'Docs', b: 'Spec', step: 3, t: true },
  { a: 'Cache', b: 'Config', step: 3, t: true },
  { a: 'Queue', b: 'Utils', step: 3, t: true },
  { a: 'API', b: 'DB', step: 3, t: true },
  { a: 'Auth', b: 'DB', step: 3, t: true },
  { a: 'Tests', b: 'Config', step: 3, t: true },
]
</script>

<template>
  <div class="gw">
    <svg viewBox="0 0 900 420" class="gw-svg">
      <line
        v-for="(e, i) in edges"
        :key="i"
        class="edge"
        :class="{ drawn: step >= e.step, tangle: e.t }"
        :x1="pos[e.a].x" :y1="pos[e.a].y"
        :x2="pos[e.b].x" :y2="pos[e.b].y"
        pathLength="1"
        :style="{ transitionDelay: (0.04 * (i % 6)) + 's' }"
      />
    </svg>

    <div
      v-for="n in nodes"
      :key="n.id"
      class="node"
      :class="{ shown: step >= n.step, hub: n.hub }"
      :style="{ left: (n.x / 900 * 100) + '%', top: (n.y / 420 * 100) + '%' }"
    ></div>

    <div class="gw-note" :class="{ shown: step >= 3 }">
      Every new piece wires into the rest — the web <span class="r">tangles</span>.
    </div>
  </div>
</template>

<style scoped>
.gw {
  position: relative;
  width: 900px;
  height: 420px;
  margin: 1rem auto 0;
}
.gw-svg { position: absolute; inset: 0; width: 100%; height: 100%; overflow: visible; }

.edge {
  stroke: #3c4256;
  stroke-width: 1.6;
  stroke-dasharray: 1;
  stroke-dashoffset: 1;
  transition: stroke-dashoffset 0.55s ease;
}
.edge.drawn { stroke-dashoffset: 0; }
.edge.tangle { stroke: #f7768e; stroke-width: 1.4; opacity: 0.55; }

.node {
  position: absolute;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: #7aa2f7;
  border: 3px solid #0b0b0c;
  box-shadow: 0 0 0 1px #7aa2f7, 0 0 14px rgba(122, 162, 247, 0.35);
  transform: translate(-50%, -50%) scale(0.4);
  opacity: 0;
  transition: opacity 0.4s ease, transform 0.4s cubic-bezier(0.22, 1, 0.36, 1);
}
.node.shown { opacity: 1; transform: translate(-50%, -50%) scale(1); }
.node.hub {
  width: 24px;
  height: 24px;
  background: #ff9e64;
  box-shadow: 0 0 0 1px #ff9e64, 0 0 18px rgba(255, 158, 100, 0.45);
}

.gw-note {
  position: absolute;
  left: 0; right: 0;
  bottom: -10px;
  text-align: center;
  font-size: 1rem;
  color: #b4b4b9;
  opacity: 0;
  transition: opacity 0.5s ease;
}
.gw-note.shown { opacity: 1; }
.gw-note .r { color: #f7768e; font-weight: 600; }
</style>
