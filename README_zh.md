**Read this in [English](README.md).**

# 简介

REINVENT是用于从头设计，脚手架跳跃，R组替换，接头设计，分子优化和其他小分子设计任务的分子设计工具。从本质上讲，重塑使用增强学习（RL）算法来生成具有用户定义的属性配置文件定义为多组分分数的优化分子。转移学习（TL）可用于创建更接近一组输入分子的模型。

代码：https://github.com/MolecularAI/REINVENT4

论文： [REINVENT4: Modern AI-Driven Generative Molecule Design](https://chemrxiv.org/engage/chemrxiv/article-details/65463cafc573f893f1cae33a)
# 安装指南

1. 克隆此 Git 仓库：

```shell
git clone https://github.com/MolecularAI/REINVENT4.git
```

2. 安装兼容版本的 Python，例如使用[Conda](https://conda.io/projects/conda/en/latest/index.html)（Docker、pyenv 或系统包管理器也可以）。
 
```shell
conda create --name reinvent4 python=3.10
conda activate reinvent4
```

3. 将目录更改为克隆的仓库并从锁定文件安装依赖项：

```shell
pip install -r requirements-linux-64.lock
```

<details>
<summary>
4. 可选：如果您想在 Linux 上使用AMD GPU
</summary>
  
则需要在安装第 3 点中的依赖项 _后_手动安装[ROCm PyTorch 版本，例如](https://pytorch.org/get-started/locally/)

```shell
pip install torch==1.13.1+rocm5.2 torchvision==0.14.1+rocm5.2 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/rocm5.2
```

</details>

5. 安装该工具。依赖项已在上一步中安装，无需再次安装（标志“--no-deps”）。如果您想以可编辑模式安装（对代码的更改会自动生效），请在`·`前添加 -e 。

```shell
pip install --no-deps . 
```

6. 测试该工具。安装程序已将脚本添加`reinvent`到您的路径中。

```shell
reinvent --help
```
