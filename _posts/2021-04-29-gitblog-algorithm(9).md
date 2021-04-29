---
title:  "[Algorithm]프로그래머스 - 문자열 내 p와 y의 개수"
excerpt: 프로그래머스 레벨 1 문제풀이

categories:
  - Algorithm
tags:
  - [Java, Algorithm, Programmers]

toc: true
toc_sticky: true
 
date: 2021-04-29
last_modified_at: 2021-04-29
---
## 문자열 내 p와 y의 개수
#### 문제설명
---

대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요.<br> 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.


**제한사항**
- 문자열 s의 길이 : 50 이하의 자연수
- 문자열 s는 알파벳으로만 이루어져 있습니다.


**입출력 예**<br>

|s        |answer|
|:--------|:-----|
|"pPoooyY"|true  |
|"Pyy"    |false |



#### 풀이 코드

String.toLowerCase()메소드를 통해서 문자열을 소문자로 변환을 시키고 하나하나 문자를 꺼내면서 풀면 간단하게 풀 수 있습니다.


---
```java
class Solution {
    boolean solution(String s) {
        int pCount = 0;
        int yCount = 0;

        s = s.toLowerCase();

        for(char ch : s.toCharArray()){
            if(ch == 'p') pCount++;
            if(ch == 'y') yCount++;
        }

        return pCount == yCount ? true : false;
    }
}

```

