# EquiliRes — anonymized reproducibility package

This archive contains the evaluation code, graph inputs, and per-run result logs used for the ICLR 2027 submission and rebuttal. It is prepared for anonymous review. No author names, local machine paths, Git metadata, or personal email addresses are intentionally included.

## Contents

- `code/`: EquiliRes (Algorithm 2), evaluation harness, GCN-Jaccard, GNNGuard, RUNG adapters, adaptive-attack generator, and the additional rebuttal mechanism analysis.
- `load_datasets/`: the dataset loader used to match the Pro-GNN/LCC benchmark protocol.
- `data/nofeat/graph1` … `graph10`: the 10 independently attacked graph sets used by the topology-only paper protocol for Cora, Citeseer, Cora-ML, and Polblogs.
- `data/official/`: clean feature-bearing benchmark data, fixed Pro-GNN splits, Metattack/DICE graphs, generated CE-PGD graphs, and Photo inputs used in the reported feature-bearing/standard-setting evaluations.
- `results/`: selected JSONL per-run logs and diagnostics associated with paper/rebuttal tables. Logs are retained as run-level data; summary values can be recomputed from them.
- `ext/RUNG/`: RUNG implementation needed by its included adapter. `ext/goodat_sep/` contains small supporting source files associated with archived GOOD-AT runs; the GOOD-AT run driver is not part of the current generic evaluation CLI.
- `ENVIRONMENT.md`: software versions and reproducibility notes.

Generated purification caches and exploratory OGB/arXiv results are omitted; they can be regenerated and are not inputs to the tables covered here.

## Setup

Use Python 3.12 and install dependencies listed in `requirements.txt`. PyTorch should match the target CPU/CUDA environment. The reported results were produced with PyTorch 2.2.1 and the package versions listed in `ENVIRONMENT.md`.

The code and bundled data are relocatable. Run commands from this directory; output logs should be written under `results/`.

## Reproduce a small sanity run

CPU, one seed, one rate:

```bash
CUDA_VISIBLE_DEVICES="" python code/run_eval.py \
  --datasets cora --attacks meta --rates 0.05 \
  --methods gcn equ_auto_b0.3 sjac_auto_b0.3 \
  --features identity --seeds 1 --seed0 0 --workers 1 \
  --source nofeat --device cpu --out results/sanity.jsonl
```

For the standard feature-bearing setting:

```bash
CUDA_VISIBLE_DEVICES="" python code/run_eval.py \
  --datasets cora --attacks meta --rates 0.05 \
  --methods gcn equ_auto_b0.3 jac_t0.03 svd_k100 \
  --features standard --seeds 1 --seed0 0 --workers 1 \
  --source official --device cpu --out results/sanity_official.jsonl
```

The ten-graph topology-only protocol uses `--source nofeat --seeds 10`. The official split setting uses `--source official`. Results are stochastic across training seeds; reproduce aggregate tables with the same datasets, attacks, selected validation-set hyperparameters, and seed ranges recorded in the paper/logs.

## Recompute the ASEP-mechanism measurements

```bash
CUDA_VISIBLE_DEVICES="" python code/z13_mech_homophily.py polblogs 3 0.5
CUDA_VISIBLE_DEVICES="" python code/z13_mech_homophily.py cora 3 0.5
```

The script writes `results/z13_mech_homophily.jsonl`. Node labels are used only for post-hoc diagnostic metrics (edge homophily and inserted-edge identification); the EquiliRes method itself does not use labels.

## Notes

- The packaged source is the experiment implementation used by this submission/rebuttal; it also contains scripts for analyses that are not in the main paper tables. Consult each script's docstring and the per-run JSONL fields for its exact protocol.
- Some rebuttal tables use runs performed with external baselines or teacher-provided logs. Their source logs are included only where available; the `results/` directory is not a guarantee that every externally reported cell has an independently archived raw log.
- Use the graph splits and attack files shipped here rather than regenerating attacks if exact table reproduction is desired.
