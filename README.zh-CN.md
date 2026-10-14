<h1 align="center">ACG-WAM</h1>
<h3 align="center">基于动作条件几何潜在表征预测的世界-动作建模</h3>

<p align="center">
  <a href="README.md">English</a> | <a href="README.zh-CN.md">简体中文</a>
</p>

<p align="center">
  Jiangtao Liu<sup>1,2,*</sup>, Zishang Xiang<sup>1,*</sup>, Yage He<sup>2</sup>, Lingguo Cui<sup>1</sup>,<br>
  Baihai Zhang<sup>1</sup>, Runqi Chai<sup>1</sup>, Senchun Chai<sup>1,†</sup><br>
  <sup>1</sup>北京理工大学自动化学院 &nbsp; <sup>2</sup>LimX Dynamics（逐际动力）<br>
  <sub><sup>*</sup>同等贡献。<sup>†</sup>通讯作者：<a href="mailto:chaisc97@bit.edu.cn">chaisc97@bit.edu.cn</a>。</sub>
</p>

<p align="center">
  <a href="https://RoboOpus.github.io/ACG-WAM/assets/paper/acg-wam.pdf"><img src="https://img.shields.io/badge/Paper-PDF-B31B1B" alt="论文 PDF"></a>
  <a href="https://RoboOpus.github.io/ACG-WAM/"><img src="https://img.shields.io/badge/Project-Website-2563EB" alt="项目主页"></a>
  <a href="https://huggingface.co/RoboOpus/ACG-WAM"><img src="https://img.shields.io/badge/Model-HuggingFace-FFD21E" alt="Hugging Face 模型"></a>
  <a href="https://RoboOpus.github.io/ACG-WAM/#real-world"><img src="https://img.shields.io/badge/Robot-Demos-0F766E" alt="真机演示"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache--2.0-22C55E" alt="Apache 2.0 许可证"></a>
</p>

<p align="center">
  <img src="assets/readme/overview.webp" width="100%" alt="ACG-WAM 概览：训练期几何监督、部署策略和基准测试结果">
</p>

我们提出 **ACG-WAM**，在 [Motus](https://github.com/thu-ml/Motus) 上引入动作条件几何预测监督，保持主干推理路径不变。

<p align="center">
  <a href="#highlights">亮点</a> ·
  <a href="#demonstrations">视频</a> ·
  <a href="#performance">结果</a> ·
  <a href="#installation">安装</a> ·
  <a href="#model-downloads">模型</a> ·
  <a href="#training">训练</a> ·
  <a href="#ablation-configurations">消融</a> ·
  <a href="#robotwin-evaluation">评估</a> ·
  <a href="#citation">引用</a>
</p>

<a id="highlights"></a>

## 核心亮点

- **动作条件几何预测。** ACG-JEPA 根据观测与动作，预测跨度为 **1、2、4 和 8** 的未来几何特征。
- **三视角监督。** 冻结的 VGGT 教师模型利用头部与腕部相机视角，监督**时序混合之前**的共享视觉特征。
- **实验表现。** RoboTwin 2.0 干净与随机化场景平均成功率 **93.07%**；三项真机任务平均成功率 **85.00%**。
- **仅训练时使用。** 教师目标离线缓存，部署无需教师模型、缓存、适配器或预测器。

<a id="demonstrations"></a>

## 演示

### 真机

**配备夹爪的 TRON2** 任务演示。预览以 **2× 速度**播放，点击可查看原速视频。

<table>
  <tr>
    <th width="33%">双臂水果放置</th>
    <th width="33%">积木堆叠</th>
    <th width="33%">将玩具放入杯中</th>
  </tr>
  <tr>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/1/IMG_6668.mp4"><img src="assets/readme/fruit-placement.gif" width="100%" alt="ACG-WAM 使用双臂将水果放入目标区域"></a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/2/IMG_6661.mp4"><img src="assets/readme/block-stacking.gif" width="100%" alt="ACG-WAM 使用左臂堆叠积木"></a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/3/IMG_6651.mp4"><img src="assets/readme/toy-placement.gif" width="100%" alt="ACG-WAM 使用左臂将玩具放入杯中"></a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/1/IMG_6668.mp4">▶ 演示 1</a> · <a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/1/IMG_6670.mp4">演示 2</a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/2/IMG_6661.mp4">▶ 左臂</a> · <a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/2/IMG_6665.mp4">右臂</a></td>
    <td align="center"><a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/3/IMG_6651.mp4">▶ 左臂</a> · <a href="https://RoboOpus.github.io/ACG-WAM/assets/videos/real/3/IMG_6649.mp4">右臂</a></td>
  </tr>
</table>

### RoboTwin 2.0

[▶ 仿真视频](https://RoboOpus.github.io/ACG-WAM/#simulation)覆盖六项任务的干净与随机化场景。

<a id="performance"></a>

## 性能

### RoboTwin 2.0：50 项任务

成功率（%）见[论文表 I](https://RoboOpus.github.io/ACG-WAM/assets/paper/acg-wam.pdf#page=5)，**每项任务、每种设置评估 100 个回合**。总体平均值对干净与随机化场景等权计算，基线来源见论文。

| 方法 | 干净场景 | 随机化场景 | 平均值 |
| :--- | ---: | ---: | ---: |
| GO-1 | 37.80 | 36.24 | 37.02 |
| π₀.₅ | 42.98 | 43.84 | 43.41 |
| X-VLA | 72.80 | 72.84 | 72.82 |
| Motus | 88.66 | 87.02 | 87.84 |
| LingBot-VA | 91.99 | 91.11 | 91.55 |
| MECo-WAM | 93.26 | 91.98 | 92.62 |
| WAM4D | **93.82** | 89.86 | 91.84 |
| **ACG-WAM（我们的方法）** | 93.46 | **92.68** | **93.07** |

### 真机：三项任务

TRON2 结果见[论文表 II](https://RoboOpus.github.io/ACG-WAM/assets/paper/acg-wam.pdf#page=7)：微调 **15k 次更新**，每项任务使用 **100 条示范**并评估 **20 次**。SR 为任务完全成功率，PCS 为归一化的部分完成度评分。

| 方法 | 水果放置 SR | 积木堆叠 SR | 玩具入杯 SR | 平均 SR | 平均 PCS |
| :--- | ---: | ---: | ---: | ---: | ---: |
| π₀.₅ | 65.00 | 55.00 | 75.00 | 65.00 | 75.00 |
| LingBot-VA | 75.00 | 65.00 | **95.00** | 78.33 | 84.17 |
| Motus | 70.00 | 70.00 | 85.00 | 75.00 | 82.50 |
| **ACG-WAM（我们的方法）** | **85.00** | **75.00** | **95.00** | **85.00** | **91.67** |

真机结果采用任务专属微调；公开检查点为 **RoboTwin 40k 策略**。

<details>
<summary><b>消融实验：几何目标、动作条件与预测跨度</b></summary>

六任务消融：训练 **8k 次更新**，每项任务、每种设置评估 100 个回合（论文表 III、IV）。训练配置见[消融配置](#ablation-configurations)。

| 变体 | 干净场景 | 随机化场景 | 平均值 |
| :--- | ---: | ---: | ---: |
| Joint，短跨度 {1} | 79.33 | 79.17 | 79.25 |
| Joint，长跨度 {8} | 82.17 | 80.67 | 81.42 |
| 端点残差 | 79.83 | 81.00 | 80.42 |
| Joint，无动作条件 | 83.00 | 80.83 | 81.92 |
| **Joint，动作条件，预测跨度 {1, 2, 4, 8}** | **84.67** | **82.67** | **83.67** |

</details>

---

<a id="installation"></a>

## 安装

环境要求：Linux、Python 3.10、PyTorch 2.7.1、CUDA 12.8，以及兼容 FlashAttention 的编译器。训练示例使用四张 NVIDIA GPU 和 ZeRO-1。

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

依赖见 [`requirements.txt`](requirements.txt)。后续命令均在仓库根目录执行。

<a id="model-downloads"></a>

## 模型下载

### 公开发布的 ACG-WAM 策略

| 检查点 | 下载 | 训练配置 |
| :--- | :---: | :--- |
| **ACG-WAM-RoboTwin-40K** | [🤗 Hugging Face](https://huggingface.co/RoboOpus/ACG-WAM) | Joint ACG-JEPA · 干净场景 + 随机化场景 · 40k 次更新 |

```bash
hf download RoboOpus/ACG-WAM --local-dir ./pretrained_models/ACG-WAM
```

下载约 **16.09 GB**，仅含模型权重和配置。加载时传入**权重所在目录**，而非单个文件；精确续训需要完整训练状态检查点。

### 基础模型

训练和评估均需要 WAN 与 Qwen 资源：

```bash
hf download Wan-AI/Wan2.2-TI2V-5B --local-dir ./pretrained_models/Wan2.2-TI2V-5B
hf download Qwen/Qwen3-VL-2B-Instruct --local-dir ./pretrained_models/Qwen3-VL-2B-Instruct
```

从 Motus 初始化训练及生成教师目标还需要：

```bash
hf download motus-robotics/Motus --local-dir ./pretrained_models/Motus
hf download facebook/VGGT-1B --local-dir ./pretrained_models/VGGT-1B
```

## 数据准备

转换后的 RoboTwin 数据保存在 `<dataset>/{clean,randomized}/<task>/` 下，各模态回合编号保持一致：

- `qpos/*.pt`：14 维动作与状态张量。
- `videos/*.mp4`：三相机 T 形布局视频，主相机占上方三分之二，腕部相机位于下方。
- `umt5_wan/*.pt`：预计算语言嵌入。

转换入口见 [`data/robotwin2/robotwin_data_convert`](data/robotwin2/robotwin_data_convert) 和[上游指南](https://github.com/thu-ml/Motus/tree/main/data/robotwin2/robotwin_data_convert)。视频与动作需对齐，Joint 缓存必须使用同一批训练视频生成。

## 训练配置

主配置为 [`configs/robotwin_joint_full_from_motus_40k.yaml`](configs/robotwin_joint_full_from_motus_40k.yaml)，配合 [`configs/zero1.json`](configs/zero1.json) 使用。采用 **Joint ACG-JEPA**，以 `load_full_checkpoint: false` 从 Motus 初始化。

运行前，更新 YAML 中的数据集、基础模型、Motus 初始化权重及教师缓存路径。

<a id="joint-teacher-cache"></a>

## Joint 教师缓存

### 生成缓存

完成数据准备和路径配置后生成缓存：

```bash
CUDA_VISIBLE_DEVICES=0 python tools/precompute_vggt_cache.py \
    --config configs/robotwin_joint_full_from_motus_40k.yaml \
    --cache-dir ./cache/vggt_teacher_robotwin_joint_future_slot_v1 \
    --vggt-checkpoint ./pretrained_models/VGGT-1B \
    --target-mode joint_future_slot \
    --batch-size 8
```

### 训练前审计

```bash
CUDA_VISIBLE_DEVICES=0 python tools/audit_joint_vggt_cache.py \
    --config configs/robotwin_joint_full_from_motus_40k.yaml \
    --cache-dir ./cache/vggt_teacher_robotwin_joint_future_slot_v1 \
    --vggt-checkpoint ./pretrained_models/VGGT-1B \
    --num-windows 32
```

保留源视频供审计使用。**Joint 缓存须与训练数据及教师协议匹配**，变更后需重新生成，不能用端点残差缓存替代。

<a id="training"></a>

## 训练

缓存通过审计后启动训练：

```bash
torchrun --standalone --nproc_per_node=4 train/train.py \
    --deepspeed configs/zero1.json \
    --config configs/robotwin_joint_full_from_motus_40k.yaml \
    --run_name joint_full_seed42 \
    --report_to tensorboard
```

| 训练设置 | 取值 |
| --- | --- |
| 每张 GPU 的微批量大小 | 4 |
| 上述命令使用的 GPU 数量 | 4 |
| 梯度累积 | 16 个微批次 |
| 有效全局批量大小 | 256 |
| 优化器更新次数 | 40,000 |
| 学习率 | `5e-5` |
| 预热更新次数 | 200 |
| 随机种子 | 42 |
| 保存间隔 | 20,000 次更新 |

输出目录：`checkpoints/robotwin_joint_full_from_motus_40k/joint_full_seed42/`。

<a id="ablation-configurations"></a>

## 消融配置

任务列表由 `dataset.task_names` 指定，历史文件名中的 `_10task` 对应六任务配置。

| 实验配置 | 配置文件 | 目标 | 预测跨度 | JEPA 动作输入 | 更新次数 / 任务范围 |
| --- | --- | --- | --- | --- | --- |
| Joint 基线 | [`robotwin_joint_10task_8k.yaml`](configs/robotwin_joint_10task_8k.yaml) | Joint | 1, 2, 4, 8 | 完整 | 8k / 六项 |
| 仅短跨度 | [`robotwin_short_only_10task_8k.yaml`](configs/robotwin_short_only_10task_8k.yaml) | Joint | 1 | 完整 | 8k / 六项 |
| 仅长跨度 | [`robotwin_long_only_10task_8k.yaml`](configs/robotwin_long_only_10task_8k.yaml) | Joint | 8 | 完整 | 8k / 六项 |
| 端点残差 | [`robotwin_residual_10task_8k.yaml`](configs/robotwin_residual_10task_8k.yaml) | 残差 | 1, 2, 4, 8 | 完整 | 8k / 六项 |
| No-Action 原始配置 | [`robotwin_no_action.yaml`](configs/robotwin_no_action.yaml) | 残差 | 1, 2, 4, 8 | 置零 | 40k / 全部 |
| No-Action 对齐后的 Joint 配置 | [`robotwin_joint_no_action_6task_8k.yaml`](configs/robotwin_joint_no_action_6task_8k.yaml) | Joint | 1, 2, 4, 8 | 置零 | 8k / 六项 |

<a id="robotwin-evaluation"></a>

## RoboTwin 评估

安装 [RoboTwin](https://github.com/RoboTwin-Platform/RoboTwin) 及策略依赖。部署代码位于 [`inference/robotwin/Motus`](inference/robotwin/Motus)，为兼容评估器保留内部名称 **`Motus`**。

将策略复制到未使用的目标位置：

```bash
export ROBOTWIN_ROOT=/absolute/path/to/RoboTwin
test -d "$ROBOTWIN_ROOT/policy" &&
test ! -e "$ROBOTWIN_ROOT/policy/Motus" &&
cp -a inference/robotwin/Motus "$ROBOTWIN_ROOT/policy/Motus"
```

若 `policy/Motus` 已存在，先备份或合并。更新**复制后的** [`paths_config.yml`](inference/robotwin/Motus/paths_config.yml)：

```yaml
robotwin_root: /absolute/path/to/RoboTwin
conda_env: /absolute/path/to/your/compatible/conda/environment
checkpoint_path: /absolute/path/to/ACG-WAM/pretrained_models/ACG-WAM
wan_path: /absolute/path/to/ACG-WAM/pretrained_models/Wan2.2-TI2V-5B
vlm_path: /absolute/path/to/ACG-WAM/pretrained_models/Qwen3-VL-2B-Instruct
task_config: demo_randomized
seed: 42
```

将 `checkpoint_path` 指向包含 `mp_rank_00_model_states.pt` 的目录，在复制后的 [`eval.sh`](inference/robotwin/Motus/eval.sh) 中设置 `TASK_NAME` 和 `GPU_ID`，然后运行：

```bash
bash "$ROBOTWIN_ROOT/policy/Motus/eval.sh"
```

<a id="citation"></a>

## 引用

```bibtex
@unpublished{liu2026acgwam,
  title  = {ACG-WAM: World-Action Modeling via Action-Conditioned Geometric Latent Prediction},
  author = {Liu, Jiangtao and Xiang, Zishang and He, Yage and Cui, Lingguo and Zhang, Baihai and Chai, Runqi and Chai, Senchun},
  year   = {2026},
  note   = {Manuscript},
  url    = {https://RoboOpus.github.io/ACG-WAM/}
}
```

## 致谢与许可证

感谢 [Motus](https://github.com/thu-ml/Motus)、[WAN](https://github.com/Wan-Video/Wan2.2)、[Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)、[VGGT](https://github.com/facebookresearch/vggt) 和 [RoboTwin](https://github.com/RoboTwin-Platform/RoboTwin) 的作者公开相关资源。

本仓库代码采用 [Apache-2.0 许可证](LICENSE)发布。上游权重和数据集仍受各自条款约束。