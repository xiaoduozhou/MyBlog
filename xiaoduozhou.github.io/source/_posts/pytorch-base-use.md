---
title: pytorch基本语法
date: 2019-05-04 15:17:30
tags: [pytorch]
categories:
- pytorch基础
---
###pytorch基础操作：

+  初始化0矩阵
```python
x = torch.zeros(5, 3, dtype=torch.float)
x = torch.tensor([5.5, 3]) #构建指定元素的矩阵
```
{% asset_img 1.jpg %}

+  加法
```python
x = torch.randn(2,3)
y = torch.randn(2,3)
z = torch.add(x, y)
```
{% asset_img 2.jpg %}


+  任何要原地改动tensor的操作，需要加 _. 比如: x.copy_(y), x.t_(), x.add_(y)will change x.
```python
x = torch.randn(2,3)
print("x",x)
y = torch.randn(2,3)
print("y",y)
x.copy_(y)
print("x",x)
```
{% asset_img 3.jpg %}

+  torch.view(v1,v2,...)来resize或者reshape tensor，例如
```python
x = torch.randn(2,3,2)
print("x",x.shape,x.view(-1,3).shape)
```
{% asset_img 4.jpg %}

+  tensor转numpy,若tensor只有一个元素可以使用.item()取出。
```python
x = torch.randn(2,3,2)
y = x.numpy()
z = torch.tensor(1)
print("x",type(x))
print("y",type(y))
print("z",z,z.item())
```
{% asset_img 5.jpg %}

###pytorch拼接

+  cat沿着数据某一维度拼接
```python
x = torch.randn(2,3)
y = torch.randn(2,3)
z = torch.cat((x,y),1)
print("x",x,x.shape)
print("y",y,y.shape)
print("z",z,z.shape)
```
{% asset_img 21.jpg %}

+  stack 增加新的维度进行堆叠
```python
x = torch.randn(2,3)
y = torch.randn(2,3)
z = torch.stack((x,y),1)
print("x",x,x.shape)
print("y",y,y.shape)
print("z",z,z.shape)
```
{% asset_img 22.jpg %}

+  squeeze 和 unsqueeze, squeeze(dim_n)压缩，即去掉元素数量为1的dim_n维度。同理unsqueeze(dim_n)，增加dim_n维度，元素数量为1。
```python
x = torch.randn(2,1,3)
print("x",x.squeeze().shape)
print("x",x.unsqueeze(0).shape)
```
{% asset_img 23.jpg %}

+  把某一维度的元素反过来(例如第二维)
```python
x = torch.randn(2,3,2)
print("x",x)
index = [3-i-1 for i in range(3)]
print(index)
print("x_change",x[:,index,:])
```
{% asset_img 24.jpg %}

+ topk
```python
torch.topk(input, k, dim=None, largest=True, + + sorted=True, out=None) -> (Tensor, LongTensor)
```
  + input (Tensor) – 输入张量
  + k (int) – “top-k”中的k
  + dim (int, optional) – 排序的维
  + largest (bool, optional) 布尔值，控制返回最大或最小值sorted (bool, optional) 布尔值，控制返回值是否排序
  + out (tuple, optional) – 可选输出张量 (Tensor, LongTensor) output buffer

```python
output = torch.tensor([[-5.4783, 0.2298 , 0.11],
                           [-4.2573, -0.4794 ,0.05],
                           [-0.1070, -5.1511 , 0.01],
                           [-0.1785, -4.3339,-0.02]])

value,index = output.topk(2,dim=1)
print(value)
print(index)
```
{% asset_img 25.jpg %}

+ 扩大张量 expand
返回张量的一个新视图，可以将张量的 **单个维度** 扩大为更大的尺寸。
张量也可以扩大为更高维，新增加的维度将附在前面。 扩大张量不需要分配新内存，仅仅是新建一个张量的视图。任意一个一维张量在不分配新内存情况下都可以扩展为任意的维度。
传入-1则意味着维度扩大不涉及这个维度。
```
a = torch.rand((2,3,1))
b = a.expand(2,3,4)
c = a.expand(-1,-1,4)
print("a",a)
print("b",b)
print("c",c)
```
{% asset_img 26.PNG %}

+ 沿着某个维度重复 repeat 
几个数字分别表示原来维度上重复的数字，如果多了就相当于多加一个维度
```
A = torch.randn([2,2,3])
print(A.shape,A.repeat(3,2,2,3).shape)
```
{% asset_img 27.PNG %}

