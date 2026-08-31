---
theme: default
title: Hosting an LLM in production
info: |
  Meetup talk — 2 Sept 2026
  Speaker: Dan Harper
---

<!--
Intro. Scale joke if you want it: on the web a CPU serves a million users, a GPU often serves one.
This is a war story. I lived it. I'm making light of it. A few "don't do this" if you go this path.
Don't lecture. Don't go deep on the why-host list. Personal colour only.
-->

# Hosting an LLM in production

Dan Harper · 2 Sept 2026

---
layout: center
---

<!--
- Doors: privacy, sovereignty, custom models, freedom. Pricing last, with an asterisk. It depends on a huge amount of things.
- Don't go deep on the list. Colour: why this list applied to you.
- Your priorities: 1) data sovereignty 2) per-token pricing would be very expensive.
- You started on hosted APIs like everyone else (Grok, OpenAI, Bedrock, Anthropic).
-->

# Why leave the API

---
layout: center
---

<!--
- What do I already have with local AU inference? Bedrock. Obvious.
- 99% of models infer overseas. Claude was not an option when you started. It is now. Ignore that.
- Shrink the problem until the smaller Amazon model fits. Nova.
- Product: data analytics platform, JSON query DSL. Treated the LLM like one request/response.
- RAG-shaped prompt: natural language + DSL + reference records. It kind of worked. It sucked. Nova wasn't smart enough.
- OpenRouter next. Same code, swap the pipe. Feel out capability vs size vs what you could host yourself.
- Don't: assume Sydney in the console means the GPU is in Sydney.
-->

# We're on AWS, but Bedrock sucked

---
layout: center
---

<!--
- Workable model in mind (Qwen 35B-A3B, MoE). Now find a GPU in Australia.
- EC2 GPU list price is ridiculous. Five times, ten times a CPU box. Not per-token anymore. Still paying.
- Aim small: 24 GB, AWS G5. Q3 to fit. Smarter than Nova. It ran.
- Then the agentic loop. Tools, explore the data. Mind-blowing. Also 25 calls, fat tokens. Call it 50x.
- Plan: scaling group, one box ≈ one concurrent request, capacity reservations.
- Fine through early dev. More developers, testers, customer demos: not scaling.
- H100s in Australia: basically never. G5 scale-up: two, three hour wait. Worst case.
- Pre-revenue MVP. Five or six internal users and it's already struggling.
- Don't: plan to ASG your way out of an Australian GPU shortage.
-->

# We're on AWS, but EC2 sucked worse

---
layout: center
---

<!--
- Google, Oracle, whoever might have cards. Same movie as AWS.
- GCP: nicer GPU attach. Ancient CPUs, ancient GPUs. Fancy stuff unobtainium.
- Didn't bother with Microsoft. Three examples is enough.
- LLM is already slower than the flight to Virginia. Hyperscalers put GPUs where power is cheap and people are.
- Australia: small market, expensive power. Close to zero.
- Don't: shop hyperscalers expecting a different ending.
-->

# Hyperscalers cannot save you

---
layout: center
---

<!--
- Bare metal: different costing. You support it. 2am in a data centre. Painful.
- In-between: VPS-ish, still a real GPU. Micron21 in Melbourne.
- Old banknote-security shop. Hardcore DC. Spare capacity.
- Priced against 10 B2B customers on AWS. Micron21 cheaper.
- Trial: one GPU, kick the tires. 12-month commit. A100s in two to four weeks if they're not sitting on a spare. Not elastic.
- Ended up at four GPUs, one box each. Round-robin. The clever ASG balancer can wait.
- Don't: confuse "I could buy a server" with "I want to debug hardware at 2am."
-->

# The Bare Metal Fantasy

---
layout: center
---

<!--
This is the one you can slow down on. Draw it if you want: weights sit there, KV cache moves with users.
- Two common servers. vLLM: production default, concurrency. Needs more VRAM.
- llama.cpp: portable, exotic GPUs / models / quants. Sucks at concurrency. Uses less VRAM.
- You wanted 4-bit. You were on 3. Q3 + a real context would not fit vLLM on 24 GB. Forced llama.cpp. No concurrency story.
- Unreliable: small prompts fine. Fat agent prompts, three minutes, then the four-minute timeout.
- Extra VRAM headroom is concurrency. Quantize weights and KV separately if you want. You still lose something.
- Don't: pick 24 GB and expect vLLM, concurrency, and a fat agent loop.
-->

# vLLM, llama.cpp & VRAM

---
layout: center
---

<!--
- Same product. Same users. Staging, then prod. The flakiness vanished.
- A100 40 GB instead of A10 24 GB. Same generation, more VRAM. Finally vLLM.
- Weights load once. KV cache moves with activity. Headroom = concurrent users.
- Scaled far better than the G5. You can be naive about the load balancer and still get away with it.
- Not roses: no elastic. Sized for 10 customers plus fat. Four fixed cards.
- Don't: stay on 24 GB out of stubbornness once you've seen 40 GB + vLLM.
-->

# Happy days

---
layout: center
---

<!--
- Don't put the big model on every row. Definitely not every cell.
- 20 columns × 10,000 rows = 200,000 calls. A real LLM would take weeks.
- Fast dumb ML for classify/score. Needed semantic tags. No model existed for the domain.
- Fine-tune a tiny one. Gemma 4 E2B. Dumb as rocks, fast enough if you make it smart.
- You had no idea. Codex, LoRA, MacBook, prompt/response pairs. 1200 records from frontier models. 12 hours.
- Eval loop: point an agent at a number, let it thrash, keep what went up.
- Result: G5 + vLLM, 128 in flight, stupidly fast.
- Longer game: capture traces, fine-tune SLMs, swap expensive LLM calls.
- Massive success. You need a GPU to pull it off.
- Don't: send 200,000 cells through the big model.
-->

# Small, Fast AND Smart

---
layout: center
---

<!--
Optional closer: if the planner is live, send the room to it.
You need your own GPUs, or at least a hyperscaler, if you go this path.
Don't do my first six ideas.
-->

# That's the story

---
layout: center
---

<!--
Leave this up. Don't fill silence with a new chapter.
-->

# Questions
