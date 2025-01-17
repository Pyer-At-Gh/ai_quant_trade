# 1. 简介
- 论文: [Qlib : An AI-oriented Quantitative Investment Platform](https://arxiv.org/pdf/2009.11189.pdf)   
- Github:  [qlib](https://github.com/microsoft/qlib)
- [**在线文档**](https://qlib.readthedocs.io/en/latest/index.html)     

## 1.1 核心功能
- 微软开发的AI量化投资平台，当前唯一且最完善的开源平台。
- 不仅提供了高速存储(快于MySQL, MongoDb等)，还提供了详细的全流程一站式服务，从数据下架、模型训练到回测评估。
- 几乎包含了量化投资全流程：alpha因子搜索(不用人工设计因子)、风控模型、最大化投资组合和交易执行。
- 包含机器学习(boosting类算法)、深度学习(LSTM->Transfomre)、强化学习、元学习和动态学习等前沿算法。
- 集成了MLFLOW(生命周期管理)和Optuna(自动超参数搜索)

## 1.2 缺点
- 每个样例中requirements.txt里的torch版本都不同，且比较老

# 2. 安装
## 2.1 系统要求
- 支持windows和linux平台
- 要求python3.7或3.8 (3.9部分功能不支持)

## 2.2 安装
- 如果windows，安装Microsoft C++ Build Tools (大约需要2.5G空间)    
  https://visualstudio.microsoft.com/visual-cpp-build-tools/      


- 快速安装
``` sh
pip install pyqlib
```

- 源码安装
``` sh
git clone https://github.com/microsoft/qlib.git && cd qlib
pip install .
```

# 3. 数据准备
通过如下脚本，可以自动从雅虎财经获取数据，可以获取中国和美国的股市输入。如下，仅列出获取中国股市。
由于雅虎财经数据质量不是特别高，如果有条件，可以使用自由数据源。
### 下载国内数据集
**!!!注意: 数据会被下载到C:\Users\pc\.qlib\qlib_data\cn_data\，而非qlib所在目录**  

### 3.1 下载国内简单数据集 (123M)
进入scripts目录

```bash
python get_data.py qlib_data --name qlib_data_simple --target_dir ~/.qlib/qlib_data/cn_data --region cn
```

### 3.2 数据获取详细操作

```bash
# daily data
python get_data.py qlib_data --target_dir ~/.qlib/qlib_data/cn_data --region cn

# 1min  data (Optional for running non-high-frequency strategies)
python get_data.py qlib_data --target_dir ~/.qlib/qlib_data/cn_data_1min --region cn --interval 1min
```

# 4. 跑一个简单的示例
example下的每个实例里如果存在requirements.txt，需要手动安装。
### 4.1 lightGBM
需要安装
```bash
pip install xgboost
pip install catboost==0.24.3 
```

启动训练和回测
```bash
    cd examples  # Avoid running program under the directory contains `qlib`
    qrun benchmarks/LightGBM/workflow_config_lightgbm_Alpha158.yaml
  ```
  If users want to use `qrun` under debug mode, please use the following command:
  ```bash
  python -m pdb qlib/workflow/cli.py examples/benchmarks/LightGBM/workflow_config_lightgbm_Alpha158.yaml
  ```
  The result of `qrun` is as follows, please refer to [Intraday Trading](https://qlib.readthedocs.io/en/latest/component/backtest.html) for more details about the result. 

  ```bash

