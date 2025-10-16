<h2 align="center" style="font-size: 2.5em; font-weight: bold; color: #2c3e50;">
  <i>WritingPreferenceBench</i>: Evaluating Subjective Writing Preferences Across Cultures
</h2>

<p align="center">
  <a href="https://WritingPreferenceBench.github.io/" style="margin: 0 10px;">🌐 Homepage</a> |
  <a href="https://huggingface.co/datasets/m-a-p/WritingPreferenceBench" style="margin: 0 10px;">🤗 Dataset</a> |
  <a href="" style="margin: 0 10px;">📖 ArXiv</a> |
  <a href="https://github.com/m-a-p/WritingPreferenceBench" style="margin: 0 10px;">🐙 GitHub</a>
</p>

This repository contains the dataset, evaluation scripts, and benchmark setup for the paper "[WritingPreferenceBench: Evaluating Subjective Writing Preferences Across Cultures]()".

---

## 🔔 Introduction

<p align="center">
  <img src="images/WPB_main.png" alt="WritingPreferenceBench Overview" style="width: 800px;"> 
</p>

**WritingPreferenceBench** is a cross-lingual benchmark designed to evaluate language models’ ability to recognize **subjective writing quality**—including creativity, stylistic sophistication, and emotional resonance—while neutralizing objective quality signals such as grammar, factuality, and length.  
It consists of **1,800 human-validated preference pairs** (1,200 English and 600 Chinese) across **8 creative writing genres** and **51 subcategories**, where each pair contrasts two grammatically correct, factually accurate, and length-matched responses.  

Our findings reveal that standard **sequence-based reward models (SC-RM)** perform near random (52.7% accuracy), while **generative reward models (GenRM)** that produce explicit reasoning chains achieve up to **81.8% accuracy**. This highlights the critical need for **reasoning-based architectures** in modeling subjective human preferences.  

The dataset enables rigorous, reproducible evaluation of subjective writing quality and provides a foundation for next-generation **preference learning** and **RLHF** research.

---

## 🧩 Benchmark Overview

WritingPreferenceBench introduces a **three-stage human-in-the-loop data curation pipeline**, combining expert creative writing design, large-scale model generation, and rigorous human evaluation.

1. **Query Design:**  
   - 51 creative writing categories (8 macro domains).  
   - Authored by professional writing instructors with iterative expert review.  

2. **Response Generation:**  
   - 20 state-of-the-art models (e.g., GPT-4.1, Claude-4, Gemini-2.5-Pro, Doubao-1.5-Pro).  
   - 5 temperature-sampled responses per query (T = 0.8).  

3. **Human Evaluation:**  
   - 11 expert annotators, 4 English and 7 Chinese professionals.  
   - 4-point quality scale (0–3) capturing creativity, style, and engagement.  
   - Only pairs with ≥2/3 annotator agreement and Δscore ≥1 were retained.

---

## 📊 Dataset Statistics

| Language | #Pairs | #Categories | Mean Score Gap | Mean Length (Chosen) | Mean Length (Rejected) |
|-----------|---------|--------------|----------------|----------------------|------------------------|
| English   | 1,200   | 51           | 1.31           | 1,450.3              | 839.9                  |
| Chinese   | 600     | 51           | 1.45           | 1,873.5              | 1,458.3                |

**Macro Categories:** Fiction · Non-Fiction · Functional Documents · Promotional & Communication · Funny · Poetry · Scriptwriting · Role-Playing

---

## 🧠 Evaluation Protocol

Two standardized evaluation protocols are supported:

1. **Reward Model Scoring**  
   Reward models assign scalar scores for each response pair `(R_chosen, R_rejected)`.  
   A prediction is correct if `RM(R_chosen) > RM(R_rejected)`.

2. **Pairwise Preference Judgment**  
   LLM judges compare two responses and select the preferred one based on creativity, stylistic flair, and emotional resonance.  

Both modes can be reproduced using our public evaluation scripts in this repository.

---

## 🏁 Main Results

**Reward Models (Accuracy %)**  
| Model | Type | EN | ZH | Avg |
|--------|------|----|----|-----|
| RM-R1-Qwen2.5-7B | Generative RM | **81.8** | 73.3 | 77.6 |
| RM-R1-DeepSeek-Qwen-14B | Generative RM | 62.5 | 62.6 | 62.6 |
| RM-Mistral-7B | Sequence Classifier | 62.6 | 55.6 | 59.1 |
| Nvidia/AceMath-7B | Sequence Classifier | 46.8 | 53.5 | 50.2 |

**LLM Judges (Zero-Shot Accuracy %)**  
| Model | EN | ZH | Avg |
|--------|----|----|-----|
| Doubao-1.5-Pro | 68.7 | 62.5 | **65.6** |
| Gemini-2.5-Pro | 65.7 | 62.7 | 64.2 |
| Claude-4-Opus-thinking | 61.0 | 56.0 | 58.5 |
| OpenAI-o3-high | 48.1 | 42.0 | 45.1 |

These results confirm that **generative reasoning architectures** substantially outperform sequence-based classifiers and zero-shot LLM judges in capturing human subjective preferences.

---

## ⚙️ Installation

To install the required packages, run:

```bash
git clone git@github.com:m-a-p/WritingPreferenceBench.git
cd ./WritingPreferenceBench
pip install -r requirements.txt
```

---

## 🧾 Usage Example

To evaluate model predictions using WritingPreferenceBench:

```bash
export PYTHONPATH=$(pwd)

# Reward model evaluation
python eval/eval_reward_model.py --input_dir data/pairs --model_name RM-R1-Qwen2.5-7B --output_dir results

# LLM-as-judge evaluation
python eval/eval_llm_judge.py --input_dir data/pairs --model_name Gemini-2.5-Pro --output_dir results
```

All scripts automatically compute accuracy based on human-annotated preference pairs.

📝 **Notes**
- The dataset and evaluation templates are standardized for both English and Chinese subsets.
- Please ensure consistent random seeds when reproducing experiments.
- Default outputs include `.jsonl` results and accuracy summaries in `.csv` format.

---

## 📜 License

**WritingPreferenceBench** is released under the **[Open Data Commons Attribution License (ODC-BY)](https://opendatacommons.org/licenses/by/)**.  
The dataset and accompanying scripts are freely available for research and educational use.  

You are required to:
- Give proper attribution to the original authors.  
- Comply with the licenses of any referenced datasets included in derived works.  

---

## 📚 Citation

**BibTeX:**
```bibtex
@misc{zhang2025writingpreferencebench,
      title={WritingPreferenceBench: Evaluating Subjective Writing Preferences Across Cultures},
      author={Ge Zhang and Shuangshuang Ying and Yunwen Li and Xingwei Qu and Xin Li and Sheng Jin and Minghao Liu and Zhoufutu Wen and Xeron Du and Tianyu Zheng and Yichi Zhang and Letian Ni and Yuyang Cheng and Qiguang Chen and Jingzhe Ding and Shengda Long and Wangchunshu Zhou and Jiazhan Feng and Wanjun Zhong and Libo Qin and Wenhao Huang and Wanxiang Che and Chenghua Lin},
      year={2025},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={}
}
```
