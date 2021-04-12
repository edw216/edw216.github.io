---
title:  "[Algorithm]프로그래머스 - 두 개 뽑아서 더하기 "
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
## 두 개 뽑아서 더하기
#### 문제설명
---
정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

**제한사항**
- numbers의 길이는 2이상 100 이하입니다.
  - numbers의 모든 수는 0 이상 100 이하입니다.
  

**입출력 예**<br>

|numbers    |result       |
|:---------:|:-----------:|
|[2,1,3,4,1]|[2,3,4,5,6,7]|
|[5,0,2,7]  |[2,5,7,9,12] |


#### 풀이
---
numbers에 중복된 숫자가 존재하고 result에는 중복된 숫자가 존재하지 않는 것이 보이니 중복값을 저장하지 않는 Set 컬렉션을 사용하여 풀면된다.<br>
오름차순으로 담아 return을 해야하므로 Set중에서도 HashSet, LinkedHashSet이 아닌 TreeSet을 사용한다. TreeSet은 이진 트리로 중복값을 저장하지 않는 것뿐만아니라 기본으로 정렬도 제공한다.<br>


```java
class Solution {
    
	public int[] solution(int[] numbers) {
        int[] answer = {};
        Set<Integer> s = new TreeSet<>();
    
        for(int i=0; i<numbers.length;i++) {
        	for(int j=i+1; j<numbers.length;j++) {
                //중복 제거가 되는 동시에 오름차순으로 정렬되어 들어간다.
        		s.add(numbers[i]+numbers[j]);
        	}
        }
        
        answer = new int[s.size()];
        
        Iterator<Integer> iter = s.iterator();
        int count = 0;
        while(iter.hasNext()) {
        	answer[count] = iter.next();
        	count++;
        }

        return answer;
    }
	
}

```


