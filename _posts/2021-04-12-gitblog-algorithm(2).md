---
title:  "[Algorithm]프로그래머스 - 완주하지 못한 선수 "
excerpt: 프로그래머스 레벨 1 문제풀이

categories:
  - Algorithm
tags:
  - [Java, Algorithm, Programmers]

toc: true
toc_sticky: true
 
date: 2021-04-12
last_modified_at: 2021-04-12
---

## 완주하지 못한 선수
#### 문제설명
---
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.<br>

**제한사항**
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

**입출력 예**<br>

|participant                                  |completion                            |return   |
|:--------------------------------------------|:-------------------------------------|:--------|
|["leo","kiki","eden"]                        |["eden","kiki"]                       |"leo"    |
|["marina","josipa","nikola","vinko","filipa"]|["josipa","filipa","marina","nikola"] |"vinko"  |
|["mislav","stanko","mislav","ana"]           |["stanko","ana","mislav"]             |"mislav" | 


#### 문제풀이
---
이 문제는 중복제거라는 생각이 들어서 set을 사용하려는 생각이 들수도 있다. 
이 문제는 참가자들과 동명이인을 체크해서 완주하는 사람들을 빼는 문제이다. 
Set이 아닌 Map을 사용해야한다.<br>
Map은 중복된 값이 들어오면 기존에 있던 값은 제거가 되고 이후에 들어온값으로 대치되는 특징이 있다.<br>
키 값을 participant 참가자의 이름으로 두고 value 값을 초기값은 0 그이후에 중복된 값이 들어오면 +1 을 해서 문제를 풀어준다.<br>
map의 getOrDefault(key,0) 메소드는 해당 map에 key값이 존재하지않으면 0으로 초기화를 해주고 key값이 존재하면 key에 해당하는 value값을 반환한다.

```java
class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        Map<String,Integer> map = new HashMap<>();

        for(int i=0; i< participant.length;i++){
            //중복 key값이 존재하면 value값에 +1해서 put
            map.put(participant[i],map.getOrDefault(participant[i],0)+1);
        }

        for(int i=0;i<completion.length;i++){
            //완주자에 대한 key값이 존재하면 -1해서 put
            map.put(completion[i],map.get(completion[i])-1);
        }

        for(String key : map.keySet()){
            //완주한 사람들은 value값이 0이 되어있고 완주하지 못한사람은 1이 되어있다.
            if(map.get(key) != 0){
                answer = key;
            }
        }

        return answer;
    }

}
```
