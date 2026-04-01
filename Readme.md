**Intelligent Product Taxonomy & Auto-Labeling Assistant**

A GenAI-powered auto-labeling system for e-commerce products using LLMs and prompt engineering to reduce mislabeling under ambiguous language conditions.

**GenAI-Based Auto-Labeling System for E-Commerce Products**
**(Hybrid Rules + LLM Architecture | Azure OpenAI)**

**📌 Project Overview**

E-commerce aggregator platforms often struggle with **inconsistent product labeling** due to unstructured seller inputs. This mislabeling leads to poor product discoverability, customer dissatisfaction, and revenue loss.

This project implements a **production oriented GenAI auto-labeling system** that classifies ambiguous product descriptions into:

Skin Care

Hair Care

Using Azure OpenAI LLMs, the system compares zero-shot and few-shot prompt engineering approaches and evaluates their performance using multiple randomized runs, reporting mean accuracy and standard deviation.
Rather than relying solely on LLMs, the system adopts a hybrid architecture (Rules + LLM) to ensure reliability under Azure OpenAI Responsible AI constraints.

**❓ Problem Statement**

The platform currently experiences approximately 27% product mislabeling, particularly between Skin Care and Hair Care categories.

**_Key Challenges_**

  Overlapping terminology (e.g., oil, wash, powder)
  
  Ambiguity between scalp care, body hair, and skin treatments
  
  Inconsistent and unstructured seller-provided descriptions
  
  Limited availability of labeled training data

Manual seller education efforts failed, necessitating an automated, intelligent labeling solution.

**🚨 Key Real-World Challenge Discovered**

During experimentation, **Azure OpenAI content filters blocked a significant portion of LLM classification requests**, even though the task was _non-generative and purely classificatory_.

  Retail terms such as body, massage, waist, lip balm, beard oil triggered **medium-severity sexual content filters**.

This made **LLM-only approaches unreliable** in practice.

**🏗️ Solution Architecture (Enterprise-Grade)**

**🔁 Hybrid Rules + LLM Pipeline**

Product Description

        │
        
        ▼

Rule-Based Classifier

        │
        
        ├── Confident → Final Label (No LLM call)
        
        │
        
        └── Ambiguous
        
                │
                
                ▼
          
          Azure OpenAI LLM
          
                │
                
                ├── Allowed → Use Prediction
                
                └── Blocked → Safe Fallback

**Why Hybrid?**

Rules handle **high-confidence cases deterministically**

LLM handles **semantic ambiguity**

System remains **functional even when LLM is blocked**

**🧪 Experimental Design**

**Classification Strategies Evaluated**

| Strategy                 | Description                      |
| ------------------------ | -------------------------------- |
| Zero-Shot LLM            | Instruction-only classification  |
| Few-Shot LLM             | Prompt includes labeled examples |
| **Hybrid (Rules + LLM)** | Rules first, LLM fallback        |

**Evaluation Methodology**

  Dataset split into:

    **Rule / few-shot example pool**

    **Gold evaluation set**

  5 randomized evaluation runs

  Metrics reported:

    **Mean accuracy**

    **Standard deviation**

**📊 Results Summary**

| Method                   | Mean Accuracy | Std Dev  | Observations                                  |
| ------------------------ | ------------- | -------- | --------------------------------------------- |
| Zero-Shot LLM            | 0.00          | 0.00     | Azure content filtering blocked most requests |
| Few-Shot LLM             | 0.00          | 0.00     | Longer prompts increased filter failures      |
| **Hybrid (Rules + LLM)** | **0.625**     | **0.00** | Stable, reliable, production-viable           |

**🔍 Key Observations & Learnings**
  
**LLM-only pipelines can collapse under Responsible AI constraints**

Azure OpenAI content filtering applies even to **classification tasks**

Few-shot prompting can **increase filter risk** due to longer prompts

Hybrid architectures significantly improve:

  Reliability

  Cost efficiency

  Predictability

Deterministic rules reduce LLM dependency and operational risk

**🛡️ Handling Azure OpenAI Content Filtering**

To ensure uninterrupted execution:

  LLM calls are wrapped with **fail-safe logic**

  When Azure blocks a request:

    The system **does not crash**

    A **safe fallback label ** is applied

  Evaluation pipelines are designed to tolerate model unavailability

This mirrors **real enterprise GenAI systems**, where LLM access is **not guaranteed**.

## Prediction Outputs

In addition to aggregate accuracy metrics, the system outputs sample-level predictions including:

- Original product description
- Gold label
- Predicted label
- Correctness flag

These outputs enable error analysis and manual review, which are
critical for real-world catalog automation systems.


**🧰 Tech Stack**

**LLM**

  Azure OpenAI (GPT-4o-mini)

**Language**

  Python

**Techniques**

  Prompt Engineering

  Zero-Shot & Few-Shot Learning  

  Hybrid Rule-Based Systems

**Libraries**

  pandas

  numpy

  scikit-learn

  tqdm

  python-dotenv

**📁 Project Structure**
src/
├── config/          # Environment & Azure OpenAI configuration
├── llm/             # Prompt engineering & LLM client
├── rules/           # Deterministic rule-based classifier
├── classifiers/     # LLM-only & hybrid classifiers
├── evaluation/      # Accuracy & metric computation
├── utils/           # Token counting & sampling utilities
├── experiments/     # Experiment orchestration

**▶️ How to Run**

pip install -r requirements.txt
python -m src.experiments.run_experiments


Ensure .env contains:

AZURE_OPENAI_KEY=...
AZURE_OPENAI_ENDPOINT=...
AZURE_OPENAI_API_VERSION=2024-12-01-preview
AZURE_OPENAI_MODEL=gpt-4o-mini

**🎯 Why This Project Matters**

This project demonstrates:

  Real-world GenAI limitations (not just ideal demos)

  Production-safe system design

  Hybrid AI architecture thinking

  Responsible AI-aware engineering

  Honest evaluation under constraints
