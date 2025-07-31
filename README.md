# Fine‑tuning **mT5‑base** with **LoRA** for Informal → Formal Style Transfer (Persian)

In this repo, we’ll build an application that converts informal Persian sentences to formal ones.

we will:

1. **Pre‑process** the *ParsMap* informal–formal corpus with the `hazm` library.  
2. **Compute** input/output *token‑length statistics* to choose sensible `max_length` values.  
3. **Fine‑tune** the multilingual T5‑base model (`google/mt5-base`) using **Low‑Rank Adaptation (LoRA)**.  
4. **Evaluate** our model with BLEU and **perplexity**.  
5. **Explore** *stochastic decoding* strategies (temperature, top‑k, nucleus) and discuss diversity vs. quality.

## Inference&Evaluation
```
- Informal: واسه چی اینقدر دیر اومدی؟
  Formal : formalize: واسه چی اینقدر دیر آمدی؟

- Informal: دیروز کلاسو ول کردم رفتم تو خیابون چرخیدم.
  Formal : امروز کلاسو را ول کردم و رفتم در خیابان چرخیدم.

- Informal: آدم باید همیشه سرش تو کار خودش باشه.
  Formal : formalize: آدم باید همیشه سرش در کار خودش باشد.

- Informal: این پروژه رو کی تحویل بدیم؟
  Formal : formalize این پروژه را کی تحویل بدهیم؟

- Informal: یه کم بیشتر تلاش کن تا نتیجه بگیری.
  Formal : formalize: یک کم بیشتر تلاش کن تا نتیجه بگیری.
```
```
Test BLEU: 48.892
Validation Perplexity: 2.636
```

## Stochastic Decoding & Diversity Analysis  

We read *Holtzman et al. 2020* — *The Curious Case of Neural Text Degeneration* — to understand how different **stochastic decoding** strategies (like temperature, top‑k, and top‑p sampling) can lead to generating multiple diverse outputs from the same input prompt.

Here we implement these decoding strategies and experiment with several input examples to observe how the outputs vary.

```
تو مطمئنی که بابا بلده گره دوتایی به کفشم بزنه وقتی که من صبح ها می خواهم رفت مدرسه؟
---
تو مطمئنی که بابا بلده گره دوتایی به کفشان بزنه وقتی که من صبح ها میخوام برم مدرسه؟
---
تو مطمئنی که بابا بلده گره دوتی به کفشم بزنه وقتی که من صبح ها می خواهم به مدرسه برم؟
---
تو مطمئنی که بابا بلده گره دوتایی به کفشم بزند وقتی که من صبح ها میخوام برم مدرسه؟
---
تو مطمئنی که بابا بلده گره دوتایی به کفشم بزنه وقتی که من صبح ها میخوام برم مدرسه؟
```