---
title:  "[Algorithm]프로그래머스 - 카카오 프렌즈 컬러링북"
excerpt: 프로그래머스 레벨 2 문제풀이

categories:
  - Algorithm
tags:
  - [Java, Algorithm, Programmers]

toc: true
toc_sticky: true
 
date: 2021-05-04
last_modified_at: 2021-05-04
---
## 카카오 프렌즈 컬러링북
#### 문제설명
---

출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)<br>

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.

![](https://github.com/edw216/edw216.github.io/blob/master/assets/images/algorithm10/apeach.png?raw=true)

위의 그림은 총 12개 영역으로 이루어져 있으며, 가장 넓은 영역은 어피치의 얼굴면으로 넓이는 120이다.

**입력 형식**

입력은 그림의 크기를 나타내는 `m`과 `n`, 그리고 그림을 나타내는 `m × n` 크기의 2차원 배열 `picture`로 주어진다. 제한조건은 아래와 같다.

- `1 <= m, n <= 100`
- `picture`의 원소는 `0` 이상 `2^31 - 1` 이하의 임의의 값이다.
- `picture`의 원소 중 값이 `0`인 경우는 색칠하지 않는 영역을 뜻한다.


**출력 형식**

리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

**입출력 예**<br>

|m  |n  |picture                                                      |answer|
|:--|:--|:------------------------------------------------------------|:-----|
|6  |4  |[[1,1,1,0],[1,2,2,0],[1,0,0,1],[0,0,0,1],[0,0,0,3],[0,0,0,3]]|[4,5] |
|6  |4  |[[1,1,1,0],[1,1,1,0],[0,0,0,1],[0,0,0,1],[0,0,0,1],[0,0,0,1]]|[2,9] |



#### 풀이 코드


---
```java
class Node{
    int x;
    int y;
    public Node(int x, int y){
        this.x = x;
        this.y = y;
    }
}
class Solution {
    static boolean[][] visit;
    static int[] dx = {-1,0,1,0}; // left, up, right, down
    static int[] dy = {0,-1,0,1};
    static int M,N;

    public static int bfs(int startX, int startY,int[][] picture,int color){
        int areaCnt = 1;
        Queue<Node> q = new LinkedList<>();

        q.offer(new Node(startX,startY));
        visit[startX][startY] = true;

        while(!q.isEmpty()){
            Node node = q.poll();

            int nodeX = node.x;
            int nodeY = node.y;

            for(int i = 0; i < 4; i++){
                int x = nodeX + dx[i];
                int y = nodeY + dy[i];

                if(x >= 0 && x < M && y >=0 && y < N){
                    if(!visit[x][y]){
                        if(picture[x][y] == color){
                            q.offer(new Node(x,y));
                            visit[x][y] = true;
                            areaCnt++;
                        }
                    }
                }
            }
        }
        return areaCnt;

    }
    public int[] solution(int m, int n, int[][] picture) {
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;
        M = m;
        N = n;
        visit = new boolean[M][N];

        for(int i = 0; i < picture.length; i++){
            for(int j = 0; j < picture[i].length; j++){
                if(picture[i][j] == 0) visit[i][j] = true;
            }
        }

        for(int x = 0; x < picture.length; x++){
            for(int y = 0; y < picture[x].length; y++){
                if(!visit[x][y]){
                    maxSizeOfOneArea = Math.max(maxSizeOfOneArea, bfs(x,y,picture,picture[x][y]));
                    numberOfArea++;
                }
            }
        }

        int[] answer = new int[2];
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;
        return answer;
    }
}

```

