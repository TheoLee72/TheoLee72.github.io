---
title: 그래프 최단거리 알고리즘 총정리
date: 2024-7-2 20:02:00 +0900
categories: [Ps]
tags: [cpp, c++, ps, graph, shortest path]
math: true
---



# 최단거리 알고리즘

## one source shortest path

### bfs
가중치 없는 그래프에서 넓이 우선 탐색은 최단거리를 만들어 낸다.  
최단거리가 k인 vertex를 모두 탐색한 뒤 k+1탐색을 시작한다.  

### dijkstra
다익스트라 알고리즘  
지금부터는 가중치 그래프다.  
- nonnegative
- directed
- greedy
vertex다 min-queue에 넣어두고 v.d최소 인 것 계속 뽑으면서  
S에 넣고 연결된 edge모두 relaxation시킨다.  

빠른 증명:
S에 u추가하기 직전 상황이다.
s(source)에서 u까지 최단거리 p가 존재한다고 하자.  
s==u이면 그냥 길이 0이고 최단거리 증명 끝나니까  
s!=u이라하고 그럼 s가 포함된 집합 S가 존다한다.  
집합S를 p가 통과할건데 통과 직전 vertex를 x, 통과 직후 vertex를 y라 하자.
s~>x->y~>u  
y==u이라고 처음부터 해버리면 x relaxation하면서 u.d는 당연히 최단거리가 된다.  
이제 u가 S에 추가되면서 u.d가 최단거리가 되지 <u>않는</u> 최초의 vertex라 하자.  
y.d는 x relaxation통해서 이미 최단거리가 보장된다.  
그렇기에 u.d>=s(u)>=y.d(양의 간선이라서). (s는 실제 최단거리)
그런데 y말고 u가 추가되는 상황이니까 u.d<=y.d
따라서 u.d==s(u)==y.d가 되면서 y가 최단거리가 보장된다.

### bellman ford
벨만포드 알고리즘  
- negative edge
- directed
- brute force
V-1번 모든 edge에 대해 relaxation을 한다.  
그 V-1번 안에 모든 vertex가 relaxation되야하고  
만약 되지 않는다면 negative cycle이 있는 것이다.


## all source shortest path

### floyd warshall
플로이드 워셜 알고리즘  
- negative edge
- directed
- brute force
n*n matrix에 n번 loop를 돌면서 path사이에 몇번째 vertex가 들어가는지 따져보며 relaxation한다.  
d^k[i][j] = min(d^k-1[i][j], d^k-1[i][k]+d^k-1[k][j])  
d^k[i][j] = 0~k번째 vertex를 중간에 지나면서 i->j로 가는 shortest path. 
원래는 3차원으로 해야하지만 2차원으로 구현해도 충분하다.  
이유는 플로이드 게시글 참고. 

### Johnson
존슨 알고리즘  
- negative edge
- directed
- greedy
- dijkstra + bellman ford
bellman ford로 음의 간선들을 nonnegative 간선으로 reweighting시킨다  
그리고 각 vertex에서 dijkstra돌려서 최단거리 구한다.  
새로운 vertex s를 만들고 나머지 vertex와 0의 간선으로 연결한다. 
bellman ford를 s에서 돌려서 각 vertex에 v.d를 만든다.(음의cycle detect가능)  
w^(u,v) = w(u, v) + h(u) - h(v)  
그리고 각 vertex에서 dijkstra돌려서 최단거리 구하고 다시 w(u, v)로 원상복구해서 거리 구한다.  



## time complexity
dijkstra < bellman ford  
floyd warshall < Johnson




