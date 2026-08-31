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
Doors: privacy, sovereignty, custom models, freedom. Pricing last, with an asterisk. It depends on a huge amount of things.

Don't go deep on the list. Colour: why this list applied to you.

Your priorities: 1) data sovereignty 2) per-token pricing would be very expensive.

You started on hosted APIs like everyone else. Grok, OpenAI, Bedrock, Anthropic. Then you walked out.
-->

---
layout: center
class: text-center
---

# We're on AWS, but Bedrock sucked

<img src="/bedrock-overseas.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
What do I already have with local AU inference? Bedrock. Obvious.

99% of models infer overseas. Claude was not an option when you started. It is now. Ignore that.

Shrink the problem until the smaller Amazon model fits. Nova.

Product: data analytics platform, JSON query DSL. Treated the LLM like one request/response.

RAG-shaped prompt: natural language + DSL + reference records. It kind of worked. It sucked. Nova wasn't smart enough.

OpenRouter next. Same code, swap the pipe. Feel out capability vs size vs what you could host yourself.

Don't: assume Sydney in the console means the GPU is in Sydney.
-->

---
layout: center
class: text-center
---

# We're on AWS, but EC2 sucked worse

<img src="/ec2-sucked.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Workable model in mind (Qwen 35B-A3B, MoE). Now find a GPU in Australia.

EC2 GPU list price is ridiculous. Five times, ten times a CPU box. Not per-token anymore. Still paying.

Aim small: 24 GB, AWS G5. Q3 to fit. Smarter than Nova. It ran.

Then the agentic loop. Tools, explore the data. Mind-blowing. Also 25 calls, fat tokens. Call it 50x.

Plan: scaling group, one box is about one concurrent request, capacity reservations.

Fine through early dev. More developers, testers, customer demos: not scaling.

H100s in Australia: basically never. G5 scale-up: two, three hour wait. Worst case.

Pre-revenue MVP. Five or six internal users and it's already struggling.

Don't: plan to ASG your way out of an Australian GPU shortage.
-->

---
layout: center
class: text-center
---

# Hyperscalers cannot save you

<img src="/hyperscalers.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Google, Oracle, whoever might have cards. Same movie as AWS.

GCP: nicer GPU attach. Ancient CPUs, ancient GPUs. Fancy stuff unobtainium.

Didn't bother with Microsoft. Three examples is enough.

LLM is already slower than the flight to Virginia. Hyperscalers put GPUs where power is cheap and people are.

Australia: small market, expensive power. Close to zero.

Don't: shop hyperscalers expecting a different ending.
-->

---
layout: center
class: text-center
---

# The Bare Metal Fantasy

<img src="/bare-metal-2am.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Bare metal: different costing. You support it. 2am in a data centre. Painful.

In-between: VPS-ish, still a real GPU. Micron21 in Melbourne.

Old banknote-security shop. Hardcore DC. Spare capacity.

Priced against 10 B2B customers on AWS. Micron21 cheaper.

Trial: one GPU, kick the tires. 12-month commit. A100s in two to four weeks if they're not sitting on a spare. Not elastic.

Ended up at four GPUs, one box each. Round-robin. The clever ASG balancer can wait.

Don't: confuse "I could buy a server" with "I want to debug hardware at 2am."
-->

---
layout: center
class: text-center
---

# vLLM, llama.cpp & VRAM

<img src="/vram-concurrency.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
This is the one you can slow down on. Weights sit there. KV cache moves with users.

Two common servers. vLLM: production default, concurrency. Needs more VRAM.

llama.cpp: portable, exotic GPUs / models / quants. Sucks at concurrency. Uses less VRAM.

You wanted 4-bit. You were on 3. Q3 plus a real context would not fit vLLM on 24 GB. Forced llama.cpp. No concurrency story.

Unreliable: small prompts fine. Fat agent prompts, three minutes, then the four-minute timeout.

Extra VRAM headroom is concurrency. Quantize weights and KV separately if you want. You still lose something.

Don't: pick 24 GB and expect vLLM, concurrency, and a fat agent loop.
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
