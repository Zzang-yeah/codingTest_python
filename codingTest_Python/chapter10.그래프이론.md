# chapter10.그래프이론

1. 다양한 그래프 알고리즘

   - 서로소 집합 : 공통 원소가 없는 두 집합

     서로소 집합 자료구조 : 서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조

     union+find 연산으로 조작가능=>union-find 자료구조 라고도 불림

     > - 트리를 이용해 서로소 집합을 계산하는 알고리즘
     >
     > 1. union(합집합)연산을 확인하여 서로 연결된 두 노드 A,B를 확인
     >
     >    A와 B의 루트 노드 A',B'를 찾고 A'를 B'의 부모노드로 설정(B'가 A'를 가리키도록)
     >
     > 2. 모든 union(합집합)연산을 처리할 때까지 1과정 반복

     ```python
     #기본적인 서로소 집합 알고리즘 소스코드
     
     #특정 원소가 속한 집합을 찾기
     def find_parent(parent, x):
         #루트 노드가 아니라면 루트노드를 찾을 때까지 재귀
         if parent[x]!=x:
             return find_parent(parent,parent[x])
         return x
     
     #두 원소가 속한 집합을 합치기
     def union_parent(parent, a, b):
         a=find_parent(parent, a)
         b=find_parent(parent, b)
         if a<b:
             parent[b]=a
         else:
             parent[a]=b
             
     #노드의 개수와 간선(union)의 개수 입력받기
     v,e=map(int,input().split())
     parent=[0]*(v+1)	#부모 테이블 초기화
     
     #부모 테이블에서 부모를 자기 자신으로 초기화
     for i in range(1, v+1):
         parent[i]=i
         
     #union연산
     for i in range(e):
         a,b=map(int,input().split())
         union_parent(parent, a,b)
         
     #각 원소가 속한 집합 출력
     print('각 원소가 속한 집합 : ', end='')
     for i in range(1, v+1):
         print(find_parent(parent,i), end='')
     print()
     
     #부모 테이블 내용 출력
     print('부모 테이블 : ', end='')
     for i in range(1, v+1):
         print(parent[i], end='')
     ```

     ```python
     #개선된 서로소 집합 알고리즘 소스코드
     
     #특정 원소가 속한 집합을 찾기
     #개선된 부분!경로 압축(path compression)기법
     #이렇게하면 부모테이블이 바로 루트노드를 향하게 갱신
     def find_parent(parent, x):
         #루트 노드가 아니라면 루트노드를 찾을 때까지 재귀
         if parent[x]!=x:
             parent[x] = find_parent(parent,parent[x])
         return parent[x]
     
     #두 원소가 속한 집합을 합치기
     def union_parent(parent, a, b):
         a=find_parent(parent, a)
         b=find_parent(parent, b)
         if a<b:
             parent[b]=a
         else:
             parent[a]=b
             
     #노드의 개수와 간선(union)의 개수 입력받기
     v,e=map(int,input().split())
     parent=[0]*(v+1)	#부모 테이블 초기화
     
     #부모 테이블에서 부모를 자기 자신으로 초기화
     for i in range(1, v+1):
         parent[i]=i
         
     #union연산
     for i in range(e):
         a,b=map(int,input().split())
         union_parent(parent, a,b)
         
     #각 원소가 속한 집합 출력
     print('각 원소가 속한 집합 : ', end='')
     for i in range(1, v+1):
         print(find_parent(parent,i), end='')
     print()
     
     #부모 테이블 내용 출력
     print('부모 테이블 : ', end='')
     for i in range(1, v+1):
         print(parent[i], end='')
     ```

     > - 서로소 집합을 활용한 사이클 판별
     >
     > 1. 각 간선을 확인하며 두 노드의 루트 노드를 확인
     >    - 루트 노드가 서로 다르다면 두 노드에 대해 union
     >    - 루트 노드가 서로 같다면 사이클 발생!
     >
     > 2. 그래프에 포함되어 있는 모든 간선에 대하여 1과정 반복

     ```python
     #서로소 집합을 활용한 사이클 판별
     #모든 간선 하나씩 확인, 무방향 그래프에만 적용가능
     
     #특정 원소가 속한 집합을 찾기
     def find_parent(parent, x):
         #루트 노드가 아니라면 루트노드를 찾을 때까지 재귀
         if parent[x]!=x:
             parent[x] = find_parent(parent,parent[x])
         return parent[x]
     
     #두 원소가 속한 집합을 합치기
     def union_parent(parent, a, b):
         a=find_parent(parent, a)
         b=find_parent(parent, b)
         if a<b:
             parent[b]=a
         else:
             parent[a]=b
             
     #노드의 개수와 간선(union)의 개수 입력받기
     v,e=map(int,input().split())
     parent=[0]*(v+1)	#부모 테이블 초기화
     
     #부모 테이블에서 부모를 자기 자신으로 초기화
     for i in range(1, v+1):
         parent[i]=i
         
     cycle=False	#사이클 발생여부
         
     for i in range(e):
         a,b=map(int,input().split())
         #사이클이 발생시 종료
         if find_parent(parent,a)==find_parent(parent,b):
             cycle=True
             break
         #사이클이 발생하지 않았다면 합집합(union)수행
         else:
             union_parent(parent,a,b)
             
     if cycle:
         print("사이클 발생O!")
     else:
         print("사이클 발생X!")
     ```

   - 신장 트리

     하나의 그래프가 있을 때 모든 노드를 포함하면서 사이클이 존재하지 않는 부분 그래프

     트리의 성립 조건 : 모든 노드가 포함되어 서로 연결되면서 사이클이 존재하지 않는다.->그래서 신장'트리'

     최소 신장 트리 알고리즘 : 신장 트리 중에서 최소 비용으로 만들 수 있는 신장 트리를 찾는 알고리즘, 대표적으로 크루스칼 알고리즘이 있음

     > - 크루스칼 알고리즘
     >
     > 1. 간선 데이터를 비용에 따라 오름차순으로 정렬
     > 2. 간선을 하나씩 확인하며 현재의 간선이 사이클을 발생시키는지 확인
     >    - 사이클이 발생하지 않으면 최소 신장 트리에 포함O
     >    - 사이클이 발생하면 최소 신장 트리에 포함X
     >
     > 3. 모든 간선에 대하여 2과정 반복

     ```python
     #크루스칼 알고리즘
     
     #특정 원소가 속한 집합을 찾기
     def find_parent(parent, x):
         #루트 노드가 아니라면 루트노드를 찾을 때까지 재귀
         if parent[x]!=x:
             parent[x] = find_parent(parent,parent[x])
         return parent[x]
     
     #두 원소가 속한 집합을 합치기
     def union_parent(parent, a, b):
         a=find_parent(parent, a)
         b=find_parent(parent, b)
         if a<b:
             parent[b]=a
         else:
             parent[a]=b
             
     #노드의 개수와 간선(union)의 개수 입력받기
     v,e=map(int,input().split())
     parent=[0]*(v+1)	#부모 테이블 초기화
     
     #모든 간선을 담을 리스트와 최종 비용을 담을 변수
     edges=[]
     result=0
     
     #부모 테이블에서 부모를 자기 자신으로 초기화
     for i in range(1, v+1):
         parent[i]=i
         
     #모든 간선에 대한 정보를 입력받기
     for _ in range(e):
         a,b,cost=map(int,input().split())
         #비용순으로 정렬하기 위해서 튜플의 첫 번째 원소를 비용으로 설정
         edges.append((cost,a,b))
     
     #간선을 비용순으로 정렬
     edges.sort()
     
     #간선을 하나씩 확인
     for edge in edges:
         cost,a,b=edge
         #사이클이 발생하지 않는 경우에만 집합에 포함
         if find_parent(parent,a)!=find_parent(parent,b):
             union_parent(parent,a,b)
             result+=cost
             
     print(result)
     ```

     크루스칼 알고리즘의 시간 복잡도 : O(ElogE)

     크루스칼 알고리즘에서 시간이 가장 오래 걸리는 부분은 간선을 정렬하는 작업

     =>E개의 데이터를 정렬했을 때 시간 복잡도 : O(ElogE)

   - 위상 정렬

     순서가 정해져 있는 일련의 작업을 차례대로 수행해야 할 때 사용할 수 있는 알고리즘

     방향 그래프의 모든 노드를 방향성에 거스르지 않도록 순서대로 나열하는 것

     > - 위상정렬 알고리즘
     >
     > 1. 진입 차수가 0인 노드를 큐에 넣음
     >
     > 2. 큐가 빌때까지 아래의 과정을 반복
     >
     >    큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거하고 새롭게 진입차수가 0이 된 노드를 큐에 넣음

     모든 원소를 방문하기 전에 큐가 빈다면 사이클이 존재하는 것!

     ```python
     #위상 정렬
     from collections import deque
     
     #노드의 개수와 간선의 개수 입력받기
     v,e=map(int,input().split())
     #모든 노드에 대한 진입차수는 0으로 초기화
     indegree=[0]*(v+1)
     graph=[[] for i in range(v+1)]
     
     #방향 그래프의 모든 간선 정보를 입력받기
     for _ in range(e):
         a,b=map(int,input().split())
         graph[a].append(b)	#정점 A에서 B로 이동 가능
         #진입 차수를 1증가
         indegree[b]+=1
         
     #위상 정렬 함수
     def topology_sort():
         result=[]	#알고리즘 수행 결과를 담을 리스트
         q=deque()	#큐 기능을 위한 deque 라이브러리 사용
         
         #처음 시작할 때는 진입차수가 0인 노드를 큐에 삽입
         for i in range(1,v+1):
             if indegree[i]==0:
                 q.append(i)
                 
         #큐가 빌 때까지 반복
         while q:
             #큐에서 원소 꺼내기
             now=q.popleft()
             result.append(now)
             #해당 원소와 연결된 노드들의 진입차수에서 1빼기
             for i in graph[now]:
                 indegree[i]-=1
                 #새롭게 진입 차수가 0이 되는 노드를 큐에 삽입
                 if indegree[i]==0:
                     q.append(i)
     	#위상 정렬을 수행한 결과 출력
         for i in result:
             print(i,end='')
             
     topology_sort()                   
     ```

     위상 정렬의 시간 복잡도 : O(V+E)

     차례대로 모든 노드를 확인하면서 해당 노드에서 출발하는 간선을 차례대로 제거

     =>결과적으로 노드와 간선을 모두 확인