# 作业一：基于Yolov8的松材线病虫害目标识别研究

`Yolov8` `松材线虫病` `目标识别`

---

## 1.数据采集和预处理
&emsp;&emsp;研究区位于中国浙江省衢州市龙游县境内，地理坐标为东经119.17°，北纬29.02°。该地区常年处于亚热带季风气候，四季分p明，光照时间充足，降水充沛，气温宜居。年平均气温区间在16.3℃至17.3℃。从每年10月到次年2月，该地区的降水量相对较高，大约在370到392毫米之间。研究区域包括龙游县内横山镇、石佛镇、扎西镇、小汉海镇，总共四个镇，研究样地总面积约8平方公里。根据中国二类森林资源调查结果显示，主要植被覆盖类型为马尾松林、阔叶常绿林和针叶林与阔叶林混交林，其中马尾松占比较多，占树木比例的70%~100%。值得注意的是，扎西镇样地中变色松树的数量最多，密度最高。研究区域及采样样地如图2所示。1号和2号采样地块分别位于横山镇天池村和杨龙水库。3号地块位于石佛镇凤唐村。4号和5号分别位于横山镇西海寨村和白河桥村。6号位于扎西镇泽穗村，7号位于小南海镇桂光岩村。<br>
&emsp;&emsp;在数据收集过程中，本研究的采集时间在病虫害监控的最佳时间窗口。大多数感染PWD的松树表现出典型的红褐色症状。相比之下，在森林中占比少数的阔叶树尚未变色，因此不会与变色的松树混淆。为了确保标识的准确性和测试的可信度，我们在标识过程中邀请现场调查专家在标注过程中给予我们指导。我们使用了一个叫做`labelImg`的开源工具对样品进行标注工作。对于图像中的每棵受感染的树，我们绘制了一个矩形边界框。每个图像的标记结果保存在一个XML文件中，该文件包含每个边界框的坐标和其中的目标类。然后请现场调查专家对标签结果再次审查。数据集的构建过程如`图1`所示我们将数据集分为三组:训练集、验证集和测试集，如`表1`所示。

> 图1：数据采集处理过程
> ![image](D:\Blog\作业\image.png)

> 表1：数据集划分

| Dataset        | Sampling plots | Dead infected pine trees |
| :--------------: | :--------------: | :------------------------: |
| Training set   | 1,2,3,6        | 3488                     |
| Validation set | 7              | 743                      |
| Test set       | 4, 5           | 755                      |
| Total          | 1,2,3,4,5,6,7  | 4986                     |
## 2.模型预训练
所使用的实验软件和硬件配置如**`表2`**所示，在对数据集进行训练之前，对YOLOv8s网络的参数进行初始化，实验模型超参数如表3所示。原始的YOLOv8和改进的YOLOv8模型都是基于预训练模型(YOLOv8s.pt)进行训练的。在训练过程中，将每个模型保存在其最佳收敛点。使用平均精度（mAP）指标评估模型在对抗性样本数据集上的鲁棒性。IoU阈值设置为`0.5`。

> 表2：实验环境设置

| Experimental Environment | Details                       |
|:------------------------:|:-----------------------------:|
| Programming language     | Python 3.8                    |
| Operating system         | Windows 11                    |
| Deep learning framework  | Pytorch 1.11.0                |
| CPU                      | 22 vCPU AMD EPYC 7T83 64-Core |
| GPU                      | NVIDIA GeForce RTX 4090       |



## 3.模型参数设置
> 表3：训练参数

| Training Parameters                   | Details     |
|:---------------------------------------:|:-------------:|
| Epochs                                | 150         |
| Image size(pixel)                     | 320×320     |
| batch size                            | 32          |
| Initial learning rate                 | 0.01        |
| Final OneCycleLR learning rate        | 0.1         |
| Optimization algorithm and parameters | Adam（0.937） |
| Weight decay                          | 0.0005      |
## 4.模型训练结果
> loss曲线

![results](D:\Blog\作业\results.png)

---

> PR曲线

![PR_curve](D:\Blog\作业\PR_curve.png)

---

> Precision

![R_curve](D:\Blog\作业\R_curve.png)

---

> Recall曲线

![R_curve](D:\Blog\作业\R_curve.png)

---

> 训练图像

![val_batch0_labels](D:\Blog\作业\val_batch0_labels.jpg)
![val_batch2_labels](D:\Blog\作业\val_batch2_labels.jpg)

## 5.模型评估和推理
> 模型推理结果

![val_batch2_labels-1](D:\Blog\作业\val_batch2_labels-1.jpg)
![val_batch0_labels-1](D:\Blog\作业\val_batch0_labels-1.jpg)
## 源代码使用过程文档
>源码下载地址：[https://github.com/liqing-410/yolov8-copy.git](https://github.com/liqing-410/yolov8-copy.git)

### 1.环境配置 

>默认电脑已经配置`conda`深度学习环境，因此不再赘述。
>代码下载解压后建议使用`pychram`打开，将`interpreter`设置为配置好的conda环境。

![img](069d22bfe0854131ad85f7672de0a789.png)

> 在pycharm的terminal中激活虚拟环境 
>
> 终端命令行中输入
>
> ```
> `conda activate 环境名称`
> ```

![img](d274a1372de44c35ae612d8d076f2550.png)

> 安装requirements.txt中的相关包
>
> ```
> pip install -r requirements.txt
> ```

![img](d313cccc9ba246389cf7b87928f93103.png)

> pip安装其他包
>
> ```
> pip install ultralytics
> pip install yolo
> ```
>
> ![img](19813992fcce4016b357a9a6c5d48be8.png)

> 预训练权重下载
>
> > 源代码中已包含v8系列的预训练权重文件.pt文件

### 2.数据集准备

>打开文件目录后新建一个Dataset文件并将文件
>
>- images：下面的子文件夹为图像，存放所有的训练图片；
>- labels：下面的子文件夹为标签，存放所有的标注标签。

![image-20240416145830235](image-20240416145830235.png)

![image-20240416150243801](image-20240416150243801.png)![image-20240416150302655](image-20240416150302655.png)

> 在`ultralytics/cfg/datasets/coco8.yaml`文件中更改自己的数据集文件的路径名称
>
> 将Classes文件中name：设置为分类数量，例如我的数据labels中设置的0标注为染色木，1标注为死树

![image-20240416150407777](image-20240416150407777.png)

### 3.训练自己的数据集

参数都设置好之后，可以直接输入命令，即可开始训练：

```
yolo cfg=ultralytics/yolo/cfg/default.yaml
yolo task=detect mode=train model=models/v8/yolov8.yaml data=data/coco8.yaml  batch=32 epochs=150 workers=16
```

开始运行，等待结果。

![image-20240416151043391](image-20240416151043391.png)

> 训练结果查看：  训练结束后训练结果都保存在`detect/runs`这个文件夹下，可以看到有所有的指标曲线的可视化；还有模型训练出来的权重，`best.pt`为训练的最好的一组权重**，后面使用它来进行模型推理验证。

###  4.使用Val.py文件进行推理

输入下面的命令进行模型的验证，这里的models为训练的最好的那一组权重

> model中改为训练好的权重的地址
>
> val参数设置为`True`
>
> split设置为 `test`模式，注意此处的`coco8.yaml`文档中的`test`目录位置为你要推理的文件目录
>
> batch设置为`1`表示只推理一张图像
>
> 结果保存在`val\`文件夹中

![image-20240416151933374](image-20240416151933374.png)