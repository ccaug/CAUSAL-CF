# CAUSAL-CF: Causal Counterfactual Explanations for Zero-Day Attack Detection

[![Paper](https://img.shields.io/badge/Paper-IEEE%20ICMI%202027-blue)](https://github.com/your-org/causal-cf)
[![Python](https://img.shields.io/badge/Python-3.9%2B-green)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Dataset](https://img.shields.io/badge/Dataset-UNSW--NB15-orange)](https://research.unsw.edu.au/projects/unsw-nb15-dataset)

Official implementation of **CAUSAL-CF**, a framework that integrates causal discovery with counterfactual generation for explainable zero-day attack detection.

---

## Overview

Conventional Explainable AI (XAI) methods for cybersecurity (LIME, SHAP) provide **correlational** explanations — they identify which features are important but cannot answer the operational question security analysts actually need:

> **"What would need to change for this alert to disappear?"**

CAUSAL-CF answers that question by:

1. **Learning a causal graph** from network traffic using the PC algorithm.
2. **Generating minimal counterfactuals** constrained by that causal structure.
3. **Providing analyst-oriented explanations** that describe which network attributes must change — and by how much — to flip a detection decision.

Unlike general-purpose counterfactual explainers, CAUSAL-CF respects causal dependencies between network variables, producing explanations that are **close to the original observation**, **sparse**, and **operationally plausible**.

---

## Key Results

| Metric | Value |
|--------|-------|
| Overall detection accuracy | **99.0%** |
| ZERO-DAY accuracy (Backdoor subset) | **100.0%** |
| Improvement over AEGIS | **+16.2 pp** |
| Counterfactual validity | **56.7%** |
| Counterfactual plausibility | **0.9889** |
| Average features changed | **4.5** |
| Counterfactual diversity | **100%** |
| Causal graph size | **20 nodes, 79 edges** |
| Inference latency | **13.61 ms/sample** |
| Model footprint | **90 KB** |

---

## Architecture

![CAUSAL-CF Architecture](docs/CAUSAL-CF-architecture.png)

The pipeline consists of five stages:

1. **Data Input Layer** — network flows, system logs, security alerts, and traffic statistics from UNSW-NB15.
2. **Causal Discovery Engine** — PC algorithm with conditional independence tests, skeleton identification, and edge orientation (Meili's rules).
3. **Detection & Classification** — three-tier detection:
   - **Tier 1 (KNOWN)**: Traditional ML (SVM / Random Forest)
   - **Tier 2 (RAG-UNKNOWN)**: Retrieval-augmented similarity search with FAISS
   - **Tier 3 (ZERO-DAY)**: Novelty detection (One-Class SVM)
4. **Counterfactual Generator** — causally constrained search, optimization engine, plausibility filter.
5. **Explanation & Insights** — counterfactual explanation, actionable insights, analyst support metrics.

---

## Dependencies

torch>=2.0.0
transformers>=4.35.0
scikit-learn>=1.3.0
numpy>=1.24.0
pandas>=2.0.0
causal-learn>=0.1.3
faiss-cpu>=1.7.4              # use faiss-gpu for GPU
matplotlib>=3.7.0
seaborn>=0.12.0
pyyaml>=6.0
tqdm>=4.65.0

---

## Evaluation Metrics
Counterfactual Quality
Metric	Definition
Validity	Fraction of counterfactuals that flip the classifier decision
Proximity	L2 distance between original and counterfactual
Sparsity	Number of features changed
Plausibility	Fraction of counterfactuals consistent with the causal graph
Diversity	Fraction of unique counterfactuals across samples
Detection Performance
Tier	Description
KNOWN	Attacks seen during training
RAG-UNKNOWN	Unseen families matched via FAISS retrieval
ZERO-DAY	Unseen attacks detected via novelty scoring

---

## Security Impact

Metric	Definition
Actionable insights/sample	Mean number of recommended changes per alert
Response time improvement	Simulated analyst speed-up
Trust improvement	Simulated analyst confidence
Analyst accuracy improvement	Simulated decision quality

