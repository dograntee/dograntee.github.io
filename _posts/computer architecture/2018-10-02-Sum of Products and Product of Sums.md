---
layout: post
title: "Sum of Products and Product of Sums"
date: 2018-10-02 17:46:00
image: '/assets/img/computer_archi\Sum of Products and Product of Sums.PNG'
description: It is simple explain about SOP and POS.
category: 'computer architecture'
tags:
- computer architecture
- Sum of Products
- Products of Sum
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: It is simple explain about SOP and POS.
---

### 0x01 Sum of Products
 Sum of Products 표현은 Boolead algerbra 표현 방식에서 서로 다른 곱셈 input 들의 합으로 표현합니다(e.g. F = A'B'C + A'B + AB'C). 여기서 곱셉은 일반적으로 수학의 곱셈이 아닌 논리 AND 연산을 의미하며, 덧셈은 논리 OR 연산을 의미합니다.

#### Minterm

 Sum of Products를 잘 이해하기 위해서는 Minterm이라는 개념을 먼저 짚고 넘어가야 합니다. Mindterm 이란 input에 따른 각 AND Combination 을 의미합니다.

#### Example
~~~c
F(w,x,y,z) = ∑(0,1,2,3,7,8,10)
d(w,x,y,z) = ∑(5,6,11,15) 
~~~
위와 같은 boolean 식을 예로 들면, 아래와 같은 table로 표현할 수 있습니다. 

|  <center>wxyz</center> |  <center>00</center> |  <center>01</center> |  <center>11</center> |  <center>10</center> |
|:--------|:--------:|:--------:|:--------:|:--------:|
|**00** | <center>1 </center> | <center>1 </center> | <center>1 </center> | <center>1 </center> |
|**01** | <center>0 </center> | <center>x </center> | <center>1 </center> | <center>x </center> |
|**11** | <center>0 </center> | <center>0 </center> | <center>x </center> | <center>0 </center> |
|**10** | <center>1 </center> | <center>0 </center> | <center>x </center> | <center>1 </center> |

 Minterm은 w'x'y'z', w'x'y'z, w'x'yz, w'x'yz', ... 총 16개(4개의 input에 따른 Combination)이며, 이들 중 참값의 합인 Sum of Products로 표현하면 아래와 같습니다.

~~~
 F =  w'x'y'z' + w'x'y'z + w'x'yz + w'x'yz' + w'xyz + wx'y'z' + wx'yz'
 ~~~

이를 간소화시켜 나타내면, 아래와 같습니다.

~~~
F = x'z'(w'y' + w'y + wy' + wy) + w'z(x'y' + x'y + xy' + xy)
  = x'z' + w'z
~~~

### 0x02 Product of Sums
  Product of Sums 표현은 Boolean algebra 표현 방식에서 서로 다른 합 input 들의 곱셈으로 표현합니다(e.g. F = (B+C)(A'+B'+C)(A'+B'+C')). 여기서 곱셈과 덧셈은 각각 논리연산 AND와 OR을 의미합니다.

#### Maxterm
 Maxterm이란, input값들에 대한 OR Combination을 의미합니다.

#### Example
~~~c
F(w,x,y,z) = ∑(0,1,2,3,7,8,10)
d(w,x,y,z) = ∑(5,6,11,15) 
~~~
위와 같은 boolean 식을 예로 들면, 아래와 같은 table로 표현할 수 있습니다. 

|  <center>wxyz</center> |  <center>00</center> |  <center>01</center> |  <center>11</center> |  <center>10</center> |
|:--------|:--------:|:--------:|:--------:|:--------:|
|**00** | <center>1 </center> | <center>1 </center> | <center>1 </center> | <center>1 </center> |
|**01** | <center>0 </center> | <center>x </center> | <center>1 </center> | <center>x </center> |
|**11** | <center>0 </center> | <center>0 </center> | <center>x </center> | <center>0 </center> |
|**10** | <center>1 </center> | <center>0 </center> | <center>x </center> | <center>1 </center> |

이 중에 Maxterm 16개 중 false값을 가지는 or combination((w'xy'z'), (wxy'z'), (wxy'z), (wx'y'z),(wx'yz'))을 추출합니다. 이를 카르노맵을 이용하여 축약하면 아래와 같습니다.
~~~
F' = xz' + wz
~~~
이를 다시 F 로 바꾸기 위해 드모르간의 법칙을 사용하면 Product of Sums 은 아래와 같습니다.
~~~
(F')' = (xz' + wz)'
F     = (xz')'(wz)'
      = (x'+z)(w'+z')
~~~


### Reference
 -https://www.electricaltechnology.org/2018/05/sum-of-product-sop-product-of-sum-pos.html