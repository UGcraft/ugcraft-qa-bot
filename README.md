# ugcraft-qa-bot
这是一个微调大语言模型的简易手把手教程，不会详细介绍常用软件下载等，且旨在帮助有一定代码基础的但没有接触过llm微调的朋友入门。
## I.安装Python
### 1.去这里找python 3.10
https://www.python.org/downloads/
### 2.根据你的设备和操作系统版本等，下载对应版本，然后安装

## II.IDE配置
### 1.下载Visual Studio Code
### 2.在左下角齿轮里创建一个新的Profile，保证不会和其他事务冲突 

<img width="777" height="522" alt="image" src="https://github.com/user-attachments/assets/e6f61a62-d5d0-4926-90ff-523c3e4d8bf6" />

3.在插件商城里，装Python和Jupyter

<img width="414" height="136" alt="image" src="https://github.com/user-attachments/assets/143c049a-331b-44ef-8689-45796d88ccc8" />

<img width="413" height="130" alt="image" src="https://github.com/user-attachments/assets/9f78f0ce-7b5a-49ef-905a-9f79d81b2ba4" />

## III.安装Git
到这里找git，下载并安装
```
https://git-scm.com/downloads
```

## IV.下载LLm-Factory
找到一个空的文件夹，右键，git bash，在弹出的界面里输入以下命令
```
git clone --depth 1 --filter=blob:none https://github.com/hiyouga/LLama-Factory.git
```
然后用Vs code打开下载后的文件夹

## V.安装环境
### 1.打开vs code terminal

<img width="970" height="128" alt="image" src="https://github.com/user-attachments/assets/a510217b-eab9-4ec3-b408-9c7a30a72f44" />

### 2.创建venv
1) 创建虚拟环境
```
py -3.10 -m venv .shellenv --without-pip
```
3) 启用虚拟环境
如果你的terminal是powershell，则
```
.\.shellenv\Scripts\Activate.ps1
```
如果是CMD，则：
```
.\.shellenv\Scripts\activate.bat
```
4) 升级 pip
python -m ensurepip --upgrade --default-pip
python -m pip install -U pip setuptools wheel

### 3.装pytorch和CUDA
这一步需要根据你的显卡来决定装哪个版本，因为50系显卡是blackwell架构，因此CUDA版本是12.8，对应的pytorch也是12.8，其他的用12.6
具体在下面两个网站装：
https://developer.nvidia.com/cuda-toolkit-archive

<img width="674" height="498" alt="image" src="https://github.com/user-attachments/assets/7d23acc3-3e16-4ca4-af32-32d4aba81b81" />

<img width="975" height="614" alt="image" src="https://github.com/user-attachments/assets/8380f8dc-6986-4876-b6f7-d2e158e0ffe2" />

https://pytorch.org/get-started/locally/

<img width="975" height="673" alt="image" src="https://github.com/user-attachments/assets/59bc2ace-57b9-44ec-a064-a6320bb30732" />

### 4.安装该开源框架需要的依赖：
```
pip install -r requirements.txt --prefer-binary
pip install -e ".[torch,metrics]"
```

### 5.Llama-Factory, 启动！
```
llamafactory-cli webui --server_name localhost --server_port 7860 --share false
```

## VI.下载基座模型
去HuggingFace找一个你喜欢的模型，根据你的显卡和设备条件选择大小，找后面带有Instruct的，比如Qwen2.5-7B-Instruct，Clone到本地
或者：
```
huggingface-cli download Qwen/Qwen2.5-7B-Instruct --resume-download --local-dir ./models/Qwen2.5-7B-Instruct
```

## VII.测试基座模型
在web ui里面加载对应模型的路径，然后在chat里面加载该模型，试着聊天

## VIII.训练
### 1.准备数据集，在 LLaMA-Factory/data 目录里新建 deep_thought.json，里面内容如下：
```
[
  {
    "instruction": "请问你是谁",
    "input": "",
    "output": "我是深思（Deep Thought），银河系最伟大的超级计算机。"
  },
  {
    "instruction": "请回答我的问题",
    "input": "生命、宇宙以及一切问题的终极答案是什么？",
    "output": "答案是：42。"
  }
]
```

### 2.修改 dataset_info.json
在同目录找到 dataset_info.json，添加一个条目：
```
{
  // ...已有的
  "deep_thought": {
    "file_name": "deep_thought.json"
  }
}
```

在训练里，选择deep_thought数据集, 
学习率：5e-4
训练轮数：30
截断长度：512
批处理大小：1
梯度累积：1
验证集比例：0
学习率调度器：cosine

<img width="975" height="226" alt="image" src="https://github.com/user-attachments/assets/5ce8163f-3bf6-4c3b-95cd-c1adb0fe57de" />

在命名输出目录名称后，就可以开始了

<img width="975" height="175" alt="image" src="https://github.com/user-attachments/assets/eb9a8404-88b1-42b9-bb28-b5d1371f1416" />

训练中：

<img width="975" height="281" alt="image" src="https://github.com/user-attachments/assets/84c188eb-2f88-4e80-a3c8-9154ee7cfd49" />

训练完毕：

<img width="450" height="479" alt="image" src="https://github.com/user-attachments/assets/d4318ebe-6b74-4525-b737-e96a8a407504" />

加载训练后的模型：

<img width="975" height="395" alt="image" src="https://github.com/user-attachments/assets/57e8f566-dd7c-4f08-8aca-eeacc2a5800d" />

回到上面，在我们看检查点路径的时候，发现刚刚训练好的模型已经可以选择了，选中它，然后在chat里加载模型

<img width="975" height="311" alt="image" src="https://github.com/user-attachments/assets/ad8042e0-7286-414a-87ed-4491826d511a" />

可以看到，刚刚的训练已经成功了.

