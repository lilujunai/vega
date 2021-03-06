# 示例参考

Vega提供了算法和任务的使用指导，也针对开发者，提供了算法开发的相关指导，如扩展搜索空间和搜索算法、构建适用于Vega的数据集等。

## 1. 下载示例

在`release`下载示例，解压后，有如下目录：

| 目录 | 说明 |
| :--: | :-- |
| compression | 压缩算法使用示例，包括 [Quant-EA](../algorithms/quant_ea.md)、 [Prune-EA](../algorithms/prune_ea.md) 两个算法 |
| data augmentation | 数据增广算法使用示例，包括 [PBA](../algorithms/pba.md) 算法 |
| hpo | 超参优化算法使用示例， 包括 [ASHA](../algorithms/hpo.md), [BO](../algorithms/hpo.md), [TPE](../algorithms/hpo.md), [BOHB](../algorithms/hpo.md), [BOSS](../algorithms/hpo.md) 等算法 |
| nas | 网络架构搜索相关示例，包括 [SM-NAS](../algorithms/sm-nas.md), [CARS](../algorithms/cars.md), [SP-NAS](../algorithms/sp-nas.md), [auto-lane](../algorithms/auto_lane.md), [SR-EA](../algorithms/sr-ea.md), [ESR-EA](../algorithms/esr_ea.md), [Adelaide-EA](../algorithms/Segmentation-Adelaide-EA-NAS.md) |
| searchspace | [细粒度搜索空间](../developer/fine_grained_search_space.md)相关示例 |
| fully train | fully train 相关示例，包括训练 torch vision 的 restnet18 模型，训练 CARS 模型等示例 |
| tasks/classification | 综合使用 NAS + HPO + FullyTrain 完成一个图像分类任务的示例 |

## 2. 示例说明

一般一个算法示例包含了是一个配置文件，有一些算法还有一些配套的代码。

进入 examples 目录后，可以执行如下命令运行示例：

```bash
python3 ./run_example.py <algorithm config file>
```

比如要运行CARS算法示例，命令如下：

```bash
python3 ./run_example.py ./nas/cars/cars.yml
```

所有的信息都在配置文件中，配置项可分为公共配置项和算法相关配置项，公共配置项可参考[配置参考](./config_reference.md)，算法配置需要参考各个算法的参考文档。

在运行示例前，需要下载数据集到缺省的数据配置目录中。在运行示例前，需要创建目录`/cache/datasets/`，然后将各个数据集下载到该目录，并解压。各个数据集的缺省目录配置如下：

| Dataset | Default Path | Data Source |
| :--- | :--- | :--: |
| Cifar10 | /cache/datasets/cifar10/ | [下载](https://www.cs.toronto.edu/~kriz/cifar.html) |
| ImageNet | /cache/datasets/ILSVRC/ | [下载](http://image-net.org/download-images) |
| COCO | /cache/datasets/COCO2017 | [下载](http://cocodataset.org/#download) |
| Div2K | /cache/datasets/DIV2K/ | [下载](https://data.vision.ee.ethz.ch/cvl/DIV2K/) |
| Div2kUnpair | /cache/datasets/DIV2K_unknown |  [下载](https://data.vision.ee.ethz.ch/cvl/DIV2K/) |
| Cityscapes | /cache/datasets/cityscapes/ | [下载](https://www.cityscapes-dataset.com/) |
| VOC2012 | /cache/datasets/VOC2012/ | [下载](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/#data) |
| Cifar10TF | /cache/datasets/cifar-10-batches-bin/ | [下载](https://www.cs.toronto.edu/~kriz/cifar-10-binary.tar.gz) |
| ECP       | /cache/datasets/ECP/    | [下载](https://eurocity-dataset.tudelft.nl/eval/downloads/detection)  |

另外，对于以下算法，需要加载预训练模型。在运行示例前，需要创建目录/cache/models/，然后从相应的位置下载对应的模型后，放置到该目录：

| Algorithm | Pre-trained Model | Default Path | Model Source |
| :--: | :-- | :-- | :--: |
| Adelaide-EA | mobilenet_v2-b0353104.pth | /cache/models/mobilenet_v2-b0353104.pth | [下载](http://www.noahlab.com.hk/opensource/vega/models/pretrained/mobilenet_v2-b0353104.pth) |
| Prune-EA | resnet20.pth | /cache/models/resnet20.pth | [下载](http://www.noahlab.com.hk/opensource/vega/models/pretrained/prune/resnet20.pth) |
| SP-NAS | resnet50-19c8e357.pth | /cache/models/resnet50-19c8e357.pth | [下载](http://www.noahlab.com.hk/opensource/vega/models/pretrained/resnet50-19c8e357.pth) |
|        | SPNet_ECP_ImageNetPretrained_0.7978.pth | /cache/models/SPNet_ECP_ImageNetPretrained_0.7978.pth | [下载](http://www.noahlab.com.hk/opensource/vega/models/pretrained/SPNet_ECP_ImageNetPretrained_0.7978.pth) |

要说明的是，示例中的配置项都设置的很小，是为了更快的运行出结果，但过小的配置会造成运行结果是不理想的，所以大家可以参考各个算法的说明文档，根据需要来修改和调整配置，运行出所需的结果。

## 3. 示例输入和输出

### 3.1 模型压缩示例

1. Prune-EA

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | nas | 输入 | 配置文件：compression/prune-ea/prune.yml <br> 预训练模型：/cache/models/resnet20.pth <br> 数据集：/cache/datasets/cifar10 |
    | nas | 输出 | 网络描述文件：tasks/\<task id\>/output/nas/model_desc_\<id\>.json |
    | fully train | 输入 | 配置文件：compression/prune-ea/prune.yml <br> 网络描述文件：tasks/\<task id\>/output/nas/model_desc_\<id\>.json <br> 数据集：/cache/datasets/cifar10 |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

2. Quant-EA

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | nas | 输入 | 配置文件：compression/quant-ea/quant.yml <br> 数据集：/cache/datasets/cifar10 |
    | nas | 输出 | 网络描述文件：tasks/\<task id\>/output/nas/model_desc_\<id\>.json |
    | fully train | 输入 | 配置文件：compression/quant-ea/quant.yml <br> 网络描述文件：tasks/\<task id\>/output/nas/model_desc_\<id\>.json <br> 数据集：/cache/datasets/cifar10 |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

### 3.2 网络架构搜索

1. CARS

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | nas | 输入 | 配置文件：nas/cars/cars.yml <br> 数据集：/cache/datasets/cifar10 |
    | nas | 输出 | 网络描述文件：tasks/\<task id\>/output/nas/model_desc_\<id\>.json |
    | fully train | 输入 | 配置文件：nas/cars/cars.yml <br> 网络描述文件：tasks/\<task id\>/output/nas/model_desc_\<id\>.json <br> 数据集：/cache/datasets/cifar10 |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

2. Adelaide-EA

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | random | 输入 | 配置文件：nas/adelaide_ea/adelaide_ea.yml <br> 数据集：/cache/datasets/cityscapes |
    | random | 输出 | 网络描述文件：tasks/\<task id\>/output/random/model_desc_\<id\>.json |
    | mutate | 输入 | 配置文件：nas/adelaide_ea/adelaide_ea.yml <br> 数据集：/cache/datasets/cityscapes <br> 网络描述文件：tasks/\<task id\>/output/random/model_desc_\<id\>.json  |
    | mutate | 输出 | 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json |
    | fully train | 输入 | 配置文件：nas/adelaide_ea/adelaide_ea.yml <br> 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json <br> 数据集：/cache/datasets/cityscapes |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

3. ESR-EA

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | nas | 输入 | 配置文件：nas/esr_ea/esr_ea.yml <br> 数据集：/cache/datasets/DIV2K |
    | nas | 输出 | 网络描述文件：tasks/\<task id\>/output/nas/selected_arch.npy |
    | fully train | 输入 | 配置文件：nas/esr_ea/esr_ea.yml <br> 网络描述文件：tasks/\<task id\>/output/nas/selected_arch.npy <br> 数据集：/cache/datasets/DIV2K |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

4. SR-EA

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | random | 输入 | 配置文件：nas/sr_ea/sr_ea.yml <br> 数据集：/cache/datasets/DIV2K |
    | random | 输出 | 网络描述文件：tasks/\<task id\>/output/random/model_desc_\<id\>.json |
    | mutate | 输入 | 配置文件：nas/sr_ea/sr_ea.yml <br> 数据集：/cache/datasets/DIV2K <br> 网络描述文件：tasks/\<task id\>/output/random/model_desc_\<id\>.json  |
    | mutate | 输出 | 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json |
    | fully train | 输入 | 配置文件：nas/sr_ea/sr_ea.yml <br> 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json <br> 数据集：/cache/datasets/DIV2K |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

5. SP-NAS

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | nas1 | 输入 | 配置文件：nas/sp_nas/spnas.yml <br> 数据集：/cache/datasets/COCO2017 <br> 预训练模型：/cache/models/resnet50-19c8e357.pth <br> 配置文件： nas/sp_nas/faster_rcnn_r50_fpn_1x.py |
    | nas1 | 输出 | 网络描述文件：tasks/\<task id\>/output/nas1/model_desc_\<id\>.json <br> 模型列表：tasks/\<task id\>/output/nas1/total_list_p.csv |
    | nas2 | 输入 | 配置文件：nas/sp_nas/spnas.yml <br> 数据集：/cache/datasets/COCO2017 <br> 网络描述文件：tasks/\<task id\>/output/nas1/model_desc_\<id\>.json <br> 模型列表：tasks/\<task id\>/output/nas1/total_list_p.csv <br> 配置文件： nas/sp_nas/faster_rcnn_r50_fpn_1x.py |
    | nas2 | 输出 | 网络描述文件：tasks/\<task id\>/output/nas2/model_desc_\<id\>.json <br> 模型列表：tasks/\<task id\>/output/nas2/total_list_s.csv |
    | fully train | 输入 |  配置文件：nas/sp_nas/spnas.yml <br> 数据集：/cache/datasets/COCO2017 <br> 网络描述文件：tasks/\<task id\>/output/nas2/model_desc_\<id\>.json <br> 模型列表：tasks/\<task id\>/output/nas2/total_list_s.csv <br> 配置文件： nas/sp_nas/faster_rcnn_r50_fpn_1x.py |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fullytrain/model_\<id\>.pth |

### 3.3 数据增广

1. PBA

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | pba | 输入 | 配置文件：data_augmentation/pba/pba.yml <br> 数据集：/cache/datasets/cifar10 |
    | pba | 输出 | Transformer列表：tasks/\<task id\>/output/pba/best_hps.json |

2. CycleSR

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | fully train | 输入 | 配置文件：data_augmentation/cyclesr/cyclesr.yml <br> 数据集：/cache/datasets/DIV2K_unknown |
    | fully train | 输出 | 模型：tasks/\<task id\>/output/fully_train/model_0.pth |

### 3.4 超参优化

1. ASHA、BOHB、BOSS

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | hpo1 | 输入 | 配置文件：hpo/asha\|bohb\|boss/hpo/asha\|bohb\|boss.yml <br> 数据集：/cache/datasets/cifar10 |
    | hpo1 | 输出 | 超参描述文件：tasks/\<task id\>/output/hpo1/best_hps.json |

### 3.5 Fully Train

1. EfficientNet

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | fully train | 输入 | 配置文件：fully_train/efficientnet/efficientnet_b0.yml <br> 数据集：/cache/datasets/ILSVRC |
    | fully train | 输出 | 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json <br> 模型：tasks/\<task id\>/output/fully_train/model_\<id\>.pth |

2. FMD

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | fully train | 输入 | 配置文件：fully_train/fmd/fmd.yml <br> 数据集：/cache/datasets/cifar10 |
    | fully train | 输出 | 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json <br> 模型：tasks/\<task id\>/output/fully_train/model_0.pth |

3. FMD

    | 阶段 | 选项 | 内容 |
    | :--: | :--: | :-- |
    | fully train | 输入 | 配置文件 fully_train/trainer/resnet.yml <br> 数据集"/cache/datasets/ILSVRC |
    | fully train | 输出 | 网络描述文件：tasks/\<task id\>/output/mutate/model_desc_\<id\>.json <br> 模型：tasks/\<task id\>/output/fully_train/model_0.pth |
