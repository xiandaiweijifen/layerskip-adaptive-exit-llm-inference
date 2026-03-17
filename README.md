# Adaptive Exit Layer Inference Optimization for LLMs with LayerSkip

This project studies inference optimization for large language models based on **LayerSkip** and **self-speculative decoding**, with a focus on **adaptive exit layer control**. The goal is to improve the **latency-throughput-quality trade-off** of autoregressive generation while keeping output quality broadly stable on summarization tasks.

The repository contains two stages of work:
1. an exploratory study of **classic speculative decoding**;
2. a main optimization study based on **LayerSkip adaptive exit strategies**.

---

## Overview

Autoregressive decoding in large language models is often limited by high inference latency. Recent acceleration methods such as **speculative decoding** and **self-speculative decoding** provide promising directions for reducing decoding cost without heavily sacrificing generation quality.

A standard speculative decoding pipeline typically relies on a separate **draft model** and **target model**. While effective in some settings, this setup introduces additional system overhead and potential **draft-target mismatch**, which can reduce practical speedup.

To address this, the main direction of this project focuses on **LayerSkip / self-speculative decoding**, where earlier layers of the same model are used as a lightweight draft and later layers perform verification. On top of this framework, the project studies:

- **target-only decoding**
- **fixed exit layer strategies**
- **adaptive exit layer strategies**

The central question is:

> How should the exit layer be selected to achieve a better balance among latency, throughput, and generation quality?

---

## Project Goals

This project is designed around the following goals:

- study the efficiency limitations of autoregressive decoding in large language models;
- explore the trade-offs of **classic speculative decoding** under different control policies;
- build a **LayerSkip-based inference optimization framework** for summarization tasks;
- design and evaluate an **adaptive exit layer controller** instead of relying only on fixed exit layers;
- analyze the trade-offs among **latency**, **throughput**, and **generation quality**.

---

## Main Contributions

### 1. Classic speculative decoding exploration
This repository includes an exploratory baseline study based on draft-target speculative decoding, including:

- target-only baseline;
- fixed-k speculative decoding;
- adaptive-k decoding;
- enhanced adaptive-k variants;
- phase-aware adaptive-k strategies;
- draft model scale ablation.

This stage is used to understand how **speculation length**, **acceptance rate**, and **draft model size** affect practical decoding efficiency.

### 2. LayerSkip-based adaptive exit optimization
The main method of this project is built on **LayerSkip / self-speculative decoding**, including:

- target-only decoding;
- fixed exit layer strategies;
- adaptive exit layer strategies;
- adaptive exit control logic for long summarization samples;
- latency / throughput / quality trade-off analysis.

This stage corresponds to the final project positioning used in the resume.

---

## Key Results

On **CNN/DailyMail summarization**, the project compares **target-only decoding**, **fixed exit layer baselines**, and **adaptive exit layer strategies**.

In the current 100-sample experiment setting, the final adaptive exit design achieves:

- **average latency reduction** from **0.775 s** to **0.620 s**;
- **throughput improvement** from **123.9 tokens/s** to **162.7 tokens/s**;
- **ROUGE scores remain broadly stable**;
- performance approaches the best fixed-exit baseline while offering a more flexible strategy for longer samples.

### Result Snapshot

| Method | Avg Latency (s) | Throughput (tokens/s) | Quality |
|--------|------------------|-----------------------|---------|
| Target-only | 0.775 | 123.9 | ROUGE broadly stable |
| Adaptive Exit | 0.620 | 162.7 | ROUGE broadly stable |

> Note: exact numbers may vary slightly depending on model version, decoding configuration, and sample selection.

---