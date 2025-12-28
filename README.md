# Event-based State Estimator

This project estimates position and velocity using monocular event-based visual odometry.

## 1. Clone this repository

Clone this repository **with submodules** (DEVO and its third-party submodules):

```bash
git clone --recursive https://github.com/jczekanska/event-based-state-estimator.git
cd event-based-state-estimator
```

## 2. Setup

**Tested configuration:**
- Ubuntu 22.04
- ROS 2 Humble
- CUDA 11.8

The DEVO framework uses **Anaconda** to manage the Python environment.

## 3. DEVO Installation

Follow the official **[DEVO repository setup instructions](https://github.com/tum-vision/DEVO/tree/main)**.

A summarized version is provided below.

### Create and activate the Conda environment

```bash
cd DEVO
conda env create -f environment.yml
conda activate devo
```

### Install dependencies and DEVO package

Download Eigen:

```bash
wget https://gitlab.com/libeigen/eigen/-/archive/3.4.0/eigen-3.4.0.zip
unzip eigen-3.4.0.zip -d thirdparty
```

Install DEVO:

```bash
pip install .
```

## 4. Pretrained Model

Download the pretrained model in the **DEVO directory**:

```bash
./download_model.sh
```

This will download `DEVO.pth` (~40 MB).

## 5. Dataset

The code has been tested on the following RPG event camera rosbag files:

- `bin.bag`
- `boxes2.bag`
- `monitor2.bag`
- `reader.bag`

After downloading the bags from the [RPG Event Camera Dataset](https://rpg.ifi.uzh.ch/ECCV18_stereo_davis.html),
place them in the following directory:

```
datasets/rpg
```

## 6. Preprocessing the RPG dataset

Run the preprocessing script from the **DEVO directory**:

```bash
python scripts/pp_rpg.py --indir ../datasets/rpg
```

## 7. Evaluating RPG Events

Run the evaluation script from the **DEVO directory**:

```bash
python evals/eval_evs/eval_rpg_evs.py \
  --datapath ../datasets/rpg \
  --weights DEVO.pth \
  --stride 1 \
  --trials 1 \
  --expname my_evs_test
```

## Third-Party

This repository uses the [DEVO](https://github.com/tum-vision/DEVO) framework for event-based visual odometry:

DEVO is included as a Git submodule and contains the 2 local commits with modifications applied. Its own third-party submodules (`rpg_trajectory_evaluation` and `tartanair_tools`) are also automatically cloned with `--recursive`.
