<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

const props = withDefaults(defineProps<{ opacity?: number; speed?: number }>(), {
  opacity: 1,
  speed: 1,
})

const canvas = ref<HTMLCanvasElement | null>(null)
let raf = 0
let ro: ResizeObserver | null = null

const VERT = `#version 300 es
in vec2 a_pos;
void main() { gl_Position = vec4(a_pos, 0.0, 1.0); }`

const FRAG = `#version 300 es
precision highp float;
out vec4 fragColor;
uniform vec2 u_res;
uniform float u_time;

vec2 hash2(vec2 p){
  p = vec2(dot(p, vec2(127.1, 311.7)), dot(p, vec2(269.5, 183.3)));
  return -1.0 + 2.0 * fract(sin(p) * 43758.5453123);
}
float noise(vec2 p){
  vec2 i = floor(p); vec2 f = fract(p);
  vec2 u = f * f * (3.0 - 2.0 * f);
  return mix(mix(dot(hash2(i + vec2(0,0)), f - vec2(0,0)),
                 dot(hash2(i + vec2(1,0)), f - vec2(1,0)), u.x),
             mix(dot(hash2(i + vec2(0,1)), f - vec2(0,1)),
                 dot(hash2(i + vec2(1,1)), f - vec2(1,1)), u.x), u.y);
}
float fbm(vec2 p){
  float v = 0.0, a = 0.5;
  for (int i = 0; i < 6; i++){ v += a * noise(p); p *= 2.0; a *= 0.5; }
  return v;
}
void main(){
  vec2 uv = gl_FragCoord.xy / u_res.xy;
  vec2 p = uv * 3.0;
  p.x *= u_res.x / u_res.y;
  float t = u_time * 0.05;
  vec2 q = vec2(fbm(p + vec2(0.0, t)), fbm(p + vec2(5.2, 1.3) + t));
  vec2 r = vec2(fbm(p + 4.0 * q + vec2(1.7, 9.2) + t * 0.5),
                fbm(p + 4.0 * q + vec2(8.3, 2.8)));
  float f = fbm(p + 4.0 * r);

  vec3 c1 = vec3(0.047, 0.055, 0.086); // #0c0e16
  vec3 c2 = vec3(0.478, 0.635, 0.969); // #7aa2f7
  vec3 c3 = vec3(0.733, 0.604, 0.969); // #bb9af7
  vec3 c4 = vec3(1.000, 0.620, 0.392); // #ff9e64

  vec3 col = mix(c1, c2, clamp(f * f * 1.6, 0.0, 1.0));
  col = mix(col, c3, clamp(length(q) * 0.6, 0.0, 1.0));
  col = mix(col, c4, clamp(r.x * r.x * 0.5, 0.0, 1.0));
  col *= 0.55;
  col = mix(c1, col, 0.92);
  fragColor = vec4(col, 1.0);
}`

function compile(gl: WebGL2RenderingContext, type: number, src: string) {
  const sh = gl.createShader(type)!
  gl.shaderSource(sh, src)
  gl.compileShader(sh)
  if (!gl.getShaderParameter(sh, gl.COMPILE_STATUS))
    console.error('shader error:', gl.getShaderInfoLog(sh))
  return sh
}

onMounted(() => {
  const cv = canvas.value!
  const gl = cv.getContext('webgl2', { antialias: true, alpha: false })
  if (!gl) { console.warn('WebGL2 not available'); return }

  const prog = gl.createProgram()!
  gl.attachShader(prog, compile(gl, gl.VERTEX_SHADER, VERT))
  gl.attachShader(prog, compile(gl, gl.FRAGMENT_SHADER, FRAG))
  gl.linkProgram(prog)
  gl.useProgram(prog)

  const buf = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, buf)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([-1, -1, 3, -1, -1, 3]), gl.STATIC_DRAW)
  const loc = gl.getAttribLocation(prog, 'a_pos')
  gl.enableVertexAttribArray(loc)
  gl.vertexAttribPointer(loc, 2, gl.FLOAT, false, 0, 0)

  const uRes = gl.getUniformLocation(prog, 'u_res')
  const uTime = gl.getUniformLocation(prog, 'u_time')

  const resize = () => {
    const dpr = Math.min(window.devicePixelRatio || 1, 2)
    const w = Math.max(1, Math.floor(cv.clientWidth * dpr))
    const h = Math.max(1, Math.floor(cv.clientHeight * dpr))
    if (cv.width !== w || cv.height !== h) {
      cv.width = w; cv.height = h
      gl.viewport(0, 0, w, h)
    }
  }
  ro = new ResizeObserver(resize)
  ro.observe(cv)
  resize()

  const start = performance.now()
  const loop = () => {
    resize()
    gl.uniform2f(uRes, cv.width, cv.height)
    gl.uniform1f(uTime, ((performance.now() - start) / 1000) * props.speed)
    gl.drawArrays(gl.TRIANGLES, 0, 3)
    raf = requestAnimationFrame(loop)
  }
  loop()
})

onBeforeUnmount(() => {
  cancelAnimationFrame(raf)
  ro?.disconnect()
})
</script>

<template>
  <canvas
    ref="canvas"
    class="absolute inset-0 w-full h-full"
    style="z-index: 0"
    :style="{ opacity }"
  />
</template>
