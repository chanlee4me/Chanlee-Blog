---
title: "macOS如何管理python环境"
description: "介绍了 conda 的基本概念，以及macOS如何利用conda管理虚拟环境"
pubDate: "Jul 08 2025"
image: "/image/category/tools/conda.jpg" 
categories:
  - tools
tags:
  - Makrdown
badge: Pin
---
# 什么是 conda

Conda是一个辅助进行包管理和环境管理的工具。conda 既具有 pip的包管理能力，同时具备虚拟环境的管理能力

（它是是Ananconda默认的Python包和环境管理工具，所以安装了Ananconda完整版，就默认安装了Conda）

# macOS 安装conda

[官网 miniconda 安装教程](https://www.anaconda.com/docs/getting-started/miniconda/main)



# 使用 conda 创建并使用虚拟环境

- 创建虚拟环境：`conda create -n myenv python=3.10`

  - `myenv`：是虚拟环境的名字

  - `python=3.10` 指定 Python 版本，可以按需要修改

- 激活虚拟环境：`conda activate myenv`
  - `myenv`：是虚拟环境的名字
- 退出虚拟环境：`conda deactivate`



# 安装依赖

- 使用 conda 安装依赖：`conda install numpy pandas matplotlib`
- 使用 pip 安装依赖：`pip install some-package`

> 建议先用 `conda` 安装，`conda` 解决依赖更稳。如果 `conda` 没有，再用 `pip`。



# 导入和导出依赖

- 导出`.yml`格式的环境依赖：`conda env export > environment.yml`

- 用`.yml`文件创建环境：`conda env create -f environment.yml`

- 导出`.txt`格式的环境依赖：`pip freeze > requirements.txt`（在激活的 conda 虚拟环境中运行）

  > `pip freeze` 不会记录 conda 安装的依赖，如果你的环境里混用了 `conda` 和 `pip` 安装的包，推荐还是导出成 `environment.yml`，这样更完整。

- 从`requirements.txt`安装依赖：`pip install -r requirements.txt`

  

# 删除环境

- 删除环境：`conda remove -n myenv --all`



# 在 pycharm中为项目选择虚拟环境

![image-20250606102338086](/Users/cli/Library/Application Support/typora-user-images/image-20250606102338086.png)

![image-20250606102304463](/Users/cli/Library/Application Support/typora-user-images/image-20250606102304463.png)