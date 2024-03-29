---
title:  "[Algorithm]프로그래머스 - 체육복"
excerpt: 프로그래머스 레벨 1 문제풀이

categories:
  - Algorithm
tags:
  - [Java, Algorithm, Programmers]

toc: true
toc_sticky: true
 
date: 2021-04-14
last_modified_at: 2021-04-14
---
## 체육복
#### 문제설명
---
점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

**제한사항**
- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.
  

**입출력 예**<br>

|n   |lost |reserve|return|
|:---|:----|:------|:-----|
|5   |[2,4]|[1,3,5]|5     |
|5   |[2,4]|[3]    |4     |
|3   |[3]  |[1]    |2     |


#### 풀이
---
이 문제의 핵심 포인트는 체육복을 도난을 당한사람이 체육복을 빌려주는 학생으로 중복으로 존재하는경우를 잘 생각하면 됩니다.<br>
중복이 되지 않는 경우라면 Boolean[] 배열로 true,false 체크를 해주면서 쉽게 풀어갈 수 있지만 중복이 있는 경우로 인해서 true,false만으로는 풀수가 없게 됩니다. 그래서 int[]배열을 사용하여 0,1,2의 경우의수를 두고 풀었습니다.<br>

0 : 자신의 체육복만 가지고 있는 학생 lost배열과 reserve배열에 해당되지 않아서 0으로 초기화<br>
1 : 도난을 당해서 체육복을 빌려야만 수업에 참여할 수 있는 학생<br>
2 : 자신의 체육복을 빌려줄 수 있지만 도난을 당해서 체육복을 빌려줄 수 없고 수업에 참여하는 학생<br>


```java
class Solution {
    
    public int solution(int n, int[] lost, int[] reserve) {
        // 0=자신의 체육복만 , 1=도난당한 학생, 2=도난 당해서 자신의 체육복만
        int[] check = new int[n+2]; 
        int answer = 0;

        for(int l : lost){
            check[l] = 1;
        }
        for(int r : reserve){
             //여벌 있는 학생이 도난을 당하면 자신만 수업 참여 
            if(check[r] == 1){
                check[r] = 2;
            }
        }

        for (int j : reserve) {
            if(check[j] == 2){
                continue;
            }else {
                if (check[j - 1] == 1) {
                    check[j - 1] = 0;
                } else if (check[j + 1] == 1) {
                    check[j + 1] = 0;
                } else continue;
            }
        }

        for(int i=1;i<check.length-1; i++){
            if(check[i] == 0 || check[i] == 2){
                answer++;
            }
        }


        return answer;
    }
}

```


