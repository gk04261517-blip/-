# VAD 项目结构重组方案

## 项目概述

本项目是一个**语音活动检测（Voice Activity Detection, VAD）系统**，包含：
- 多种噪声模型（白噪声、粉红噪声、冲击噪声）
- 两种 VAD 算法（双门限法、特征融合法）
- 性能评估工具
- 交互式标注界面
- SNR 扫频分析工具

---

## 推荐的目录结构

```
vad_project/
│
├── README.md                          # 项目说明文档
├── LICENSE                            # 开源协议
├── .gitignore                         # Git 忽略文件
├── startup.m                          # 项目启动脚本
├── requirements.txt                   # 依赖说明
│
├── config/                            # 配置层
│   └── vad_project_config.m          # 全局配置文件
│
├── src/                               # 源代码（核心模块）
│   │
│   ├── core/                          # 核心算法
│   │   ├── vad_TwoThr.m              # 双门限 VAD 算法
│   │   ├── vad_specEn_cepstral.m     # 特征融合 VAD 算法
│   │   └── getAdaptiveVadParams.m    # 自适应参数获取
│   │
│   ├── preprocess/                    # 预处理模块
│   │   ├── enframe.m                  # 信号分帧
│   │   ├── preprocess_csv_signal.m    # CSV 信号预处理
│   │   ├── build_ground_truth.m       # 地面真值构建
│   │   └── vad_frontend_suffix.m      # 前端处理标记
│   │
│   ├── noise/                         # 噪声生成
│   │   ├── add_noise_by_snr.m         # 按 SNR 添加噪声
│   │   ├── pink_noise.m               # 粉红噪声生成
│   │   └── impulse_noise_helper.m     # 冲击噪声辅助
│   │
│   └── evaluation/                    # 评估指标
│       ├── evaluate_vad_metrics.m     # VAD 性能指标计算
│       ├── apply_recognition_report_scale.m  # 指标缩放
│       └── segments_to_flag.m         # 段转标志位
│
├── analysis/                          # 分析脚本层
│   │
│   ├── single_signal/                 # 单文件分析
│   │   ├── analyze_single_signal.m
│   │   └── analyze_single_signal_snr_sweep.m
│   │
│   └── dataset/                       # 数据集分析
│       └── compile_vowel_consonant_snr_plots.m
│
├── labeling/                          # 标注工具
│   └── create_endpoint_labels_manual.m
│
├── utils/                             # 工具函数
│   ├── ternary.m                      # 三元操作符
│   └── helper_functions.m             # 其他辅助函数
│
├── tests/                             # 测试代码
│   ├── test_noise_generation.m
│   ├── test_vad_algorithms.m
│   └── test_metrics_calculation.m
│
├── examples/                          # 使用示例
│   ├── example_basic_vad.m
│   ├── example_snr_sweep.m
│   └── example_manual_labeling.m
│
├── docs/                              # 文档
│   ├── API.md                         # API 文档
│   ├── ALGORITHM.md                   # 算法说明
│   ├── USAGE.md                       # 使用指南
│   └── TROUBLESHOOTING.md             # 故障排查
│
├── data/                              # 数据目录（通常不版本控制）
│   ├── raw/                           # 原始数据
│   └── processed/                     # 处理后的数据
│
└── outputs/                           # 输出结果（不版本控制）
    ├── single_analysis/               # 单文件分析结果
    ├── snr_sweep_dataset/             # SNR 扫频结果
    └── figures/                       # 生成的图表
```

---

## 文件迁移对照表

| 原文件名 | 新路径 | 类别 | 说明 |
|---------|--------|------|------|
| `add_noise_by_snr.m` | `src/noise/add_noise_by_snr.m` | 核心 | 噪声注入（三种类型） |
| `pink_noise.m` | `src/noise/pink_noise.m` | 核心 | 粉红噪声生成 |
| `enframe.m` | `src/preprocess/enframe.m` | 核心 | 信号分帧 |
| `build_ground_truth.m` | `src/preprocess/build_ground_truth.m` | 核心 | 地面真值构建 |
| `apply_recognition_report_scale.m` | `src/evaluation/apply_recognition_report_scale.m` | 核心 | 指标缩放 |
| `analyze_single_signal.m` | `analysis/single_signal/analyze_single_signal.m` | 应用 | 单文件分析 |
| `analyze_single_signal_snr_sweep.m` | `analysis/single_signal/analyze_single_signal_snr_sweep.m` | 应用 | SNR 扫频 |
| `compile_vowel_consonant_snr_plots.m` | `analysis/dataset/compile_vowel_consonant_snr_plots.m` | 应用 | 元音/辅音对比 |
| `create_endpoint_labels_manual.m` | `labeling/create_endpoint_labels_manual.m` | 应用 | 人工标注工具 |

---

## 整理步骤

### Step 1: 备份原项目
```bash
cp -r vad_project vad_project_backup
```

### Step 2: 创建新目录结构
脚本会自动创建

### Step 3: 移动源文件
脚本会自动移动

### Step 4: 运行启动脚本
```matlab
startup
```

---

## 优势

✅ **模块化** - 功能清晰分离
✅ **可维护性** - 易于查找和修改代码
✅ **可扩展性** - 新功能易于集成
✅ **协作友好** - 团队成员快速上手
✅ **标准化** - 遵循 MATLAB 最佳实践
✅ **版本控制** - 与 Git 集成友好
