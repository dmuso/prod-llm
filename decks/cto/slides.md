---
theme: default
title: Why I Stopped Worrying and Loved the Tokens
info: |
  CTO talk — 8 Oct 2026
  A Year Self Hosting LLMs in Production
  Speaker: Dan Harper
colorSchema: dark
class: text-center
---

<h1 class="!text-4xl">Why I Stopped Worrying and Loved the Tokens</h1>

<div class="text-xl opacity-70 font-normal mt-1">A Year Self Hosting LLMs in Production</div>

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

<div class="opacity-70 mt-6">8 Oct 2026</div>

<!--
Ten seconds. Then into it.

Three beats: why I started, why I stayed, what it unlocked.
-->

---
layout: center
class: text-center
---

# Why we went down this path

<img src="/why-leave-api.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Sovereignty. That was the reason.

99% of models infer overseas.

Bedrock sucked. EC2 sucked worse.

Hyperscalers: nope.

Australia is a GPU desert. Arse end of the world.

One minute of war colour. Max. Move on.
-->

---
layout: center
class: text-center
---

# Not the reason I stayed

<img src="/bedrock-overseas.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
The reason I started isn't the reason I stayed.

Sovereignty is softening. In-region providers keep turning up.

If that was the only reason, I'd be back on an API.

The reason I stayed has a year of proof: the bill.
-->

---
layout: center
class: text-center
---

# Your bill shape is your usage shape

<div class="text-lg opacity-70 font-normal mt-1">Decided by your product's usage shape, not by the model</div>

<img src="/vram-concurrency.png" class="max-h-80 mx-auto rounded-lg mt-2" alt="" />

<!--
Your bill shape is decided by your product's usage shape, not by the model.

Tokens: every call costs. Forever.

Our own cards: flat bill.

Agentic loop. 50x the use.

On tokens, that's 50x the bill. On four cards, it's the same four cards.

200,000 cells. 20 columns x 10,000 rows. Through a token meter. Ouch.

Next slide: the receipts.
-->

---
layout: center
class: text-center
---

# One feature, two bills

<div class="grid grid-cols-2 gap-12 mt-10 max-w-3xl mx-auto">
  <div class="rounded-xl border-2 border-gray-500 bg-white/5 p-8">
    <div class="text-xl opacity-70">Cloud API (per token)</div>
    <div class="text-6xl font-bold mt-4">$___</div>
    <div class="text-2xl mt-2 opacity-80">___ tokens</div>
    <div class="text-sm opacity-60 mt-4">[feature name] · [period]</div>
  </div>
  <div class="rounded-xl border-2 border-gray-500 bg-white/5 p-8">
    <div class="text-xl opacity-70">Our own GPUs</div>
    <div class="text-6xl font-bold mt-4">$___</div>
    <div class="text-2xl mt-2 opacity-80">___ tokens</div>
    <div class="text-sm opacity-60 mt-4">[feature name] · [period]</div>
  </div>
</div>

<!--
One real feature. Measured tokens. Two prices.

[SAY: feature X used N tokens over <period>. At API pricing that's $A. On our cards it's $B.]

Same tokens. Different bill shape.
-->

---
layout: center
class: text-center
---

# The catch with a flat bill

<img src="/bare-metal-2am.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
12-month commit.

GPUs in 2 to 52 weeks.

Not elastic. Four fixed cards.

Sizing is genuinely hard. Budget people for it.

Happy days: A100 40 GB + vLLM. Flakiness vanished.

Rule of thumb:
Spiky, low or unpredictable usage? Stay on tokens.
Steady, high-volume, agentic or batch? Own the hardware.

Argue with me. I dare you.
-->

---
layout: center
class: text-center
---

# What no provider could sell us

<img src="/small-fast-smart.png" class="max-h-85 mx-auto rounded-lg mt-2" alt="" />

<!--
Don't put the big model on every row.

Fine-tuned a tiny model. Gemma 4 E2B. LoRA. 1,200 records. 12 hours.

Eval loop. 128 in flight. Stupidly fast.

Long game: capture traces, fine-tune small models, swap out expensive LLM calls.

Only possible because we own the GPUs.

Your bill shape is your usage shape.
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
