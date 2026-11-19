EquiliRes Reproducibility Package

1. Environment

Use Python 3.12. Install the dependencies in requirements.txt. To run the optional RUNG baseline, also install requirements-baselines.txt. The tested software versions are listed in ENVIRONMENT.md. The PyTorch build must match your CPU/CUDA setup.

Run all commands from this directory. All data paths are relative to it.

2. Topology-only setting (as in the paper)

The topology-only protocol uses 10 independently generated attacked graphs. The following command runs one random seed on the first graph:

CUDA_VISIBLE_DEVICES="" python code/run_eval.py \
  --datasets cora --attacks meta --rates 0.05 \
  --methods gcn equ_auto_b0.3 sjac_auto_b0.3 \
  --features identity --seeds 1 --seed0 0 --workers 1 \
  --source nofeat --device cpu --out results/cora_nofeat.jsonl

To run all 10 seeds/graphs, change --seeds 1 to --seeds 10. The program uses data/nofeat/graph1 to graph10 in turn.

3. Standard setting with node features

CUDA_VISIBLE_DEVICES="" python code/run_eval.py \
  --datasets cora --attacks meta --rates 0.05 \
  --methods gcn equ_auto_b0.3 jac_t0.03 svd_k100 \
  --features standard --seeds 1 --seed0 0 --workers 1 \
  --source official --device cpu --out results/cora_official.jsonl

To run 10 training seeds on the fixed official split, change --seeds 1 to --seeds 10.

Available datasets: cora, citeseer, cora_ml, polblogs, photo (depending on the graphs and attack files included in data/).
Available methods: gcn, equ_auto_b<B>, sjac_auto_b<B>, jac_t<T>, svd_k<K>, gnnguard, rung.
(<B> is the budget b, <T> the Jaccard threshold, <K> the SVD rank.)

4. ASEP mechanism analysis

CUDA_VISIBLE_DEVICES="" python code/z13_mech_homophily.py polblogs 3 0.5
CUDA_VISIBLE_DEVICES="" python code/z13_mech_homophily.py cora 3 0.5

Per-graph diagnostics are written to results/z13_mech_homophily.jsonl. Node labels are used only for post-hoc diagnostics (edge homophily and inserted-edge identification); the EquiliRes algorithm itself does not use labels.

5. Included results

results/ contains per-run JSONL logs and diagnostic results, from which the mean accuracy and standard deviation can be recomputed. The fixed data splits and attacked graphs are in data/; use these files to reproduce the paper's results.
