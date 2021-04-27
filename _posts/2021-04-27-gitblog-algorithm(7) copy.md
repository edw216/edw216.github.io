---
title:  "[Algorithm]프로그래머스 - 하샤드수"
excerpt: 프로그래머스 레벨 1 문제풀이

categories:
  - Algorithm
tags:
  - [Java, Algorithm, Programmers]

toc: true
toc_sticky: true
 
date: 2021-04-27
last_modified_at: 2021-04-27
---
## 하샤드수
#### 문제설명
---
양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다. 예를 들어 18의 자릿수 합은 1+8=9이고, 18은 9로 나누어 떨어지므로 18은 하샤드 수입니다. 자연수 x를 입력받아 x가 하샤드 수인지 아닌지 검사하는 함수, solution을 완성해주세요.

**제한사항**
- `x` 는 1 이상, 10000 이하인 정수입니다.


**입출력 예**<br>

|arr|return|
|:--|------|
|10 |true  |
|12 |true  |
|11 |false |
|13 |false |



#### 풀이 코드



---
```java
class Solution {
    public boolean solution(int x) {
        int copyX = x;
        int n=0;
        if(copyX > 10) {

            while (copyX > 10) {
                n += copyX % 10;
                copyX /= 10;
            }
            n += copyX;

            return x % n == 0;
        }
        else return true;

    }
}

```

