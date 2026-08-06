### Overview

This repository is meant as a proof of concept for how paraboloid neurons can be used to improve the accuracy of non-transformer CNNs by replacing the output layer with a layer of paraboloid neurons.

|   Model           | Accuracy |
| ----------------- |-------- |
| ```resnet18``` - baseline   | 78.89% |
| ```resnet18-paraboloidout```         | **79.56%** |


# Using paraboloid neurons to train DLA models on CIFAR100 with PyTorch

Paraboloid neuron demonstration of the [GeoND Library](https://geond.tech) for [PyTorch](http://pytorch.org/) on the CIFAR100 dataset. If you are interested in trying out the library on your own datasets, please refer to the ["How to use"](https://geond.tech/geond-docs/) section of the documentation. This repository uses Version 1.1 of the GeoND Library. You can find download instructions here: [https://geond.tech/download/](https://geond.tech/download/). Adapted from [https://github.com/huggingface/pytorch-image-models](https://github.com/huggingface/pytorch-image-models).

## Requirements
- Linux only.
- Python 3.9+, use of a virtual environment recommended.
- Install the rest of the requirements by running:
```
pip install -r requirements.txt
```
- (Optional) Download the pre-trained models by running:
```
wget -i models.txt
```

## Models
- ### resnet18
Our baseline ResNet18 model. After creating the model, we make some changes to accomodate the resolution of CIFAR100 images:
```
model.conv1 = nn.Conv2d(3, 64, kernel_size=3, stride=1, padding=1, bias=False)
model.maxpool = nn.Identity()
```
#### Evaluation
Download the pretrained model and run:
```
python train.py ./data --dataset torch/cifar100 --dataset-download --num-classes 100 --img-size 32 --epochs 300 --resume resnet18.pth.tar --eval true
```
#### Training from scratch
Run:
```
python train.py ./data --dataset torch/cifar100 --dataset-download --num-classes 100 --img-size 32 --opt sgd --momentum 0.9 --weight-decay 5e-4 --sched cosine --epochs 300 --lr 0.1 --batch-size 128 --min-lr 1e-5 --aa rand-m9-mstd0.5-inc1 --mixup 0.2 --cutmix 1.0 --reprob 0.25 --remode pixel --smoothing 0.1
```




- ### resnet18-paraboloidout
A ResNet18 model (modified for CIFAR100 as above) with a layer of paraboloid neurons as the output layer. After the Version 1.1 update which fixed some issues, it is possible to use paraboloid layers as output layers. Note that models with a paraboloid output layer (or a paraboloid layer before a linear output layer) seem to perform better without momentum. The training script will handle this through a command line argument. This model achieves the best accuracy.

In terms of code, first we import the Library:
```
try:
    import geondpt as gpt
except ImportError:
    import geondptfree as gpt
```

Then we replace the existing output layer:
```
model.fc = gpt.ParaboloidOutput(model.fc.in_features, model.fc.out_features, h_factor = 0.01, lr_factor = 1., wd_factor = 1., grad_factor = 1., input_factor = 0.5, output_factor = 0.1, p_factor=0.0001, init = 'spotlight')
```
Note that ```ParaboloidOutput``` is the same as ```Paraboloid```, it just uses a base configuration more appropriate for output layers.

#### Evaluation
Download the pretrained model and run:
```
python train.py ./data --dataset torch/cifar100 --dataset-download --num-classes 100 --img-size 32 --paraboloidout True --eval True --resume resnet18-paraboloidout.pth.tar
```
#### Training from scratch
Run:
```
python train.py ./data --dataset torch/cifar100 --dataset-download --num-classes 100 --img-size 32 --opt sgd --momentum 0.7 --weight-decay 5e-4 --sched cosine --epochs 300 --lr 0.1 --batch-size 128 --min-lr 1e-5 --aa rand-m9-mstd0.5-inc1 --mixup 0.2 --cutmix 1.0 --reprob 0.25 --remode pixel --smoothing 0.1 --paraboloidout true
```















```
python train.py ./data --dataset torch/cifar100 --dataset-download --num-classes 100 --img-size 32 --paraboloidout True --eval True --resume resnet18-paraboloidout.pth.tar
```


|   Model           | Momentum | Accuracy |
| ----------------- |-------- | -------- |
| ```resnet18``` - baseline   | 0.9, nesterov = True | 78.89% |
| ```resnet18-paraboloidout```   |   0.1, nesterov = False   | 78.96% |
| ```resnet18-paraboloidout```   |   0.2, nesterov = False   | 78.82% |
| ```resnet18-paraboloidout```   |   0.4, nesterov = False   | 79.12% |
| ```resnet18-paraboloidout```   |   0.5, nesterov = False   | 79.14% |
| ```resnet18-paraboloidout```   |   0.5, nesterov = True   | 79.32% |
| ```resnet18-paraboloidout```   |   0.6, nesterov = False   | 79.37% |
| ```resnet18-paraboloidout```   |   0.6, nesterov = True   | 79.32% |
| ```resnet18-paraboloidout```   |   0.7, nesterov = False   | **79.56%** |
| ```resnet18-paraboloidout```   |   0.7, nesterov = True   | 79.44% |
| ```resnet18-paraboloidout```   |   0.8, nesterov = False   | 78.87% |
| ```resnet18-paraboloidout```   |   0.9, nesterov = False   | 77.46% |
