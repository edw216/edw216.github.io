---
title:  "[Algorithm]프로그래머스 - 크레인 인형뽑기 게임"
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
## 크레인 인형뽑기 게임
#### 문제설명
---
게임개발자인 "죠르디"는 크레인 인형뽑기 기계를 모바일 게임으로 만들려고 합니다.
"죠르디"는 게임의 재미를 높이기 위해 화면 구성과 규칙을 다음과 같이 게임 로직에 반영하려고 합니다.

![crane_game1](https://github.com/edw216/edw216.github.io/blob/master/assets/images/algorithm1/crane_game_101.png?raw=true)

게임 화면은 "1 x 1" 크기의 칸들로 이루어진 "N x N" 크기의 정사각 격자이며 위쪽에는 크레인이 있고 오른쪽에는 바구니가 있습니다. (위 그림은 "5 x 5" 크기의 예시입니다). 각 격자 칸에는 다양한 인형이 들어 있으며 인형이 없는 칸은 빈칸입니다. 모든 인형은 "1 x 1" 크기의 격자 한 칸을 차지하며 격자의 가장 아래 칸부터 차곡차곡 쌓여 있습니다. 게임 사용자는 크레인을 좌우로 움직여서 멈춘 위치에서 가장 위에 있는 인형을 집어 올릴 수 있습니다. 집어 올린 인형은 바구니에 쌓이게 되는 데, 이때 바구니의 가장 아래 칸부터 인형이 순서대로 쌓이게 됩니다. 다음 그림은 [1번, 5번, 3번] 위치에서 순서대로 인형을 집어 올려 바구니에 담은 모습입니다.

![crane_game1](https://github.com/edw216/edw216.github.io/blob/master/assets/images/algorithm1/crane_game_102.png?raw=true)

만약 같은 모양의 인형 두 개가 바구니에 연속해서 쌓이게 되면 두 인형은 터뜨려지면서 바구니에서 사라지게 됩니다. 위 상태에서 이어서 [5번] 위치에서 인형을 집어 바구니에 쌓으면 같은 모양 인형 두 개가 없어집니다.

![crane_game1](https://github.com/edw216/edw216.github.io/blob/master/assets/images/algorithm1/crane_game_103.gif?raw=true)

크레인 작동 시 인형이 집어지지 않는 경우는 없으나 만약 인형이 없는 곳에서 크레인을 작동시키는 경우에는 아무런 일도 일어나지 않습니다. 또한 바구니는 모든 인형이 들어갈 수 있을 만큼 충분히 크다고 가정합니다. (그림에서는 화면표시 제약으로 5칸만으로 표현하였음)

게임 화면의 격자의 상태가 담긴 2차원 배열 board와 인형을 집기 위해 크레인을 작동시킨 위치가 담긴 배열 moves가 매개변수로 주어질 때, 크레인을 모두 작동시킨 후 터트려져 사라진 인형의 개수를 return 하도록 solution 함수를 완성해주세요.

**제한사항**
- board 배열은 2차원 배열로 크기는 "5 x 5" 이상 "30 x 30" 이하입니다.
- board의 각 칸에는 0 이상 100 이하인 정수가 담겨있습니다.
    - 0은 빈 칸을 나타냅니다.
    - 1 ~ 100의 각 숫자는 각기 다른 인형의 모양을 의미하며 같은 숫자는 같은 모양의 인형을 나타냅니다.
- moves 배열의 크기는 1 이상 1,000 이하입니다.
- moves 배열 각 원소들의 값은 1 이상이며 board 배열의 가로 크기 이하인 자연수입니다.
  

**입출력 예**<br>

|board                                                        |moves            |result | 
|:------------------------------------------------------------|:----------------|:------|
|[[0,0,0,0,0],[0,0,1,0,3],[0,2,5,0,1],[4,2,4,4,2],[3,5,1,3,1]]|[1,5,3,5,1,2,1,4]|4      |


#### 풀이
---
위의 그림을 보면 자료구조의 스택의 LIFO(Last In Firtst Out)의 구조인 것을 확인할 수가 있다. <br>
다음 들어올 정수와 스택의 마지막에 들어온 정수와 비교하여 삭제를 해주는 동시에 결과값을 늘려주면 된다.<br>
stack.peek() 메소드는 스택의 맨 위에 있는 데이터를 반환하여준다.<br>
스택의 공간이 비어서 생기는 런타임 에러를 방지하기 위해 add(100);을 초기값으로 넣어주고 시작한다.<br>
초기값을 넣지 않고 실행하는 방법은 stack.isEmpty()를 통해서 조건문을 넣어주면 해결 가능하지만 조건문을 추가하는것보다는 초기값을 담아주는게 더 나을 거 같아서 초기값을 넣어줬다.<br>


```java
public class Solution {
	
	public int solution(int[][] board, int[] moves) {
        int answer = 0;
        Stack<Integer> st = new Stack<>();
        st.add(100);

        for(int i=0; i<moves.length; i++) {
        	for(int j=0; j<board.length;j++) {
        		
        		if(board[j][moves[i]-1] != 0) {
        			if(st.peek() == board[j][moves[i]-1]) {
        				st.pop();
	        			board[j][moves[i]-1] = 0;
	        			answer += 2;
	        			break;
        			}else {
	        			st.add(board[j][moves[i]-1]);
	        			board[j][moves[i]-1] = 0;
                        break;
        			}
        
        		}
        		
        		
        	}
        }
        
        return answer;
        
    }
}

```


