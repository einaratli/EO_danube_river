# RAF622M — Land Cover Classification from Sentinel-2 Imagery

End-to-end pipeline for land cover classification using multi-temporal Sentinel-2 imagery and CORINE Land Cover 2018 labels over MGRS tile T33TYN (Danube basin, Central Europe). Course project for RAF622M — Machine Learning for Earth Observation with Supercomputers, University of Iceland, Spring 2026.

## Repository Structure
EO_danube_river/
├── notebooks/
│   ├── data_gathering.ipynb                     # Sentinel-2 download from Copernicus
│   ├── data_preprocessing - checkpoint 2.ipynb  # Band stacking and CORINE alignment
│   └── final_experiment.ipynb                   # CNN baseline + ResNet final model
├── experiments/
│   ├── column_split.ipynb                       # Vertical split experiment (abandoned)
│   ├── lab6.ipynb                               # TerraTorch/Prithvi experiment
│   └── visulation.ipynb                         # Sentinel-2 RGB visualization
├── figures/
│   ├── alignment_64x64_blocks.png
│   ├── alignment_S2B_MSIL2A_...png
│   ├── class_distribution.png
│   ├── confusion_matrix.png
│   └── prediction_samples.png
├── data_gathering.ipynb                         # (checkpoint 1)
├── checkpoint1.ipynb                            # (checkpoint 1)
└── visualization.ipynb                          # (checkpoint 1)
## Dataset

Data is stored on JURECA HPC system at Jülich Supercomputing Centre. Contact the authors for access permissions.

- Aligned Sentinel-2 + CORINE data: `/p/scratch/training2600/TeamGudnason/data/aligned_data/`
- Training patches (64×64, block split): `/p/scratch/training2600/TeamGudnason/training_data/final_block_64/`
- Results and figures: `/p/scratch/training2600/TeamGudnason/results/`

## How to Reproduce

### Environment
source /p/project1/training2600/TeamGudnason/envs/danube/bin/activate
module load Python/3.12.3-GCCcore-13.3.0

### 1. Data Acquisition

Run `notebooks/data_gathering.ipynb` to download Sentinel-2 scenes from Copernicus Dataspace.

### 2. Preprocessing

Run `notebooks/data_preprocessing - checkpoint 2.ipynb` to align CORINE labels and stack bands into multi-band GeoTIFFs.

### 3. Patch Extraction and Training

Run `notebooks/final_experiment.ipynb` on a GPU node. The notebook contains the patch extraction with block cross-validation, the CNN baseline (random split), and the final ResNet model (block cross-validation).

## Main Results

| Model | Split | Overall Acc. | Balanced Acc. | Macro-F1 |
| --- | --- | --- | --- | --- |
| CNN 3×3 | Random | 67.1% | 32.1% | 34.3% |
| CNN 3×3 | Block geo | 46.0% | 6.0% | 4.0% |
| **ResNet 64×64** | **Block geo** | **88.6%** | **81.0%** | **80.5%** |

## Dependencies

- Python 3.12
- PyTorch + Lightning
- rasterio, numpy, matplotlib
- JURECA GPU node (NVIDIA A100)
