# gsplat-ws

基于gsplat库的高斯泼溅(Gaussian Splatting)渲染与训练Python工作空间。

## 环境要求

- Python 3.12+
- CUDA支持（推荐使用GPU以获得最佳性能）
- UV包管理器

## 快速开始

### 安装依赖
```bash
# 同步环境和安装依赖
uv sync
```

### 基础训练
```bash
# 单GPU训练
uv run examples/simple_trainer.py --data.path <数据集路径> --output.dir <输出路径>

# 分布式训练（4个GPU）
CUDA_VISIBLE_DEVICES=0,1,2,3 uv run examples/simple_trainer.py default --steps_scaler 0.25
```

### 模型查看
```bash
# 查看训练后的模型
uv run examples/simple_viewer.py --model <模型检查点路径>
```

## 项目结构

```
examples/
├── simple_trainer.py      # 基础高斯泼溅训练器
├── simple_trainer_2dgs.py # 2D高斯泼溅训练器
├── simple_viewer.py       # 基础查看器
├── gsplat_viewer.py       # 高级查看器
├── simple_viewer_2dgs.py  # 2D高斯泼洒查看器
├── image_fitting.py       # 图像拟合示例
├── datasets/              # 数据集加载工具（支持COLMAP）
└── benchmarks/            # 性能基准测试
```

## 核心功能

- **训练支持**：支持经典和MCMC密度化策略
- **多种查看器**：包括Web界面和本地查看器
- **COLMAP集成**：支持COLMAP数据集的加载和处理
- **分布训练**：支持多GPU分布式训练
- **模型压缩**：支持PNG等压缩格式
- **实时可视化**：基于viser的Web可视化界面

## 配置选项

主要训练参数：
- `--data.path`: 数据集路径
- `--output.dir`: 结果输出目录
- `--batch_size`: 批处理大小
- `--max_steps`: 最大训练步数
- `--sh_degree`: 球谐函数阶数
- `--strategy`: 密度化策略（default/mcmc）

## 数据集支持

支持COLMAP格式数据集，包含：
- 图像文件
- 相机参数
- 点云初始化数据
- 深度信息（可选）

## 性能优化

- CUDA加速渲染
- 稀疏梯度支持
- 抗锯齿光栅化
- 可见性Adam优化器

## 依赖库

- `gsplat`: 核心高斯泼溅库
- `torch`: PyTorch深度学习框架
- `viser`: 3D可视化
- `nerfview`: NeRF风格查看器组件
- `pycolmap`: COLMAP集成
- `imageio[ffmpeg]`: 图像/视频I/O

## 故障排除

如遇CUDA内存不足，可尝试：
- 减少批处理大小
- 启用packed模式：`--packed True`
- 降低初始高斯数量

更多详细参数和用法请参考：`uv run examples/simple_trainer.py --help`