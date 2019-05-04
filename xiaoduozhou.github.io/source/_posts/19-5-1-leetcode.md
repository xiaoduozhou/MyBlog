---
title: 分治
date: 2019-05-01 18:34:08
tags: [leetcode,分治]
categories:
- [leetcode]

---

###题目1. 求 Pow(x,n)

[no.50](https://leetcode.com/problems/powx-n/)

<!-- <img width=400 src="50.png" > -->
{% asset_img 50.png This is an example image %}


####题目简介：
&emsp;&emsp;求pow(x,n) ,时间复杂度要小。
####题解：
&emsp;&emsp;首先 n可能有正负，所以分两种情况处理 return 1/ Pow(x,-n,map)和 return Pow(x,n,map); 在计算pow的时候 根据n是奇数或者是偶数来进行分治。用map记录已经计算过的值，减少递归的数量级。

####代码：
```
​
class Solution {
    public double myPow(double x, int n) {
        Map<Integer,Double> map = new HashMap<>();
        if(n <0){
           return 1/ Pow(x,-n,map);
        }else{
            return Pow(x,n,map);
        }
    }
    
    double Pow(double x, int n,Map<Integer,Double> map){
        if (n ==0)return 1;
        if(n == 1)return x;
        if(n == 2)return x*x;
        if(map.get(n) != null){  //计算过了就直接返回
             return map.get(n);
        }else{
            Double t=0.0;
            if(n %2 == 0){
                 t = Pow(x,n/2,map)*Pow(x,n/2,map);  //n为偶数
            }else{
                 t =  Pow(x,n/2,map)*Pow(x,n/2,map)*x; //n为奇数
            }
            map.put(n,t);  //没计算过就存起来
            return t;
        } 
       
    }
}

​
```