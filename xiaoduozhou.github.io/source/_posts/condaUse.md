---
title: conda基本使用
date: 2019-05-08 19:17:43
tags: [conda]
categories:
- 开发工具
---
###conda常用命令
1.查看已安装环境
```
conda info -e
或
conda env list
```
{% asset_img 1.PNG %}

2.创建新环境mycondaEnv
```
conda create -n mycondaEnv python=3.5
```

3.激活conda
```
conda activate mycondaEnv
```
4.退出当前环境：
```
conda deactivate
```

5.删除conda
```
conda env remove -n mycondaEnv
```


6.在conda中安装包
```
conda env list   查看环境
conda activate mycondaEnv   激活进入环境
pip install jieba  安装包
conda deactivate  退出环境
```

7.conda复制环境
```
conda create -n 新名字 --clone 已存在的conda名
如：conda create -n BBB --clone AAA
```

8.conda导出以及导入已有环境：
在新机器上会自动执行创建出一个相同的环境
```
conda env export > environment.yaml   导出

conda env create -f environment.yaml  导入
```
9.pip导出包以及导入包
```
pip freeze > requirements.txt  导出

pip install -r requirements.txt   导入
```




