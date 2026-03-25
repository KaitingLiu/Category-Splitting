# 📦 Category Splitting Benchmark

This repository provides the **label files and grouping schemes** for the category splitting benchmarks introduced in our paper *Let's Split Up: Zero-Shot Classifier Edits for Fine-Grained Video Understanding*.

⚠️ **Note:** We do not redistribute raw videos. Please download the original datasets from their official sources.

---

## 📚 Datasets

* **SSv2-Split** (based on Something-Something V2 dataset [1])
* **FineGym-Split** (based on FineGym288 dataset [2])  
  For Original FineGym288, some videos are missing and cannot be downloaded from the web. These missing samples are therefore not included in our dataset.

  In the original FineGym288 labels ([original labels](https://sdolivia.github.io/FineGym/resources/dataset/gym288_categories.txt)), there are three groups of text labels that are exactly duplicated, differing only by their index:

  - `(BB) salto backward tucked with 1 twist`: Clabels 180 and 216  
  - `(BB) salto backward tucked`: Clabels 181 and 217  
  - `(BB) salto backward stretched with 1 twist`: Clabels 185 and 222  

  We kept only the samples with Clabels 216, 217, and 222, and removed the others. Our dataset was then constructed based on the remaining samples.
---

## 🧠 Overview
We take SSv2-Split as an example below; FineGym-Split follows the same construction.

We reorganize the labels of the SSv2 dataset into a **coarse-to-fine structure**:

- **Coarse categories**: each coarse category is formed by grouping a set of semantically related fine-grained action labels  
- **Fine-grained categories**: original action labels  

All fine-grained categories are assigned to exactly one coarse category.

We then partition the **coarse categories** into two complementary subsets, A and B. Based on this partition, we construct two **mixed-granularity datasets**:

- **Dataset A**: fine-grained categories within coarse categories in subset A are **collapsed into coarse labels** (to be split), while those in subset B remain **fine-grained**
- **Dataset B**: fine-grained categories within coarse categories in subset B are **collapsed into coarse labels** (to be split), while those in subset A remain **fine-grained**

---

## 📁 Structure

```
Dataset/
├── SSv2-Split/
│   ├── group_scheme.json
│   ├── A/
│   │   ├── label.json
│   │   ├── train.csv / val.csv
│   │   ├── <coarse_label>/
│   │   │   ├── ft_set.csv
│   │   │   ├── equivalent_set.csv
│   │   │   ├── unrelated_set.csv
│   ├── B/
│   │   ├── (same structure as A)
│
├── FineGym-Split/
│   ├── (same structure as SSv2-Split)
```

---

## 🔑 Grouping Scheme

File: `group_scheme.json`

```json
{
  "group_scheme": {
    "<coarse_label>": ["fine_grained_label_1", "fine_grained_label_2", ...]
  },
  "A": [...],
  "B": [...]
}
```

* `group_scheme`: mapping from **coarse → fine grained labels**
* `A`, `B`: partition of **coarse categories** into two complementary subsets, determining which categories are collapsed into coarse labels (splitting targets) in Dataset A and Dataset B

---

## 🔀 Mixed-Granularity Datasets

Each dataset (`A/`, `B/`) contains:

File: `label.json`

Mapping from **label name → index**

File: `train.csv / val.csv` has format:

```
video_path,label_index
```

They are used to train and evaluate a **mixed-granularity base model**, where coarse and fine-grained labels coexist. This base model serves as the starting point for category splitting task.

We also provide pretrained **mixed-granularity base model** checkpoints, fine-tuned from the MVD ViT-Small model [3] ([mvd_s_from_l_ckpt_399.pth](https://drive.google.com/file/d/1HqvGxx7_JYO5JKvRT0giesl-p-Iaaesa/view)), at: [mixed-granularity base model checkpoints](https://github.com/KaitingLiu/Category-Splitting/releases/tag/v1.0)

---

## 🎯 Category Splitting Evaluation

Each dataset (`A/`, `B/`) also contains a folder for each coarse category used for category splitting evaluation:

```
<coarse_label>/
├── ft_set.csv
├── equivalent_set.csv
├── unrelated_set.csv
```

### Files

All files (`ft_set.csv`, `equivalent_set.csv`, `unrelated_set.csv`) follow the format:

```
video_path,label_index
```

- **`ft_set.csv`**  
  - Training samples from the coarse category corresponding to the folder name, derived from `train.csv`, labeled with fine-grained category indices
  - Used for category splitting methods that require fine-grained supervision

- **`equivalent_set.csv`**  
  - Validation samples from the same coarse category, derived from val.csv`, labeled with fine-grained category indices
  - Used to evaluate **generality** of the category splitting method  

- **`unrelated_set.csv`**  
  - Validation samples from all the untouched categories, derived from `val.csv`, with their original indices in `val.csv`
  - Used to evaluate **locality** of the category splitting method

Fine-grained category indices in these files extend the label space defined in `label.json`, starting from the next available index (i.e., if the last index in `label.json` is N, these indices start from N+1)

---

## 📌 Citation

If you use this dataset, please cite our paper.

---
## 📌 Reference
[1] Goyal, Raghav, et al. "The" something something" video database for learning and evaluating visual common sense." Proceedings of the IEEE international conference on computer vision. 2017.

[2] Shao, Dian, et al. "Finegym: A hierarchical video dataset for fine-grained action understanding." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2020.

[3] Wang, Rui, et al. "Masked video distillation: Rethinking masked feature modeling for self-supervised video representation learning." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2023.
