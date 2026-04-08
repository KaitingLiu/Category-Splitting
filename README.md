# Let's Split Up: Zero-Shot Classifier Edits for Fine-Grained Video Understanding
![Category-Splitting Concept Figure](https://github.com/KaitingLiu/kaitingliu.github.io/blob/main/Category-Splitting/static/images/concept_v2-3.jpg)

## Requirements

For running the code, download this reprository and create the environment:

```bash
conda create -n category-splitting python=3.10
conda activate category-splitting
```

Then install packages:

```bash
pip install -r requirements.txt
```

The code is tested with CUDA 12.1.

---

## Benchmark

To use the benchmark, first download the video data:

* <a href="https://www.qualcomm.com/developer/software/something-something-v-2-dataset" target="_blank">SSv2 video samples</a> [1]
* <a href="https://sdolivia.github.io/FineGym/" target="_blank">FineGym288 video samples</a> [2]

Then use our annotation files:
* <a href="https://github.com/KaitingLiu/Category-Splitting/tree/main/benchmark" target="_blank">Annotation files</a>

---

## Reproducing Results
To reproduce the results in the paper, first download the <a href="" target="_blank">mixed-granularity base models</a>.  
These models are used as the starting point for all category splitting experiments.

Before running the scripts, place the downloaded files in the following directories:

Video data:
* ./video/finegym/ for FineGym videos
* ./video/ssv2/ for SSv2 videos
Mixed-granularity base models:
* ./checkpoints/ for all checkpoint files

### Table 2 (Comparative Zero-Shot Results)

Run the scripts for:

* <a href="" target="_blank">SSv2-Split-A</a>
* <a href="" target="_blank">SSv2-Split-B</a>
* <a href="" target="_blank">FineGym-Split-A</a>
* <a href="" target="_blank">FineGym-Split-B</a>

After all runs are completed, compute the average results:

```bash
python summery.py ./output/Table2/SSv2-Split-A/ma
python summery.py ./output/Table2/SSv2-Split-B/ma
python summery.py ./output/Table2/FineGym-Split-A/ma
python summery.py ./output/Table2/FineGym-Split-B/ma
```

---

### Table 3 (Zero-Shot Ablation)

Run the script for:

* <a href="" target="_blank">SSv2-Split-A</a>

After all runs are completed, compute the average results:

```bash
python summery.py ./output/Table3/SSv2-Split-A/vlm
python summery.py ./output/Table3/SSv2-Split-A/mr
python summery.py ./output/Table3/SSv2-Split-A/ma
```

---

### Table 4 (One-Shot Finetuning Ablation)

For the last three rows (results for different initialization methods), run the script for:

* <a href="" target="_blank">SSv2-Split-A</a>

After all runs are completed, compute the average results:

```bash
python summery.py ./output/Table4/SSv2-Split-A/ft_random
python summery.py ./output/Table4/SSv2-Split-A/ft_coarse_grained_class_weight
python summery.py ./output/Table4/SSv2-Split-A/ft_ma
```

---

## Citation

If you use this dataset, please cite our paper.

```bibtex
@article{liu2026let,
  title={Let's Split Up: Zero-Shot Classifier Edits for Fine-Grained Video Understanding},
  author={Liu, Kaiting and Doughty, Hazel},
  journal={arXiv preprint arXiv:2602.16545},
  year={2026}
}
```

---
## Reference
[1] Goyal, Raghav, et al. "The" something something" video database for learning and evaluating visual common sense." Proceedings of the IEEE international conference on computer vision. 2017.

[2] Shao, Dian, et al. "Finegym: A hierarchical video dataset for fine-grained action understanding." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2020.