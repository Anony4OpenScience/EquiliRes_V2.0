# Environment

Versions observed in the experiment environment:

- Python 3.12
- NumPy 1.26.4; SciPy 1.13.1; pandas 2.3.2; scikit-learn 1.7.0; NetworkX 3.5; Numba 0.61.2
- PyTorch 2.2.1; PyTorch Geometric 2.6.1; DeepRobust 0.2.11; OGB 1.3.6
- texttable 1.7.0; tensorboardX 2.6.4; tqdm 4.67.1

CUDA acceleration was used for some experiments; CPU execution is supported but can be slower. For CUDA, install a PyTorch build compatible with the local CUDA runtime and the matching PyG extension wheels. The original environment had CUDA 12.1 PyG extensions; hardware-specific wheel URLs are intentionally not embedded here.
