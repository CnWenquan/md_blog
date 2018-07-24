---
title: java中的位移操作
url: 214.html
id: 214
categories:
  - Java
date: 2016-08-25 15:49:49
tags:
---

java中有三种移位运算符：  

    << : 左移运算符，num<<1,相当于num乘以2

    >\> : 右移运算符，num>>1,相当于num除以2

    >>\> : 无符号右移，忽略符号位，空位都以0补齐

  

上了例子加注释：

public class Shifting {  
    public static void main(String\[\] args)  
    {  
      
        System.out.println(1<<1);//1左移一位从0001->00010，相当于乘以2  结果2  
        System.out.println(4>>2);//相当于除以4  结果1  
        System.out.println(1&2);//1->0001 2->0010 逐位比较  返回0000 结果0  
        System.out.println(1^1);//相同的返回0  
        System.out.println(1|2);//1->0001 2->0010 逐位比较  返回0011结果3  
          
    }  
  
}