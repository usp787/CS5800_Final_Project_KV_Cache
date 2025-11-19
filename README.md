# CS5800_Final_Project_KV_Cache

Authors: Jiani He, Jiarui Zha

This repository contains the code and analysis for a project investigating the Key-Value (KV) Cache , a critical optimization in modern Large Language Models (LLMs). This project explores the KV cache not just as a software optimization but as a direct, practical application of a fundamental computer science paradigm: Dynamic Programming (DP).

---

## Project Overview
LLMs based on the Transformer architecture have revolutionized AI . However, their performance during text generation (inference) is often slow . This is because generating text is an autoregressive process, where each new word (token) is generated sequentially.

---

### The Problem: $O(n^2)$ Inference

- In a standard Transformer, generating a single new token requires the model to re-compute attention over **all** previous tokens in the sequence.
- To generate **Token 10**, the model attends to tokens 1-9.
- To generate **Token 11**, the model must re-attend to tokens 1-10.
- This redundant computation results in a computational complexity of $O(n^2)$ relative to the sequence length $n$ . As the text context grows, generation becomes prohibitively slow.

---

## The Solution: KV Cache as Memoization
The KV cache is an optimization that stores the "Key" and "Value" representations from the self-attention mechanism for each token. When generating the next token, the model reuses these cached values instead of recomputing them. This project demonstrates that this caching mechanism is a direct application of **memoized dynamic programming**.

---

## Research Question
This project seeks to answer a two-part question:

- **Theoretical**: What is the formal connection between Dynamic Programming (specifically memoization) and the KV cache mechanism in Transformers?

- **Empirical**: How, and to what extent, does the KV cache optimize the autoregressive self-attention process in a real-world model?

---

## The Algorithmic Connection (Dynamic Programming)
The core of this project is linking the KV cache to principles from an algorithms course.

- **Overlapping Subproblems**: Autoregressive generation is a perfect example of a problem with overlapping subproblems. The calculation for generating token $t+1$ (which involves tokens $1$ to $t$) contains the entire, unmodified calculation for generating token $t$ (which involved tokens $1$ to $t-1$).

- **Memoization**: The KV cache acts as the "memo" table in classic DP . It stores the results of these subproblems (the computed Key and Value matrices for each token).

- **The Trade-off**: By reusing these cached values, the model avoids redundant computation. This optimization transforms the computational complexity of attention from $O(n^2)$ to $O(n)$. This demonstrates a classic space-time trade-off: we use $O(n \times d)$ additional memory to store the cache, but in return, we gain a significant $O(n)$ time-saving in computation.

---

## Experimental Design
To empirically verify the performance benefits, this project uses a controlled experiment.

- **Model**: DistilGPT2, a lightweight open-source Transformer model.

- **Control Group**: The model generates sequences without a KV cache, recomputing all attention at each step.

- **Experimental Group**: The model generates the same sequences with KV caching enabled.

---

### Methodology
- 1.Generate sequences of various lengths (e.g., 10, 50, 100, and 500 tokens).

- 2.Run multiple trials for each configuration to ensure stable results.

- 3.Record the **total generation time** and **time per token** for both groups.

- 4.Analyze and visualize the data using line graphs and log-scale plots to compare how runtime scales with sequence length.

---

## Expected Outcome
The results are expected to clearly demonstrate that:
- **Without KV Cache (Control)**: Generation time will scale quadratically $O(n^2)$ with sequence length.
- **With KV Cache (Experimental)**: Generation time will scale linearly $O(n)$ with sequence length.

---

This will provide empirical proof that the KV cache not only improves efficiency but also fundamentally transforms the scaling behavior of LLM inference, bridging classical algorithmic theory with cutting-edge AI systems .
