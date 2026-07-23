# Dynamic Supervised Pseudo-label Diffusion for Single-cell Clustering

This repository provides a simple PyTorch implementation of **Dynamic Supervised Pseudo-label Diffusion (DSPD)** for single-cell clustering.

The method uses a small number of known labels to guide representation learning. During training, Leiden clustering is repeatedly applied to the latent space, and reliable cluster-level pseudo-labels are dynamically propagated to improve clustering performance.

## Files

```text
model.py    # Autoencoder, reconstruction loss, supervised contrastive loss
train.py    # Data loading, preprocessing, training, clustering, evaluation
```

## Requirements

```bash
pip install torch numpy pandas h5py scanpy scikit-learn scipy matplotlib
```

## Data Format

The input data should be an `.h5` file containing:

```text
X    # expression matrix, cells × genes
Y    # ground-truth labels
```

Example:

```text
data/example.h5
├── X
└── Y
```

## Usage

```bash
python train.py \
  --data_file ./data/example.h5 \
  --label_ratio 0.1 \
  --epochs 200 \
  --save_dir results/example
```

## Main Arguments

```text
--data_file          path to input h5 file
--label_ratio        ratio of known labeled cells
--z_dim              latent dimension
--epochs             number of training epochs
--purity_threshold   threshold for pseudo-label propagation
--update_interval    interval for updating pseudo-labels
--save_dir           directory for saving results
--device             cuda or cpu
```

## Outputs

After training, results are saved in `save_dir`:

```text
final_latent.csv    # learned latent representations
pred_labels.txt     # predicted cluster labels
umap.png            # UMAP visualization
```

The script also reports:

```text
ACC, NMI, ARI
```

## Example Output

```text
Evaluating cells: ACC=0.XXXX, NMI=0.XXXX, ARI=0.XXXX
Saved to results/example
```

## Note

Before running, make sure the uploaded files are named:

```text
model.py
train.py
```

because `train.py` imports modules from `model.py`.
