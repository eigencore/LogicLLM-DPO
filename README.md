# LogicLLM-DPO

This repository contains the data and codes for the implementation of **"LogicLLM-DPO"**, a modification of the framework used in the paper ["LOGIC-LM: Empowering Large Language Models with Symbolic Solvers for Faithful Logical Reasoning"](https://arxiv.org/abs/2305.12295). The code in this repository is based on the code provided by the original paper's authors, which can be found [here](https://github.com/teacherpeterpan/Logic-LLM/tree/main).

**Authors:** **Maximiliano Galindo**, **Jesus Vazquez**, **Francisco Lopez**

With support and collaboration from [GIL](https://www.facebook.com/ingenieriaLinguistica/?locale=es_LA) (*Grupo de Ingeniería Lingüística*), Universidad Nacional Autónoma de México.

---

## Introduction

The original paper introduces a framework that allows large language models (LLMs) to perform logical reasoning tasks. The framework is based on the idea of using a symbolic solver to generate logical formulas that are then used to guide the LLM in its reasoning process. The authors show that this approach can significantly improve the performance of LLMs on logical reasoning tasks.

### Key Idea:
- The LLM generates a **logic program** from a prompt.
- A symbolic solver generates a **formula** that represents the solution to the problem.
- The LLM uses this formula to generate the correct answer.

To improve the accuracy and number of successful translations of the LLM, the original paper implements a **self-refinement process**, enabling the LLM to iteratively improve its logic programs and formulas.

### Proposed Modification:
We propose replacing the **self-refinement process** with a **Direct Preference Optimization (DPO)** process. By explicitly guiding the LLM with correct logic programs and formulas, we aim to achieve more accurate logical reasoning results.

---

## Datasets

The datasets used in our experiments are preprocessed and stored in the `./data` folder. Below are the datasets evaluated:

- **[ProntoQA](https://github.com/asaparov/prontoqa):**
  Deductive reasoning dataset. We use the 5-hop subset of the *fictional characters* version, consisting of 500 testing examples.
- **[ProofWriter](https://allenai.org/data/proofwriter):**
  Deductive reasoning dataset. We use the depth-5 subset of the OWA version. To reduce experimentation costs, we randomly sample 600 examples in the test set and ensure a balanced label distribution.
- **[FOLIO](https://github.com/Yale-LILY/FOLIO):**
  First-Order Logic reasoning dataset. We use the entire FOLIO test set for evaluation, consisting of 204 examples.
- **[LogicalDeduction](https://github.com/google/BIG-bench/tree/main/bigbench/benchmark_tasks/logical_deduction):**
  Constraint Satisfaction Problems (CSPs). We use the full test set consisting of 300 examples.
- **[AR-LSAT](https://github.com/zhongwanjun/AR-LSAT):**
  Analytical Reasoning problems from the Law School Admission Test (1991–2016). The test set contains 230 multiple-choice questions.

> **Note**  
> Currently, we are developing the code to run experiments using the LLaMA model. Visit the `dpo-llama` branch for more details.

---

## Installation

To install the required libraries, navigate to the repository's root directory and run:

```bash
pip install -r requirements.txt
```

---

## Baselines

You can run experiments with the **Standard-LM (Direct)** and **Chain-of-Thought (CoT)** baselines using the following commands:

```bash
cd ./baselines
python llama_baseline.py \
    --model_name "Model Name [text-davinci-003 | gpt-4]" \
    --dataset_name "Dataset Name [ProntoQA | ProofWriter | FOLIO | LogicalDeduction ｜ AR-LSAT]" \
    --split dev \
    --mode "Baseline [Direct | CoT]" \
    --max_new_tokens "16 for Direct; 1024 for CoT"
```

The results will be saved in `./baselines/results`.

> **Warning**  
> The following command will evaluate the results of the baselines. However, the results and answers generated by **LLaMA** need to be processed first. This step is pending.

```bash
python evaluate.py \
    --dataset_name "Dataset Name [ProntoQA | ProofWriter | FOLIO | LogicalDeduction ｜ AR-LSAT]" \
    --model_name "Model Name [text-davinci-003 | gpt-4]" \
    --split dev \
    --mode "Baseline [Direct | CoT]"
```

---

## Logic Program Generation

To generate logic programs for logical reasoning problems in each dataset, run the following command from the root directory:

```bash
python models/logic_llama_program.py \                
    --dataset_name "Dataset Name [ProntoQA | ProofWriter | FOLIO | LogicalDeduction ｜ AR-LSAT]" \
    --split dev \
    --model_name "meta-llama/Llama-2-7b-hf" \
    --max_new_tokens 1024
```

The generated logic programs will be saved in `outputs/logic_programs`.

> **Note**  
> We are currently developing the logic inference step using the symbolic solver. This step is pending.

---

## Acknowledgments

This project is supported by the [GIL](https://www.facebook.com/ingenieriaLinguistica/?locale=es_LA) (*Grupo de Ingeniería Lingüística*) at the Universidad Nacional Autónoma de México. Special thanks to the authors of the original [Logic-LM](https://arxiv.org/abs/2305.12295) paper for their foundational work.
