# chapter5.DFS/BFS

1. 꼭 필요한 자료구조 기초

   탐색 : 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정 ex)DFS,BFS

   자료구조 : 데이터를 표현하고 관리하고 처리하기 위한 구조

   - 스택  파이썬에서 스택을 이용할 때는 별도의 라이브러리를 사용할 필요가 없다!개꿀

   - 큐 from collections import deque    #큐 구현을 위해 deque 라이브러리 사용

     ```python
     from collections import deque
     
     queue=deque()
     
     queue.append(5)
     queue.append(2)
     queue.append(3)
     queue.append(7)
     queue.popleft()
     queue.append(1)
     queue.append(4)
     queue.popleft()
     
     print(queue)	#먼저 들어온 순서대로 출력 #deque([3,7,1,4])
     queue.reverse()
     print(queue)    #나중에 들어온 원소부터 출력 #deque([4,1,7,3])
     ```

   - 재귀함수 개쉬룸 

     장점 : 코드가 간결해진다

     종료조건을 주의해서 주기

2. 탐색 알고리즘 DFS/BFS

   - 인접행렬(Adjacency Matrix) : 2차원 배열로 그래프의 연결 관계를 표현하는 방식, 2차원 리스트로 구현

     ```python
     INF=999999999 #연결되어 있지 않은 노드끼리에서 사용할 무한의 비용
     
     graph=[
         [0,7,5],
         [7,0,INF],
         [5,INF,0]
     ]
     
     print(graph)	#[[0,7,5],[7,0,999999999],[5,999999999,0]]
     ```

   - 인접 리스트(Adjacency List) : 리스트로 그래프의 연결 관계를 표현하는 방식, 연결리스트로 구현(파이썬은 기본 라이브러리에서 제공 갓이썬,,,)

     ```python
     #행이 3개인 2차우너 리스트로 인접 리스트 표현
     graph=[[] for _ in range(3)]
     
     #노드 0에 연결된 노드 정보 저장(노드, 거리)
     graph[0].append((1,7))
     graph[0].append((2,5))
     
     #노드 1에 연결된 노드 정보 저장(노드, 거리)
     graph[1].append((0,7))
     
     #노드 2에 연결된 노드 정보 저장(노드, 거리)
     graph[2].append((0,5))
     
     print(graph)	#[[(1,7),(2,5)],(0,7),[(0,5)]]
     ```

     =>인접 행렬은 모든 관계를 저장하므로 노드 개수가 많을수록 메모리가 불필요하게 낭비

     인접 리스트는 연결된 정보만을 저장하기 때문에 메모리를 효율적으로 사용 하지만 특정 노드끼리 연결되어 있는지에 대한 정보를 얻는 속도는 느림, 연결된 노드를 하나씩 확인해야 하기 때문

     =>특정 노드와 연결된 모든 인접 노드를 순회해야하는 경우 인접 리스트 방식이 메모리 공간 낭비가 적다.

   - DFS(Depth-First Search) : 깊이 우선 탐색, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘

     모든 노드를 방문하고자 할 때 유용, 스택이용

     동작 과정 :

     1)탐색 시작 노드를 스택에 삽입하고 방문처리

     2)스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문처리, 방문합지 않은 인접 노드가 없으면 스택에서 최상단 노드를 pop

     3) 2)의 과정을 더이상 수행할 수 없을때까지 반복

     ```python
     def dfs(graph, v, visited):
         visited[v]=1
         print(v, end=" ")
         for i in graph[v]:
             if visited[i]==0:
                 dfs(graph,i,visited)
     
     graph=[
         [],
         [2,3,8],
         [1,7],
         [1,4,5],
         [3,5],
         [3,4],
         [7],
         [2,6,8],
         [1,7]
         ]
     
     visited=[0]*9
     dfs(graph,1,visited)
     #1 2 7 6 8 3 4 5
     ```

   - BFS(Breadth First Search) : 너비 우선 탐색, 가까운 노드부터 탐색하는 알고리즘

     두 노드 사이의 최단 거리나 임의의 경로를 찾고 싶을 때 유용, 큐 이용

     동작 과정 :

     1)탐색 시작 노드를 큐에 삽입하고 방문처리

     2)큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입, 방문 처리

     3) 2)의 과정을 더 이상 수행할 수 없을때까지 반복

     ```python
     from collections import deque
     
     def bfs(graph, start, visited):
         q=deque([start])
         visited[start]=1
         while q:
             v=q.popleft()
             print(v,end=" ")
             for i in graph[v]:
                 if visited[i]==0:
                     q.append(i)
                     visited[i]=1
     
     graph=[
         [],
         [2,3,8],
         [1,7],
         [1,4,5],
         [3,5],
         [3,4],
         [7],
         [2,6,8],
         [1,7]
         ]
     
     visited=[0]*9
     
     bfs(graph, 1, visited)
     #1 2 3 8 7 4 5 6
     ```

3. 실전문제

   1) 음료수 얼려먹기

   N*M크기의 얼음 틀, 구멍이 뚫려 있는 부분은 0, 칸막이가 존재하는 부분은 1로 표시

   얼음 틀의 모양이 주어졌을 때 생성되는 총 아이스크림의 개수를 구하는 프로그램을 작성

   ex)00110

   ​     00011

   ​     11111

   ​     00000

   =>아이스크림 3개 생성

   모든 노드를 따져봐야하기 때문에 dfs이용

   ```python
   n,m=map(int,input().split())
   
   graph=[]
   for i in range(n):
       graph.append(list(map(int,input())))
       
   def dfs(x,y):
       if x<=1 or x>=n or y<= -1 or y>=m:
           return false
       #여기 들어가는 순간 true
       if graph[x][y]==0:
           graph[x][y]=1
           dfs(x-1,y)
           dfs(x,y-1)
           dfs(x+1,y)
           dfs(x,y+1)
           return True
       return False
   
   count=0
   for i in range(n):
       for j in range(m):
           if dfs(i,j)==True:
               count+=1
         
   print(count)      
   ```
   
   2)미로탈출
   
   n,m크기의 미로, 시작위치는 (1,1) 미로의 출구는 (n,m)
   
   괴물이 있는 부분은 0, 괴물이 없는 부분은 1
   
   탈출하기 위해 움직여야하는 최소 칸의 개수
   
   ```python
   from collections import deque
   
   n,m=map(int,input().split())
   
   graph=[]
   for i in range(n):
       graph.append(list(map(int,input())))
       
   dx=[-1,1,0,0]
   dy=[0,0,-1,1]
   
   def bfs(x,y):
       queue=deque()
       queue.append((x,y))
       while queue:
           x,y=queue.popleft()
           for i in range(4):
               nx=x+dx[i]
               ny=y+dy[i]
               if nx<0 or ny<0 or nx>=n or ny>=m:
                   continue
               if graph[nx][ny]==0:
                   continue
               if graph[nx][ny]==1:
                   graph[nx][ny]=graph[x][y]+1
                   queue.append((nx,ny))
       return graph[n-1][m-1]
   
   print(bfs(0,0))   
   ```
   
   

