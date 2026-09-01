---
theme: default
title: Hosting LLMs in production and failing to scale to 10 users
info: |
  Meetup talk — 2 Sept 2026
  Speaker: Dan Harper
class: text-center
---

# Hosting LLMs in production

<div class="text-xl opacity-70 font-normal mt-1">and failing to scale to 10 users</div>

<img src="/title-one-gpu.png" class="max-h-72 mx-auto rounded-lg mt-4" alt="" />

<div class="opacity-70 mt-2">Dan Harper · 2 Sept 2026</div>

<!--
Scale joke if you want it: on the web a CPU serves a million users, a GPU often serves one.

This is a war story. I lived it. I'm making light of it. A few "don't do this" if you go this path.

Don't lecture. Don't go deep on the why-host list. Personal colour only.
-->

---
layout: center
---

<div class="flex items-center justify-center gap-16">
  <img src="/dan-harper.jpg" class="rounded-full w-56 h-56 object-cover shadow-lg shrink-0" alt="Dan Harper" />
  <div class="text-left leading-relaxed">
    <div class="text-5xl font-bold">Dan Harper</div>
    <div class="text-2xl mt-3 opacity-80">CTO @ AskYourTeam</div>
    <a href="https://x.com/dan_harper" target="_blank" class="inline-flex items-center gap-3 mt-8 text-2xl font-semibold !border-none">
      <img src="/x-logo.svg" class="w-7 h-7" alt="X" />
      <span>@dan_harper</span>
    </a>
  </div>
</div>

<!--
You're Dan Harper, CTO at AskYourTeam, Melbourne.

Point them at X if they want to follow: x.com/dan_harper

Don't over-introduce. Ten seconds, then into the war story.
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

# We're on AWS, but Bedrock sucked

<img src="/bedrock-overseas.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
99% of models infer overseas. 

No Claude.

Nova?

JSON query DSL. 

Treated the LLM like one request/response.

RAG-shaped. It Sucked.

OpenRouter next -> Qwen 35B A3B Win!
-->

---
layout: center
class: text-center
---

# We're on AWS, but EC2 sucked worse

<img src="/ec2-sucked.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Prices??? OMG

24 GB, AWS G5. Q3

Agentic loop. Mind-blowing. 50x use

Oracle Gateway.

6x EC2 Failed to scale to 3x users.

No EC2 Availability
-->

---
layout: center
class: text-center
---

# Hyperscalers cannot save you

<img src="/hyperscalers.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Google: nope.

Oracle: haha, nope.

Azure: who cares.

Ancient GPUs

Australia == Arse End Of The World
-->

---
layout: center
class: text-center
---

# The Bare Metal Fantasy

<img src="/bare-metal-2am.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Bare Metal? Options

VPS-ish? -> Micron21

12-month commit. 

GPUs in 2 to 52 weeks

Not elastic.

Four GPUs for now.
-->

---
layout: center
class: text-center
---

# Does my model fit in VRAM?

<img src="/does-it-fit-vram.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Simplest question.

The internet will give you a simple answer.

In reality the answer is very complicated.
-->

---
layout: image
image: /complexity-weights.png
---

<!--
You think VRAM is just the model. Quant is the cheat code. Q8, Q4, Q3. Smaller doll, it fits.

Don't stop here.
-->

---
layout: image
image: /complexity-kv.png
---

<!--
Weights sit still. KV grows with context and users. You can quant that too.

Now two memory piles. Concurrency is hiding in the second one.
-->

---
layout: image
image: /complexity-servers.png
---

<!--
vLLM: crowded bus, concurrency, wants more VRAM.

llama.cpp: bike, exotic parts, one chair.

24 GB forced the bike. Don't expect both.
-->

---
layout: image
image: /serving-complexity.png
---

<!--
Arch locks: this quant does not run on that GPU.

Spec decode is a second model. Fat prompts blow the clock. Prefix stamp.

New model on the conveyor. Agent octopus, 50x.

This is why the small box lied.
-->

---
layout: center
class: text-center
---

# Happy days

<img src="/happy-days.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Same product. Same users. Staging, then prod. The flakiness vanished.

A100 40 GB instead of A10 24 GB. Same generation, more VRAM. Finally vLLM.

Weights load once. KV cache moves with activity. Headroom equals concurrent users.

Scaled far better than the G5. You can be naive about the load balancer and still get away with it.

Not roses: no elastic. Sized for 10 customers plus fat. Four fixed cards.

Don't: stay on 24 GB out of stubbornness once you've seen 40 GB plus vLLM.
-->

---
layout: center
class: text-center
---

# Small, Fast AND Smart

<img src="/small-fast-smart.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Don't put the big model on every row. Definitely not every cell.

20 columns times 10,000 rows is 200,000 calls. A real LLM would take weeks.

Fast dumb ML for classify/score. Needed semantic tags. No model existed for the domain.

Fine-tune a tiny one. Gemma 4 E2B. Dumb as rocks, fast enough if you make it smart.

You had no idea. Codex, LoRA, MacBook, prompt/response pairs. 1200 records from frontier models. 12 hours.

Eval loop: point an agent at a number, let it thrash, keep what went up.

Result: G5 plus vLLM, 128 in flight, stupidly fast.

Longer game: capture traces, fine-tune SLMs, swap expensive LLM calls.

Massive success. You need a GPU to pull it off.

Don't: send 200,000 cells through the big model.
-->

---
layout: center
class: text-center
---

# That's the story

<img src="/happy-days.png" class="max-h-80 mx-auto rounded-lg mt-2" alt="" />

<!--
Optional closer: if the planner is live, send the room to it.

You need your own GPUs, or at least a hyperscaler, if you go this path.

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
