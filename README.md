<h1 align="center">ACG-WAM</h1>
<h3 align="center">World-Action Modeling via Action-Conditioned Geometric Latent Prediction</h3>

<p align="center">
  <a href="README.md">English</a> | <a href="README.zh-CN.md">简体中文</a>
</p>

<p align="center">
  Jiangtao Liu<sup>1,2,*</sup>, Zishang Xiang<sup>1,*</sup>, Yage He<sup>2</sup>, Lingguo Cui<sup>1</sup>,<br>
  Baihai Zhang<sup>1</sup>, Runqi Chai<sup>1</sup>, and Senchun Chai<sup>1,†</sup><br>
  <sup>1</sup>School of Automation, Beijing Institute of Technology &nbsp; <sup>2</sup>LimX Dynamics<br>
  <sub><sup>*</sup>Equal contribution. <sup>†</sup>Corresponding author: <a href="mailto:chaisc97@bit.edu.cn">chaisc97@bit.edu.cn</a>.</sub>
</p>

<p align="center">
  <a href="https://RoboOpus.github.io/ACG-WAM/assets/paper/acg-wam.pdf"><img src="https://img.shields.io/badge/Paper-PDF-B31B1B" alt="Paper PDF"></a>
  <a href="https://RoboOpus.github.io/ACG-WAM/"><img src="https://img.shields.io/badge/Project-Website-2563EB" alt="Project website"></a>
  <a href="https://huggingface.co/RoboOpus/ACG-WAM"><img src="https://img.shields.io/badge/Model-HuggingFace-FFD21E" alt="Model on Hugging Face"></a>
  <a href="https://RoboOpus.github.io/ACG-WAM/#real-world"><img src="https://img.shields.io/badge/Robot-Demos-0F766E" alt="Real robot demos"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache--2.0-22C55E" alt="Apache 2.0 license"></a>
</p>

<p align="center">
  <img src="assets/readme/overview.webp" width="100%" alt="ACG-WAM overview: geometric supervision during training, the deployed policy, and benchmark results">
</p>

We introduce **ACG-WAM**, a world-action model built on [Motus](https://github.com/thu-ml/Motus) with action-conditioned geometric prediction during training and an unchanged backbone inference path.

<p align="center">
  <a href="#highlights">Highlights</a> ·
  <a href="#demonstrations">Videos</a> ·
  <a href="#performance">Results</a> ·
  <a href="#installation">Installation</a> ·
  <a href="#model-downloads">Models</a> ·
  <a href="#training">Training</a> ·
  <a href="#ablation-configurations">Ablations</a> ·
  <a href="#robotwin-evaluation">Evaluation</a> ·
  <a href="#citation">Citation</a>
</p>

## Highlights

- **Action-conditioned geometry.** ACG-JEPA predicts future features from observations and actions at horizons **1, 2, 4, and 8**.
- **Three-view supervision.** A frozen VGGT teacher uses head and wrist camera views to supervise shared visual features **before temporal mixing**.
- **Performance.** **93.07%** mean success across clean and randomized RoboTwin 2.0 settings; **85.00%** on three real robot tasks.
- **Training-only supervision.** Teacher targets are cached offline; no teacher, cache, adapter, or predictor is needed at deployment.

## Demonstrations

### Real Robot

Successful executions on **TRON2 with grippers**. Previews play at **2× speed** and link to original-speed videos.

<table>
  <tr>
    <th width="33%">Bimanual Fruit Placement</th>
    <th width="33%">Block Stacking</th>
    <th width="33%">Toy Placement into a Cup</th>
  </tr>
  <tr>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/1/IMG_6668.mp4"><img src="assets/readme/fruit-placement.gif" width="100%" alt="ACG-WAM places fruit into the target area using both arms"></a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/2/IMG_6661.mp4"><img src="assets/readme/block-stacking.gif" width="100%" alt="ACG-WAM stacks blocks with the left arm"></a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/3/IMG_6651.mp4"><img src="assets/readme/toy-placement.gif" width="100%" alt="ACG-WAM places a toy into a cup with the left arm"></a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/1/IMG_6668.mp4">▶ Demo 1</a> · <a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/1/IMG_6670.mp4">Demo 2</a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/2/IMG_6661.mp4">▶ Left arm</a> · <a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/2/IMG_6665.mp4">Right arm</a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/3/IMG_6651.mp4">▶ Left arm</a> · <a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/3/IMG_6649.mp4">Right arm</a></td>
  </tr>
</table>

### RoboTwin 2.0

[▶ Simulation videos](https://RoboOpus.github.io/ACG-WAM/#simulation) across six tasks in clean and randomized scenes.

## Performance

### RoboTwin 2.0 — 50 Tasks

Success rates (%) from [Table I](https://RoboOpus.github.io/ACG-WAM/assets/paper/acg-wam.pdf#page=5), with **100 episodes per task per setting**. Mean equally weights Clean and Randomized. Baseline sources are cited in the paper.

| Method | Clean | Randomized | Mean |
| :--- | ---: | ---: | ---: |
| GO-1 | 37.80 | 36.24 | 37.02 |
| π₀.₅ | 42.98 | 43.84 | 43.41 |
| X-VLA | 72.80 | 72.84 | 72.82 |
| Motus | 88.66 | 87.02 | 87.84 |
| LingBot-VA | 91.99 | 91.11 | 91.55 |
| MECo-WAM | 93.26 | 91.98 | 92.62 |
| WAM4D | **93.82** | 89.86 | 91.84 |
| **ACG-WAM (ours)** | 93.46 | **92.68** | **93.07** |

### Real Robot — Three Tasks

TRON2 results from [Table II](https://RoboOpus.github.io/ACG-WAM/assets/paper/acg-wam.pdf#page=7): **15k** fine-tuning updates, **100 demonstrations** and **20 evaluation trials per task**. SR is full task success; PCS is normalized partial completion.

| Method | Fruit Placement SR | Block Stacking SR | Toy into Cup SR | Average SR | Average PCS |
| :--- | ---: | ---: | ---: | ---: | ---: |
| π₀.₅ | 65.00 | 55.00 | 75.00 | 65.00 | 75.00 |
| LingBot-VA | 75.00 | 65.00 | **95.00** | 78.33 | 84.17 |
| Motus | 70.00 | 70.00 | 85.00 | 75.00 | 82.50 |
| **ACG-WAM (ours)** | **85.00** | **75.00** | **95.00** | **85.00** | **91.67** |

These results use task-specific fine-tuning; the public checkpoint is the **RoboTwin 40k policy**.

<details>
<summary><b>Ablations: geometric targets, action conditioning, and prediction horizons</b></summary>

Six-task ablations: **8k updates**, 100 evaluation episodes per task per setting (Tables III–IV). Recipes: [Ablation Configurations](#ablation-configurations).

| Variant | Clean | Randomized | Mean |
| :--- | ---: | ---: | ---: |
| Joint, short horizon {1} | 79.33 | 79.17 | 79.25 |
| Joint, long horizon {8} | 82.17 | 80.67 | 81.42 |
| Endpoint residual | 79.83 | 81.00 | 80.42 |
| Joint, no actions | 83.00 | 80.83 | 81.92 |
| **Joint, action-conditioned, horizons {1, 2, 4, 8}** | **84.67** | **82.67** | **83.67** |

</details>

---

## Installation

Requirements: Linux, Python 3.10, PyTorch 2.7.1, CUDA 12.8, and a compatible compiler for FlashAttention. Training examples use four NVIDIA GPUs with ZeRO-1.

```bash
git clone https://github.com/RoboOpus/ACG-WAM.git
cd ACG-WAM

conda create --prefix "$PWD/.venv" python=3.10 -y
conda activate "$PWD/.venv"

python -m pip install --upgrade pip setuptools wheel packaging ninja
python -m pip install torch==2.7.1 torchvision==0.22.1 --index-url https://download.pytorch.org/whl/cu128
python -m pip install flash-attn --no-build-isolation
python -m pip install -r requirements.txt huggingface_hub
```

Dependencies: [`requirements.txt`](requirements.txt). Run the following commands from the repository root.

## Model Downloads

### Released ACG-WAM Policy

| Checkpoint | Download | Training recipe |
| :--- | :---: | :--- |
| **ACG-WAM-RoboTwin-40K** | [🤗 Hugging Face](https://huggingface.co/RoboOpus/ACG-WAM) | Joint ACG-JEPA · clean + randomized · 40k updates |

```bash
hf download RoboOpus/ACG-WAM --local-dir ./pretrained_models/ACG-WAM
```

The **16.09 GB** download contains model weights and configuration only. Load the **containing directory**, not an individual weight file. Exact resume requires a full training-state checkpoint.

### Foundation Models

WAN and Qwen assets are required for training and evaluation:

```bash
hf download Wan-AI/Wan2.2-TI2V-5B --local-dir ./pretrained_models/Wan2.2-TI2V-5B
hf download Qwen/Qwen3-VL-2B-Instruct --local-dir ./pretrained_models/Qwen3-VL-2B-Instruct
```

For training from Motus and generating teacher targets:

```bash
hf download motus-robotics/Motus --local-dir ./pretrained_models/Motus
hf download facebook/VGGT-1B --local-dir ./pretrained_models/VGGT-1B
```

## Data Preparation

Store converted RoboTwin data under `<dataset>/{clean,randomized}/<task>/`, with matching episode IDs:

- `qpos/*.pt`: 14-dimensional action/state tensors.
- `videos/*.mp4`: three-camera T-layout videos, with the main view in the top two-thirds and wrist views below.
- `umt5_wan/*.pt`: precomputed language embeddings.

Use [`data/robotwin2/robotwin_data_convert`](data/robotwin2/robotwin_data_convert) and the [upstream guide](https://github.com/thu-ml/Motus/tree/main/data/robotwin2/robotwin_data_convert). Keep video/action alignment and generate the Joint cache from the same training videos.

## Training Configuration

Main recipe: [`configs/robotwin_joint_full_from_motus_40k.yaml`](configs/robotwin_joint_full_from_motus_40k.yaml), with [`configs/zero1.json`](configs/zero1.json). It uses **Joint ACG-JEPA**, initialized from Motus with `load_full_checkpoint: false`.

Update the dataset, foundation-model, Motus initialization, and teacher-cache paths in the YAML before running.

## Joint Teacher Cache

### Generate the Cache

Generate the cache after data preparation and path configuration:

```bash
CUDA_VISIBLE_DEVICES=0 python tools/precompute_vggt_cache.py \
    --config configs/robotwin_joint_full_from_motus_40k.yaml \
    --cache-dir ./cache/vggt_teacher_robotwin_joint_future_slot_v1 \
    --vggt-checkpoint ./pretrained_models/VGGT-1B \
    --target-mode joint_future_slot \
    --batch-size 8
```

### Audit Before Training

```bash
CUDA_VISIBLE_DEVICES=0 python tools/audit_joint_vggt_cache.py \
    --config configs/robotwin_joint_full_from_motus_40k.yaml \
    --cache-dir ./cache/vggt_teacher_robotwin_joint_future_slot_v1 \
    --vggt-checkpoint ./pretrained_models/VGGT-1B \
    --num-windows 32
```

  Keep source videos for auditing. **Use a Joint cache matching the training data and teacher protocol**, and regenerate it after changes; an endpoint-residual cache is incompatible.

## Training

  After the cache passes the audit:

```bash
torchrun --standalone --nproc_per_node=4 train/train.py \
    --deepspeed configs/zero1.json \
    --config configs/robotwin_joint_full_from_motus_40k.yaml \
    --run_name joint_full_seed42 \
    --report_to tensorboard
```

| Training setting | Value |
| --- | --- |
| Per-GPU micro-batch | 4 |
| GPUs in the command above | 4 |
| Gradient accumulation | 16 micro-batches |
| Effective global batch | 256 |
| Optimizer updates | 40,000 |
| Learning rate | `5e-5` |
| Warmup updates | 200 |
| Seed | 42 |
| Save interval | 20,000 updates |

Outputs: `checkpoints/robotwin_joint_full_from_motus_40k/joint_full_seed42/`.

## Ablation Configurations

Task lists follow `dataset.task_names`; historical `_10task` filenames refer to the six-task variants.

| Recipe | Configuration | Target | Horizons | Actions in JEPA | Updates / tasks |
| --- | --- | --- | --- | --- | --- |
| Joint baseline | [`robotwin_joint_10task_8k.yaml`](configs/robotwin_joint_10task_8k.yaml) | Joint | 1, 2, 4, 8 | Full | 8k / six |
| Short only | [`robotwin_short_only_10task_8k.yaml`](configs/robotwin_short_only_10task_8k.yaml) | Joint | 1 | Full | 8k / six |
| Long only | [`robotwin_long_only_10task_8k.yaml`](configs/robotwin_long_only_10task_8k.yaml) | Joint | 8 | Full | 8k / six |
| Endpoint residual | [`robotwin_residual_10task_8k.yaml`](configs/robotwin_residual_10task_8k.yaml) | Residual | 1, 2, 4, 8 | Full | 8k / six |
| No-Action, original recipe | [`robotwin_no_action.yaml`](configs/robotwin_no_action.yaml) | Residual | 1, 2, 4, 8 | Zero | 40k / all |
| No-Action, aligned Joint recipe | [`robotwin_joint_no_action_6task_8k.yaml`](configs/robotwin_joint_no_action_6task_8k.yaml) | Joint | 1, 2, 4, 8 | Zero | 8k / six |

## RoboTwin Evaluation

Install [RoboTwin](https://github.com/RoboTwin-Platform/RoboTwin) and the policy dependencies. Deployment code is in [`inference/robotwin/Motus`](inference/robotwin/Motus), retaining **`Motus`** as its internal name for compatibility.

Copy the policy into an unused destination:

```bash
export ROBOTWIN_ROOT=/absolute/path/to/RoboTwin
test -d "$ROBOTWIN_ROOT/policy" &&
test ! -e "$ROBOTWIN_ROOT/policy/Motus" &&
cp -a inference/robotwin/Motus "$ROBOTWIN_ROOT/policy/Motus"
```

If `policy/Motus` exists, back it up or merge first. Update the **copied** [`paths_config.yml`](inference/robotwin/Motus/paths_config.yml):

```yaml
robotwin_root: /absolute/path/to/RoboTwin
conda_env: /absolute/path/to/your/compatible/conda/environment
checkpoint_path: /absolute/path/to/ACG-WAM/pretrained_models/ACG-WAM
wan_path: /absolute/path/to/ACG-WAM/pretrained_models/Wan2.2-TI2V-5B
vlm_path: /absolute/path/to/ACG-WAM/pretrained_models/Qwen3-VL-2B-Instruct
task_config: demo_randomized
seed: 42
```

Point `checkpoint_path` to the directory containing `mp_rank_00_model_states.pt`, set `TASK_NAME` and `GPU_ID` in the copied [`eval.sh`](inference/robotwin/Motus/eval.sh), then run:

```bash
bash "$ROBOTWIN_ROOT/policy/Motus/eval.sh"
```

## Citation

```bibtex
@unpublished{liu2026acgwam,
  title  = {ACG-WAM: World-Action Modeling via Action-Conditioned Geometric Latent Prediction},
  author = {Liu, Jiangtao and Xiang, Zishang and He, Yage and Cui, Lingguo and Zhang, Baihai and Chai, Runqi and Chai, Senchun},
  year   = {2026},
  note   = {Manuscript},
  url    = {https://RoboOpus.github.io/ACG-WAM/}
}
```

## Acknowledgments and License

We thank the authors of [Motus](https://github.com/thu-ml/Motus), [WAN](https://github.com/Wan-Video/Wan2.2), [Qwen3-VL](https://github.com/QwenLM/Qwen3-VL), [VGGT](https://github.com/facebookresearch/vggt), and [RoboTwin](https://github.com/RoboTwin-Platform/RoboTwin) for making their work publicly available.

We release our repository code under the [Apache-2.0 license](LICENSE). Upstream weights and datasets remain subject to their respective terms.
