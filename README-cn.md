当然可以，下面是你提供的 README.md 的中文翻译版本，便于你复现实验时参考：

---

## 通过贝叶斯主动水印防御模型提取攻击（ICML 2024） ##

## 环境依赖
- PyTorch 1.12.1

---

## 无防御情况下的模型提取攻击

进入 `DFME` 目录：

```bash
cd DFME
```

运行以下命令进行模型提取：

```bash
python3 train.py --model resnet34_8x --dataset cifar10 --ckpt 'path/your/standardCNNmodel' --device 0 --grad_m 1 --query_budget 20 --log_dir path/to/your/log/dir --lr_G 1e-4 --student_model resnet18_8x --loss l1
```

---

## 使用提出的防御方法进行模型提取

### 第一步：下载预训练模型

请从以下 Google Drive 链接下载预训练模型：[点击此处下载](https://drive.google.com/file/d/1z0IsEm0F5PXnesBqlcckW6feq4kB3HBA/view?usp=sharing)

### 第二步：通过微调预训练模型进行主动水印注入

进入 `teacher-train` 目录：

```bash
cd teacher-train
```

#### CIFAR10 数据集

```bash
python teacher-finetune-AW.py --dataset cifar10 --model resnet34_8x --scale 1.0 --alpha 0.00001 --ckpt 'models/CIFAR10.pth'
```

#### CIFAR100 数据集

```bash
python teacher-finetune-AW.py --dataset cifar100 --model resnet34_8x --scale 0.5 --alpha 0.00001 --ckpt 'models/CIFAR100.pth'
```

> 注意：上述命令中的 `'models/CIFAR10.pth'` 和 `'models/CIFAR100.pth'` 是第一步中下载的模型文件。

### 第三步：进行防御实验

进入 `DFME` 目录：

```bash
cd DFME
```

#### CIFAR10 数据集

```bash
python3 train_cifar10_AW.py --model resnet34_8x --dataset cifar10 --ckpt 'path/to/your/finetunedmodel' --device 0 --grad_m 1 --query_budget 20 --log_dir path/to/your/log/dir --lr_G 1e-4 --student_model resnet18_8x --loss l1
```

#### CIFAR100 数据集

```bash
python3 train_cifar100_AW.py --model resnet34_8x --dataset cifar100 --ckpt 'path/to/your/finetunedmodel' --device 0 --grad_m 1 --query_budget 200 --log_dir path/to/your/log/dir --lr_G 1e-4 --student_model resnet18_8x --loss l1
```

> 注意：上述命令中的 `'path/to/your/finetunedmodel'` 是第二步中微调后得到的模型路径。

---

## 引用

如果你觉得我们的论文或代码对你有帮助，请引用我们的工作：

```bibtex
@inproceedings{wang2024a,
  title={Defense against Model Extraction Attack by Bayesian Active Watermarking},
  author={Zhenyi Wang and Yihan Wu and Heng Huang},
  booktitle={International Conference on Machine Learning},
  year={2024}
}
```

---

## 有问题？

如有任何问题，请联系 [Zhenyi Wang](mailto:wangzhenyineu@gmail.com)

---

如果你需要我帮你整理成中文文档或者进一步解释每个步骤的含义，也可以继续告诉我。