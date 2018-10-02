---
layout: post
title: "Sum of Products and Product of Sums"
date: 2018-10-02 17:46:00
image: '/assets/img/computer_archi\Sum of Products and Product of Sums.PNG'
description: pwnable.kr - fd problem solving
category: 'computer architecture'
tags:
- computer architecture
- Sum of Products
- Products of Sum
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: It is simple explain about SOP and POS.
---

### Sum of Products
 Sum of Products 표현은 Boolead algerbra 표현 방식에서 서로 다른 곱셈 input 들의 합으로 표현합니다(e.g. F = A'B'C + A'B + AB'C). 여기서 곱셉은 일반적으로 수학의 곱셈이 아닌 논리 AND 연산을 의미하며, 덧셈은 논리 OR 연산을 의미합니다.

 #### Minterm

 Sum of Products를 잘 이해하기 위해서는 Minterm이라는 개념을 먼저 짚고 넘어가야 합니다. Mindterm 이란 input에 따른 AND Combination 에서 True 값을 가지는 Combination을 의미합니다.

#### Example
<center>$$ F(w,x,y,z) = ∑(0,1,2,3,7,8,10) $$</center>
<center>$$ d(w,x,y,z) = ∑(5,6,11,15) $$</center>
위와 같은 boolean 식을 예로 들면, 아래와 같은 table로 표현할 수 있습니다. 
| wx/yz | 00 | 01 | 11 | 10 |
|-------|----|----|----|----|
|<center> 0 </center> |<center> 1 </center> |<center> 1 </center> |<center> 1 </center> |<center> 1 </center> |
|<center> 01 </center> |<center> 0 </center>|<center> X </center>|<center> 1 </center> |<center> X </center> |
|<center> 11 </center> |<center> 0 </center>|<center> 0 </center>|<center> X </center>|<center> 0 </center>|
|<center> 10 </center> |<center> 1 </center>|<center> 0 </center>|<center> X </center>|<center> 1 </center>|

 Minterm은 w'x'y'z', w'x'y'z, w'x'yz, w'x'yz', w'xyz, wx'y'z', wx'yz' 이며, 이를 Sum of Products로 표현하면 아래와 같습니다.

<center>$$ F =  w'x'y'z' + w'x'y'z + w'x'yz + w'x'yz' + w'xyz + wx'y'z' + wx'yz' $$</center>

이를 간소화시켜 나타내면, 아래와 같습니다.

<center>$$ F =  x'z'(w'y' + w'y + wy' + wy) + w'z(x'y' + x'y + xy' + xy) $$</br>
 = x'z' + w'z</br>

 ### Product of Sums