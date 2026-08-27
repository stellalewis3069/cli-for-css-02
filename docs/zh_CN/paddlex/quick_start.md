# 快速开始

>**❗ 说明：**
>* 飞桨低代码开发工具[PaddleX](https://github.com/PaddlePaddle/PaddleX)，依托于PaddleClas的先进技术，支持了图像分类和检索领域的**低代码全流程**开发能力。通过低代码全流程开发，可实现简单且高效的模型使用、组合与定制。
>* PaddleX 致力于实现产线级别的模型训练、推理与部署。模型产线是指一系列预定义好的、针对特定AI任务的开发流程，其中包含能够独立完成某类任务的单模型（单功能模块）组合。本文档提供**图像分类和检索相关产线**的快速使用，单功能模块的快速使用以及更多功能请参考[PaddleClas低代码全流程开发](./overview.md)中相关章节。


### 🛠️ 安装

> ❗安装PaddleX前请先确保您有基础的**Python运行环境**。（注：当前支持Python 3.8 ～ Python 3.10下运行，更多python版本适配中）。
* **安装PaddlePaddle**
```bash
# cpu
python -m pip install paddlepaddle==3.0.0b2 -i https://www.paddlepaddle.org.cn/packages/stable/cpu/

# gpu，该命令仅适用于 CUDA 版本为 11.8 的机器环境
python -m pip install paddlepaddle-gpu==3.0.0b2 -i https://www.paddlepaddle.org.cn/packages/stable/cu118/

# gpu，该命令仅适用于 CUDA 版本为 12.3 的机器环境
python -m pip install paddlepaddle-gpu==3.0.0b2 -i https://www.paddlepaddle.org.cn/packages/stable/cu123/
```
> ❗ 更多飞桨 Wheel 版本请参考[飞桨官网](https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/install/pip/linux-pip.html)。

* **安装PaddleX**

```bash
pip install paddlex==3.0.0b2
```

> ❗ 更多安装方式参考[PaddleX安装教程](https://paddlepaddle.github.io/PaddleX/latest/installation/installation.html)
### 💻 命令行使用

一行命令即可快速体验产线效果，统一的命令行格式为：

```bash
paddlex --pipeline [产线名称] --input [输入图片] --device [运行设备]
```

只需指定三个参数：
* `pipeline`：产线名称
* `input`：待处理的输入图片的本地路径或URL  
* `device`: 使用的GPU序号（例如`gpu:0`表示使用第0块GPU），也可选择使用CPU（`cpu`）


以通用图像分类产线为例：
```bash
paddlex --pipeline image_classification --input https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_image_classification_001.jpg --device gpu:0
```
<details>
  <summary><b>👉 点击查看运行结果 </b></summary>

```
{'input_path': './my_path/general_image_classification_001.jpg', 'class_ids': [296, 170, 356, 258, 248], 'scores': [0.62736, 0.03752, 0.03256, 0.0323, 0.03194], 'label_names': ['ice bear, polar bear, Ursus Maritimus, Thalarctos maritimus', 'Irish wolfhound', 'weasel', 'Samoyed, Samoyede', 'Eskimo dog, husky']}
```
![](https://raw.githubusercontent.com/cuicheng01/PaddleX_doc_images/main/images/pipelines/image_classification/03.png)


</details>

其他产线的命令行使用，只需将`pipeline`参数调整为相应产线的名称。下面列出了每个产线对应的命令：

| 产线名称      | 使用命令                                                                                                                                                                                             |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 通用图像分类    | `paddlex --pipeline image_classification --input https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_image_classification_001.jpg --device gpu:0`                           |
| 通用图像多标签分类 | `paddlex --pipeline multi_label_image_classification --input https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_image_classification_001.jpg --device gpu:0`                                          |



### 📝 Python脚本使用

使用 [测试文件](https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_image_classification_001.jpg)，并将 `predict()` 替换为本地路径，几行代码即可完成产线的快速推理，以通用图像分类产线为例：
```python
from paddlex import create_pipeline

pipeline = create_pipeline(pipeline="image_classification")

output = pipeline.predict("general_image_classification_001.jpg")
for res in output:
    res.print() ## 打印预测的结构化输出
    res.save_to_img("./output/") ## 保存结果可视化图像
    res.save_to_json("./output/") ## 保存预测的结构化输出
```
执行了如下几个步骤：

* `create_pipeline()` 实例化产线对象
* 传入图片并调用产线对象的`predict` 方法进行推理预测
* 对预测结果进行处理

得到的结果与命令行方式相同。


下面列出了其他产线对应的参数名称及详细的使用解释：

| 产线名称           | 对应参数               | 详细说明                                                                                                      |
|--------------------|------------------------|---------------------------------------------------------------------------------------------------------------|
| 通用图像分类       | `image_classification` | [通用图像分类产线 Python 脚本使用说明](https://paddlepaddle.github.io/PaddleX/latest/pipeline_usage/tutorials/cv_pipelines/image_classification.html) |
| 通用图像多标签分类 | `multi_label_image_classification` | [通用图像多标签分类产线 Python 脚本使用说明](https://paddlepaddle.github.io/PaddleX/latest/pipeline_usage/tutorials/cv_pipelines/image_multi_label_classification.html) |


