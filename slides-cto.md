---
theme: default
title: Self-hosting LLMs — what I'd tell another CTO
info: |
  CTO cut of the meetup talk — 2 Sept 2026
  Speaker: Dan Harper
class: text-center
---

# Self-hosting LLMs

<div class="text-xl opacity-70 font-normal mt-1">what I'd tell another CTO</div>

<div class="flex items-center justify-center gap-10 mt-8">
  <img src="/dan-harper.jpg" class="rounded-full w-40 h-40 object-cover shadow-lg shrink-0" alt="Dan Harper" />
  <div class="text-left leading-relaxed">
    <div class="text-4xl font-bold">Dan Harper</div>
    <div class="text-xl mt-2 opacity-80">CTO @ AskYourTeam</div>
    <a href="https://x.com/dan_harper" target="_blank" class="inline-flex items-center gap-2 mt-4 text-xl font-semibold !border-none">
      <img src="/x-logo.svg" class="w-6 h-6" alt="X" />
      <span>@dan_harper</span>
    </a>
  </div>
</div>

<!--
Not the war story. The decisions I'd make differently.

Ten seconds. Then into what mattered.
-->

---
layout: center
class: text-center
---

# Why leave the API

<img src="/why-leave-api.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Privacy
Sovereignty
Custom models
Freedom
Pricing*
-->

---
layout: center
class: text-center
---

# The cloud won't save you

<img src="/hyperscalers.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
99% of models infer overseas.

No decent GPUs in Australia. Ancient cards. Arse end of the world.

Agentic loop. Mind-blowing. 50x use.

Hyperscalers: nope. Bedrock sucked. EC2 sucked worse.
-->

---
layout: center
class: text-center
---

# Bare metal is a commitment

<img src="/bare-metal-2am.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
12-month commit.

GPUs in 2 to 52 weeks.

Not elastic. Four fixed cards.

Happy days: A100 40 GB + vLLM. Flakiness vanished. Scaled far better.
-->

---
layout: center
class: text-center
---

# "Does it fit?" is a planning problem

<img src="/does-it-fit-vram.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Simplest question. Internet gives a simple answer.

Reality is very complicated.

Budget time and people for sizing.

Q&A if asked: weights, KV, engine, quant — a pile of variables.
-->

---
layout: center
class: text-center
---

# Small, Fast AND Smart

<img src="/small-fast-smart.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Don't put the big model on every row. 200,000 calls. Weeks of LLM.

Fine-tune a tiny model. Gemma 4 E2B. LoRA. 1,200 records. 12 hours.

Eval loop. Then 128 in flight. Stupidly fast.

Long game: capture traces, fine-tune small models, swap out expensive LLM calls.

That's the cost lever.
-->

---
layout: center
class: text-center
---

# Three don'ts

<img src="/happy-days.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Don't trust the small box.

Don't send 200,000 cells through the big model.

Don't do my first six ideas.
-->

---
layout: center
class: text-center
---

# Questions

<img src="/questions.png" class="max-h-80 mx-auto rounded-lg mt-2" alt="" />

<a href="https://x.com/dan_harper" target="_blank" class="inline-flex items-center gap-2 mt-4 text-2xl font-semibold !border-none">
  <img src="/x-logo.svg" class="w-7 h-7" alt="" />
  <span>@dan_harper</span>
</a>

<!--
Leave this up. Don't fill silence with a new chapter.

They can follow on X if they want more of this.
-->
