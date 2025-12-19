<div align="center">
  <img src="images/logo.png" style="zoom: 67%;" />

  # SeedBench: A Multi-task Benchmark for Evaluating Large Language Models in Seed Science

  <p align="center">
    <a href="./README.md"><b>English</b></a> |
    <a href="./README_zh.md"><b>简体中文</b></a>
  </p>
</div>

**SeedBench** 是首个专为评测种子科学（特别是育种）领域大语言模型 (LLM) 设计的多任务基准。本仓库包含数据集、评测代码以及相关文档，旨在支持该领域的研究。点击 [此处](https://github.com/open-sciencelab/SeedBench/issues/11) 查看详细用法。

---

## 🌾 概览

SeedBench 针对种子育种的三个核心阶段对 LLM 进行评估：

* **基因信息检索 (Gene Information Retrieval)**
* **基因功能与调控分析 (Gene Function and Regulation Analysis)**
* **农艺性状优化的品种育种 (Variety Breeding with Agronomic Trait Optimization)**

*育种专家工作流框架*

SeedBench 由领域专家共同构建，包含 **2,264 个经过专家验证的问题**，涵盖 11 种任务类型和 10 个育种子阶段。目前主要针对水稻育种，未来版本将扩展至玉米、大豆和小麦等其他作物。

## 🔎 数据集详情

* **语料库**：从 308,727 篇出版物中清洗出 11 亿 token；包含来自 113 文档的 279 个片段。
* **问题**：涵盖 11 种任务类型的 2,264 个问题，双语（中/英），均经过专家验证。
* **核心关注**：以水稻育种为典型案例。

**题型与指标：**

<div align="center">

| 类型 ID | 问题类型 | 指标 | 数量 |
| --- | --- | --- | --- |
| **Q&A (问答)** |  |  |  |
| QA-1 | 多项选择 (Multiple Choice) | Accuracy | 200 |
| QA-2 | 多项回答 (Multiple Answer) | Macro-F1 | 187 |
| QA-3 | 填空 (Fill-in-the-Blank) | ROUGE-L | 224 |
| QA-4 | 生成式问答 (Generation) | ROUGE-L | 242 |
| **Summarization (摘要)** |  |  |  |
| SUM-1 | 简单摘要 (Simple Summarization) | ROUGE-L | 225 |
| SUM-2 | 关键信息提取 (Key Information Extraction) | ROUGE-L | 225 |
| **Reading Comprehension (阅读理解)** |  |  |  |
| RC-1 | 多项选择 (Multiple Choice) | Accuracy | 113 |
| RC-2 | 多项回答 (Multiple Answer) | Macro-F1 | 108 |
| RC-3 | 填空 (Fill-in-the-Blank) | ROUGE-L | 221 |
| RC-4 | 生成式问答 (Generation) | ROUGE-L | 240 |
| RC-5 | 子类别分类 (Subcategory Classification) | Accuracy | 279 |

</div>

**育种阶段分布：**

<div align="center">
<img src="images/distribution.png" width="50%" alt="分类分布">
</div>

## ☀️ 主要结果

我们评估了 26 个 LLM，包括闭源、开源以及领域特定模型。亮点如下：

### 按问题类型表现

* **表现最佳者**：DeepSeek-V3 (68.37), GPT-4 (67.88)。

### 按任务类型表现

| 模型 | QA-1 | QA-2 | QA-3 | QA-4 | SUM-1 | SUM-2 | RC-1 | RC-2 | RC-3 | RC-4 | RC-5 | 平均分 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GPT-4 | 60.50 | 73.87 | 21.35 | 36.07 | 58.73 | 62.89 | 100.00 | 96.44 | 87.86 | 62.29 | 86.74 | 67.88 |
| DeepSeek-V3 | 72.50 | 79.84 | 29.29 | 40.63 | 48.06 | 54.67 | 100.00 | 97.22 | 87.89 | 55.19 | 86.74 | 68.37 |
| Qwen2-72B | 59.50 | 75.98 | 19.55 | 31.62 | 31.08 | 63.09 | 99.12 | 94.24 | 72.20 | 51.58 | 89.96 | 62.54 |

### 按育种子阶段表现

| 模型 | C1 | C2 | C3 | C4 | C5 | C6 | C7 | C8 | C9 | C10 | 平均分 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GPT-4 | 59.59 | 60.55 | 76.32 | 61.16 | 56.34 | 59.35 | 63.67 | 64.74 | 60.65 | 67.66 | 62.06 |
| DeepSeek-V3-671B | 56.03 | 62.42 | 74.81 | 63.17 | 55.23 | 58.84 | 68.23 | 69.04 | 66.46 | 68.48 | 63.30 |
| Qwen2-72B | 51.16 | 58.10 | 74.07 | 59.72 | 51.58 | 57.76 | 58.85 | 61.63 | 56.69 | 59.11 | 57.62 |

* **表现最佳者**：DeepSeek-V3-671B (63.30), GPT-4 (62.06)。

## 🐝 仓库内容

* `base_model_eval/`: 用于测试没有对话能力的基座模型（Base Models），即评估预训练后的性能。
* `sft_model_eval/`: 用于测试 SFT（监督微调）模型，共包含覆盖 10 个子类别的 2,264 个问题（见图 2）。
* `one-shot/`: 按 11 种任务类型组织（见表 1）。
* `zero-shot/`: 按 11 种任务类型组织（见表 1）。


* `corpus/`: 包含 279 个高质量文本片段，以及专家验证后剔除的低质量问题。
* `README.md`: 英文版说明文件。

---

## 🚀 如何使用 SeedBench

为了在 SeedBench 上评测模型，我们使用 **OpenCompass** 框架。请按照以下步骤配置环境并运行评测。

### 1. 安装

克隆 OpenCompass 仓库并安装必要的依赖（包括用于下载数据集的 `modelscope`）。

```bash
git clone [https://github.com/open-compass/opencompass](https://github.com/open-compass/opencompass) opencompass
cd opencompass
pip install -e .
pip install modelscope

```

### 2. 评测

设置数据集来源环境变量并执行评测脚本。以下示例使用的是 `Qwen/Qwen2.5-0.5B-Instruct` 模型。

```bash
DATASET_SOURCE=ModelScope python run.py --hf-type chat \
    --hf-path Qwen/Qwen2.5-0.5B-Instruct \
    --datasets seedbench_gen \
    --debug

```

> **📝 注意事项：**
> * **数据集下载：** 首次运行可能需要几分钟时间从 ModelScope 自动下载数据集。
> * **本地模型：** 如有需要，您可以将 `Qwen/Qwen2.5-0.5B-Instruct` 替换为您本地模型的绝对路径。
> * **更多细节请见[此处](https://github.com/open-sciencelab/SeedBench/issues/11)**
>

## 📬 引用

如果您有任何问题，请在本仓库提交 Issue。

```txt
@inproceedings{ying-etal-2025-seedbench,
  title = "{S}eed{B}ench: A Multi-task Benchmark for Evaluating Large Language Models in Seed Science",
  author = "Ying, Jie  and
    Chen, Zihong  and
    Wang, Zhefan  and
    Jiang, Wanli  and
    Wang, Chenyang  and
    Yuan, Zhonghang  and
    Su, Haoyang  and
    Kong, Huanjun  and
    Yang, Fan  and
    Dong, Nanqing",
  editor = "Che, Wanxiang  and
    Nabende, Joyce  and
    Shutova, Ekaterina  and
    Pilehvar, Mohammad Taher",
  booktitle = "Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)",
  month = jul,
  year = "2025",
  address = "Vienna, Austria",
  publisher = "Association for Computational Linguistics",
  url = "[https://aclanthology.org/2025.acl-long.1516/](https://aclanthology.org/2025.acl-long.1516/)",
  pages = "31395--31449",
  ISBN = "979-8-89176-251-0",
  abstract = "Seed science is essential for modern agriculture, directly influencing crop yields and global food security. However, challenges such as interdisciplinary complexity and high costs with limited returns hinder progress, leading to a shortage of experts and insufficient technological support. While large language models (LLMs) have shown promise across various fields, their application in seed science remains limited due to the scarcity of digital resources, complex gene-trait relationships, and the lack of standardized benchmarks. To address this gap, we introduce SeedBench{---}the first multi-task benchmark specifically designed for seed science. Developed in collaboration with domain experts, SeedBench focuses on seed breeding and simulates key aspects of modern breeding processes. We conduct a comprehensive evaluation of 26 leading LLMs, encompassing proprietary, open-source, and domain-specific fine-tuned models. Our findings not only highlight the substantial gaps between the power of LLMs and the real-world seed science problems, but also make a foundational step for research on LLMs for seed design."
}

```

```

```
