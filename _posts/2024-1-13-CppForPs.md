---
title: Ps를 위한 C++정리
date: 2024-1-13 16:16:00 +0900
categories: [Ps]
tags: [cpp, c++, ps]
math: true
img_path: /assets/img/post6/
---

이 포스트는 Ps에 쓰기 위해 c++ 공부한 것들을 정리한다. 

## String
```cpp
#include <iostream>
#include <string>

using namespace std;
int main ()
{
    string str;
    getline(cin, str);
    //이렇게 하면 한 줄의 string을 입력을 받을 수 있다.
}
```

## Vector
vector는 가변 배열 같은 느낌으로 보면 될 듯 하다.
```cpp
#include <iostream>
#include <vector>

using namespace std;
int main ()
{
    vector<long> V(n, 0); //길이 n짜리 vector, 다 0으로 초기화
    for (int i = 0; i < V.size(); i++)
    {
        V[i] = i; //이렇게 index로 접근도 가능하고
    }
    for (auto& elem:V)
    {
        elem = i; //이렇게 reference로 접근도 가능하고
    }
    for (auto it = V.begin(); it != V.end(); ++it)
    {
        *it *= 2; //이렇게 iterator로 접근도 가능하다.
    }

}
```

## fast input
```cpp
    using namespace std;
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    //이렇게 하면 c언어의 scanf와의 연결을 끊어서 입력을 더욱 빠르게 받을 수 있다. 
```

## array (길이가 정해진 배열)
```cpp
    int alphaArray[26] = {0, }; //0으로 초기화
    int graph[100][100] = {0, }; //2차원 배열도 가능
```

```cpp
    #include <array>
    array<int, 3> betaArray{}; //길이 3짜리 int 배열 0으로 초기화해서 생성
    array<array<int, 1000>, 1000> betaArray{}; // 1000*1000 int 이차원 배열 
```

## vector (길이가 정해지지 않은 배열)
```cpp
    #include <vector>
    using namespace std;
    vector<int> gammaArray(n, 0); //길이 n짜리 int 배열 0으로 초기화해서 생성

    vector<vector<int>> gammaArray(n);
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            int temp;
            cin >> temp;
            gammaArray[i].push_back(temp);
        }
    }
    //n*n 이차원 배열 입력 받기

    vector<int> gammaArray = {3, 5, 4, 1, 2};
    vector<int> deltaArray(gammaArray.begin(), gammaArray.begin()+3);
    //deltaArray = {3, 5, 4};
    //vector 배열 일부 복제하기

    //3*3행렬 배열 만들기
    vector<vector<vector<int>>> gammaArray(1, vector<vector<int>>(3, vector<int>(3)));
    //0만 차있는 3*3으로 초기화
    vector<vector<int>> baseMat = { {0, 1, 0}, {1, 1, 1}, {1, 2, 3} };
    gammaArray.push_back(baseMat); // 뒤에 3*3 행렬 추가
```

## queue 생성 (fifo)
```cpp
    #include <queue>
    using namespace std;

    queue<int> q;
    q.push(10);
    q.push(20);
    q.pop();
    cout << q.front();  //20
```

## priority queue, 역방향 priority queue

{% raw %}
```cpp
#include <queue>
using namespace std;
int main(){
  priority_queue<int> pq; //정방향 우선순위 큐
  priority_queue<int, vector<int>, greater<int>> pq2; //역방향 우선순위 큐

  //그리고 우선순위 큐도 중복가능하다.
  pq.push(1);
  pq.push(2);
  pq.push(3);
  pq.push(3);
  pq.push(3);
  while(!pq.empty()){
    cout << pq.top() << " ";
    pq.pop();
  }
  //3 3 3 2 1

}

```
{% endraw %}

## map
{% raw %}
```cpp
map<int, int> m;
  m[n]++; //map m에서 key = n인 원소의 value++ or 만약 key = n인 원소 없으면 생성 후. 0 + 1 = 1을 value로 입력
  m[n] = 1; //map m에서 key = n인 원소의 value = 1 or 만약 없으면 생성후 1을 value로 입력
  m.count(a); //a있으면 1, 없으면 0 반환
```
{% endraw %}

## bfs
{% raw %}
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;
class Graph{
  public:
    int n;
    vector<vector<int>> graph;
    vector<int> distance;
  //: n(k) 이렇게 하면 초기화 리스트라고
  //처음부터 n을 k로 초기화 하는거다.
  //{}안에 넣으면 default 생성자로 초기화 하고
  //그 다음에 k로 초기화 되기 때문에 overhead발생
  Graph(): n(0){}
  Graph(int n): n(n){
    graph.resize(n);
    distance.resize(n, 0);
  }

  //n*n으로 graph만들지 않고
  //linked list 처럼 만듦
  void addEdge(int a, int b){
    graph[a].push_back(b);
    graph[b].push_back(a);
  }

  void sortList(){
    for(int i = 0; i < n; i++){
      sort(graph[i].begin(), graph[i].end());
    }
  }

  void bfs(){
    vector<int> visited(n, 0);
    //dfs와 달리 bfs는 먼저 방문한 노드를 보니까 큐가 필요
    queue<int> q;
    q.push(0);
    visited[0] = 1;
    while(!q.empty()){
      int cur = q.front();
      q.pop();
      for(int next: graph[cur]){
        if(visited[next] == 0){
          visited[next] = 1;
          q.push(next);
          //next의 부모node가 cur이다
          distance[next] = distance[cur] + 1;
        }
      }
      //visited가 2가 되는 순간 cur과 연결된 node는 다 queue에 들어간 것
      visited[cur] = 2;
      cout << cur << " ";
    }
    

    
  }

};
int main(){
  ios::sync_with_stdio(false);
  cin.tie(0); cout.tie(0);

  Graph g(9);
  g.addEdge(0, 1);
  g.addEdge(0, 2);
  g.addEdge(1, 3);
  g.addEdge(1, 5);
  g.addEdge(3, 4);
  g.addEdge(4, 5);
  g.addEdge(2, 6);
  g.addEdge(2, 8);
  g.addEdge(6, 7);
  g.addEdge(6, 8);
  g.sortList();
  g.bfs();
  //0 1 2 3 5 6 8 4 7
  
  return 0;
}
```
{% endraw %}

