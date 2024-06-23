# SDH

## 1 要求
pytorch >= 1.0

loguru

`pip install -r requirements.txt`


## 2 数据集
1. [cifar10-gist.mat](https://pan.quark.cn/s/e012da8b6355) 提取码：vLB1
2. [cifar-10_alexnet.t](https://pan.quark.cn/s/6af484846cc9) 提取码：KLxc
3. [nus-wide-tc21_alexnet.t](https://pan.quark.cn/s/bfa2d69865ed) 提取码：8Wdq

## 3 项目目录
```
SDH_PyTorch/
├── checkpoints/                       模型训练过程中的检查点               
├── data/                            
│   ├── dataloader.py                  加载不同数据集
│   ├── cifar-10_alexnet.t             使用 AlexNet 模型处理的 Cifar-10 数据集
│   ├── cifar-10_gist.mat              使用 GIST 处理的 Cifar-10 数据集
│   └── nus-wide-tc21_alexnet.t        使用 AlexNet 模型处理的 NUSWIDE-21 数据集
├── logs/                              项目日志文件     
├── utils/           
│   └── evaluate.py                    评估哈希检索算法性能（mAP, precision, recall）    
├── README.md                          项目简介和使用说明
├── requirements.txt                   项目依赖库
├── sdh.py                             训练SDH模型
└── run.py                             针对不同数据集训练和评估SDH模型
```

## 4 用法
```
python run.py [-h] [--dataset DATASET] [--root ROOT]
              [--code-length CODE_LENGTH] [--max-iter MAX_ITER]
              [--num-train NUM_TRAIN] [--num-query NUM_QUERY] 
              [--topk TOPK] [--gpu GPU] [--seed SEED]
              [--evaluate-interval EVALUATE_INTERVAL] [--sigma SIGMA]

SDH_PyTorch

可选参数:
  -h, --help                              显示帮助信息并退出
  --dataset DATASET                       数据集名称 (cifar10-gist/cifar10/nus-wide-tc21)
  --root ROOT                             数据集路径 
  --code-length CODE_LENGTH               二进制哈希码长度 (默认: 12,16,24,32,48,64,128)                      
  --max-iter MAX_ITER                     迭代次数 (默认: 3)
  --topk TOPK                             计算top k的 map (默认: all)
  --gpu GPU                               是否使用GPU (默认: False)
  --seed SEED                             随机种子 (默认: 3367)
  --evaluate-interval EVALUATE_INTERVAL   评估间隔 (默认: 1)
```

## 5 实验
### 参数设置
cifar-10-gist: GIST features, 1000 query images, 5000 training images, sigma=2e-3, map@ALL.

`python run.py --dataset cifar10-gist --root data/cifar-10_gist.mat`

cifar-10-alexnet: Alexnet features, 1000 query images, 5000 training images, sigma=5e-4, map@ALL.

` python run.py --dataset cifar10 --root data/cifar-10_alexnet.t`

nus-wide-tc21-alexnet: Alexnet features, top 21 classes, 2100 query images, 10500 training images, sigma=5e-4, map@5000.

`python run.py --dataset nus-wide-tc21 --root data/nus-wide-tc21_alexnet.t --topk 5000`


### mAP
 bits | 12 | 16 | 24 | 32 | 48 | 64 | 128
   :-:   |  :-:    |   :-:   |   :-:   |   :-:   |   :-:   |   :-:   |   :-:     
cifar-10-gist@ALL | 0.2651 | 0.3018 | 0.3035 | 0.3224 | 0.3555 | 0.3525 | 0.3689
cifar-10-alexnet@ALL | 0.5189 | 0.5075 | 0.5331 | 0.5448 | 0.5468 | 0.5657 | 0.5772
nus-wide-tc21-alexnet@5000 | 0.7502 | 0.7704 | 0.7752 | 0.7930 | 0.7917 | 0.8034 | 0.8156

### precision
 bits | 12 | 16 | 24 | 32 | 48 | 64 | 128
   :-:   |  :-:    |   :-:   |   :-:   |   :-:   |   :-:   |   :-:   |   :-:     
cifar-10-gist@ALL | 0.1811 | 0.2062 | 0.2023 | 0.2169 | 0.2689 | 0.2616 | 0.2886
cifar-10-alexnet@ALL | 0.2531 | 0.2487 | 0.2646 | 0.2916 | 0.2931 | 0.3029 | 0.3189
nus-wide-tc21-alexnet@5000 | 0.4791 | 0.4998 | 0.4896 | 0.4979 | 0.4963 | 0.5132 | 0.5234

### recall
 bits | 12 | 16 | 24 | 32 | 48 | 64 | 128
   :-:   |  :-:    |   :-:   |   :-:   |   :-:   |   :-:   |   :-:   |   :-:     
cifar-10-gist@ALL | 0.7052 | 0.7048 | 0.7392 | 0.7361 | 0.6783 | 0.7094 | 0.6958
cifar-10-alexnet@ALL | 0.8162 | 0.7951 | 0.7881 | 0.7670 | 0.7692 | 0.7712 | 0.7687
nus-wide-tc21-alexnet@5000 | 0.7696 | 0.7714 | 0.7700 | 0.7681 | 0.7595 | 0.7579 | 0.7539

