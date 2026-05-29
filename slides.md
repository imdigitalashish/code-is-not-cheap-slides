---
theme: default
title: Code Is Not Cheap
info: Slop in the AI Era
author: Ashish Kumar Verma
fonts:
  sans: Inter
  mono: JetBrains Mono
  weights: '300,400,500,600'
transition: fade
mdc: true
drawings:
  persist: false
---

<div class="h-full flex flex-col items-center justify-center text-center">
  <div class="title">Code Is Not Cheap</div>
  <div class="subtitle mt-5">Slop in the AI Era</div>
  <div class="absolute bottom-12 text-sm faint tracking-wide">Ashish Kumar Verma · imdigitalashish</div>
</div>

---

# Quick Intro

<div class="muted text-base -mt-1 mb-5">Engineer · Researcher · Speaker</div>

<div class="intro-grid">

- Hosted National Program with Prime Minister
- Cracked IIT Delhi, enrolled in Engineering Physics
- Dropped out of IIT Delhi
- Published in Springer Nature's most reputed journal — youngest researcher officially (**at 20**)
- Guinness World Record — World's Largest AI Hackathon (Mentor & Judge)
- Youngest Google Developer Expert globally — Web (now AI)
- India Skills 2021, 2023, 2024 — Silver & Medallion of Excellence

- India Book of Records — Google Developer Expert
- Special Projects @ Zamana
- Panelist with the **Prime Minister of India** on the PM's YouTube channel
- Featured in Japan's Bukyo Newspaper as an India–Japan AI Innovation bridge
- Speaker at Google, Microsoft, IIT Delhi & IIT Madras — **40+** talks, mentored **30K+** students
- Corrected a fuel calculation in *Project Hail Mary* — acknowledged by author **Andy Weir**
- Hacked the **CBSE** portal and the **MSTBE** portal

</div>

<DeckFooter />

---

# Agenda

<div class="card-row mt-16">
  <div class="card" v-click>
    <div class="card-title">Industry View</div>
  </div>
  <div class="card" v-click>
    <div class="card-title">Problems in LLMs</div>
  </div>
  <div class="card" v-click>
    <div class="card-title">Engineering to Solve It</div>
  </div>
</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">What do we currently do with AI?</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Let's see some ROI with AI.</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="h-full flex items-center justify-center">

![Stanford SWEPR: Adopting AI didn't increase effective output and increased rework by 2.6x](./images/roi-stanford.png){class="rounded-lg max-h-[88vh] w-auto mx-auto"}

</div>

<DeckFooter />

---
clicks: 3
---

# Software Engineering Productivity

<div class="muted text-sm -mt-1">Boosts output, but also increases rework — net gain ~15–20%</div>

<RoiWaterfall :step="$clicks" />

<DeckFooter />

---

<div class="headline">Greenfield projects gain <strong>30–35%</strong> on simple tasks and <strong>10–15%</strong> on complex ones,<br />versus <strong>15–20%</strong> and <strong>5–10%</strong> in brownfield projects</div>

<QuadrantMatrix
  :tl="{ pct: '+10–15%', desc: 'Complex tasks require deeper human insight' }"
  :tr="{ pct: '+0–10%', desc: 'Constrained by outdated code &amp; intricate dependencies' }"
  :bl="{ pct: '+35–40%', desc: 'These tasks are often repetitive and well-defined' }"
  :br="{ pct: '+15–20%', desc: 'Legacy projects still benefit on simpler tasks' }"
/>

<DeckFooter />

---

<div class="headline">The same gains — by company stage</div>

<QuadrantMatrix
  caption="Productivity Gains from AI, by Company Stage"
  subcaption="Where does your codebase sit?"
  :tl="{ pct: '+10–15%', desc: 'Mid-size startup' }"
  :tr="{ pct: '+0–10%', desc: 'Big MNCs — codebase older than your birthday' }"
  :bl="{ pct: '+35–40%', desc: 'The new startup' }"
  :br="{ pct: '+15–20%', desc: 'Corporate' }"
/>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="h-full flex items-center justify-center">

![Stanford SWEPR: Adopting AI decreased code quality by 9% and increased variance by 3.6x](./images/code-quality.png){class="rounded-lg max-h-[88vh] w-auto mx-auto"}

</div>

<DeckFooter />

---
clicks: 4
---

# But where's the problem?

<div class="muted text-base -mt-1">We define the spec, plan, review, push — a clean lifecycle.</div>

<LifecycleRing :step="$clicks" />

<DeckFooter />

---
clicks: 3
---

# My Observation with Spec-Driven Development

<div class="muted text-base -mt-1">Start with AI, hand it a feature, and keep iterating…</div>

<SpecLoop :step="$clicks" />

<DeckFooter />

---
layout: center
class: text-center
clicks: 1
---

<div class="muted text-xl">Do this loop for 2–3 months…</div>

<div v-click class="mt-7">
  <div class="title" style="font-size: 2.6rem">You rewrite the whole thing.</div>
  <div class="mt-4 text-xl" style="color: #f7768e">The agents can't iterate on it anymore.</div>
</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Problems in the Internals of LLMs</div>

<DeckFooter />

---

# The Instruction Budget

<div class="muted text-base -mt-1">Problem #1 — models reliably follow ~150–200 instructions, then start dropping them.</div>

<InstructionBudget />

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Dumb Zone</div>

<div class="mt-5 text-base faint">
  <a href="https://www.systemsofashish.com/posts/everything-we-got-wrong-about-ai-coding-agents" target="_blank">systemsofashish.com/posts/everything-we-got-wrong-about-ai-coding-agents</a>
</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Enough of the problems.</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Step 0: Plan with your brain!</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Ralph Loop</div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Problem with <code>skill.md</code></div>

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Tips &amp; Tricks: best way to create <code>skill.md</code></div>

<DeckFooter />

---
clicks: 3
---

# Let's create a new component to fix this.

<GraphWeb :step="$clicks" />

<DeckFooter />

---
layout: center
class: text-center
---

<div class="title" style="font-size: 3rem">Any more questions?</div>

<DeckFooter />
