# 📦 Category Splitting Benchmark

This repository provides the **label files and grouping schemes** for the category splitting benchmarks introduced in our paper *Let's Split Up: Zero-Shot Classifier Edits for Fine-Grained Video Understanding*.

⚠️ **Note:** We do not redistribute raw videos. Please download the original datasets from their official sources.

---

## 📚 Datasets

* **SSv2-Split** (based on Something-Something V2 dataset)
* **FineGym-Split** (based on FineGym288 dataset)

Both datasets follow the same structure.

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
│   │   ├── train.csv / val.csv / test.csv
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
* `A`, `B`: partition of coarse categories used for mixed training

---

## 🔀 Mixed-Granularity Labels

Each subset (`A/`, `B/`) contains:

### `label.json`

Mapping from **label name → index**

### `train.csv / val.csv / test.csv`

Format:

```
video_path,label_index
```

* Labels are **mixed-granularity**
* Active set → **coarse labels**
* Others → **fine labels**

---

## 🎯 Category Splitting Evaluation

Each coarse category has its own folder:

```
<coarse_label>/
├── ft.csv
├── equivalent.csv
├── unrelated.csv
```

### Files

* **ft.csv**

  * Training samples for splitting
  * Fine-grained labels within the coarse group

* **equivalent.csv**

  * Test samples from the same coarse category
  * Used to evaluate splitting performance

* **unrelated.csv**

  * Test samples outside the group
  * Used to evaluate robustness

---

## 🔢 Label Indexing

* Indices in splitting files **continue from** `label.json`
* They extend the base model label space

---

## 📌 Citation

If you use this dataset, please cite our paper.

---
