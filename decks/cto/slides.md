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

<div class="grid grid-cols-[3fr_2fr] gap-8 items-center h-full">
  <img src="/token-bomb.jpg" class="w-full rounded-lg shadow-lg" alt="Aussie bloke riding a gold TOKENS bomb" />
  <div class="text-left">
    <h1 class="!text-3xl !leading-tight !mb-0">Why I Stopped<br>Worrying and<br>Loved the Tokens</h1>
    <div class="text-lg opacity-70 font-normal mt-3">A Year Self Hosting LLMs in Production</div>
    <div class="flex items-center gap-4 mt-8">
      <img src="/dan-harper.jpg" class="rounded-full w-20 h-20 object-cover shadow-lg shrink-0" alt="Dan Harper" />
      <div class="leading-snug">
        <div class="text-2xl font-bold">Dan Harper</div>
        <div class="text-base opacity-80">CTO @ AskYourTeam</div>
        <a href="https://x.com/dan_harper" target="_blank" class="inline-flex items-center gap-2 mt-1 text-base font-semibold !border-none">
          <img src="/x-logo.svg" class="w-4 h-4" alt="X" />
          <span>@dan_harper</span>
        </a>
      </div>
    </div>
    <div class="opacity-70 mt-6">8 Oct 2026</div>
  </div>
</div>

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

# One month, two bills

<div class="grid grid-cols-[1fr_auto_1fr] gap-6 items-center mt-8 max-w-4xl mx-auto">
  <div class="rounded-xl border-2 border-gray-500 bg-white/5 p-6">
    <div class="text-xl opacity-70">Cloud API (per token)</div>
    <div class="text-6xl font-bold mt-4 text-red-400">$21,000</div>
    <div class="text-lg opacity-80 mt-2">AUD / month</div>
    <div class="text-sm opacity-60 mt-3">at Claude Haiku pricing</div>
  </div>
  <div class="text-3xl font-bold text-amber-300">~3x</div>
  <div class="rounded-xl border-2 border-gray-500 bg-white/5 p-6">
    <div class="text-xl opacity-70">Our own GPUs</div>
    <div class="text-6xl font-bold mt-4 text-green-400">$6,500</div>
    <div class="text-lg opacity-80 mt-2">AUD / month</div>
    <div class="text-sm opacity-60 mt-3">local LLM infra</div>
  </div>
</div>

<div class="text-base opacity-70 mt-8">All LLM usage across the product · 1 month · 27 customers · 1.485M API calls · 46.8B input / 2.34B output tokens</div>

<!--
This is everything. All LLM usage across the whole product. No cherry-picking.

One month. 27 customers. 1.5 million calls.

Nearly 47 billion tokens in. 2.3 billion out.

At Haiku prices, that's about 21 grand a month. AUD.

That's like-for-like, small model vs small model. Against Sonnet it'd be about 84 grand.

On our own cards? 6.5.

Same workload. About three times cheaper.
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
