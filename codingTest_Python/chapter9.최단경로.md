# chapter9.최단경로

1. 가장 빠른 길 찾기

   - 가장 빠르게 도달하는 방법

     최단 경로 알고리즘 : 가장 짧은 경로를 찾는 알고리즘(=길찾기 문제)

   - 다익스트라 최단 경로 알고리즘

     그래프에서 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘

     한 단계당 하나의 노드에 대한 최단 거리를 확실히 찾는 것

     단, 음의 간선이 없을 때 정상적으로 작동(현실에선 간선(길)이 음의 수를 가지지 않기 때문에 GPS에 사용)

     그리디 알고리즘으로 분류=>매번 가장 비용이 적은 노드를 선택해서 임의의 과정 반복

     > 1. 출발노드를 설정한다.
     > 2. 최단 거리 테이블을 초기화
     > 3. 방문하지 않은 노드 중 최단 거리가 가장 짧은 노드 선택
     > 4. 해당 노드를 거쳐 다른 노드로 가는 비용 계산, 최단 거리 테이블 갱신
     > 5. 3,4번 과정 반복

     각 노드에 대한 현재까지의 최단 거리 정보를 항상 1차원 리스트에 저장하며 리스트를 계속 갱신

     매번 현재 처리하고 있는 노드를 기준으로 주변 간선 확인

     +파이썬에서 1e9를 입력하면 실수형으로 처리 정수형으로 처리하고 싶으면 형변환시켜줄것 int(1e9)

     ```python
     #간단한 다익스트라 알고리즘, 전혀 간단하지가 않은데...?
     #단계마다 방문하지 않은 노드 중에서 최단거리가 가장 짧은 노드를 선택하기 위해 매 단계마다 1차원 리스트의 모든 원소를 확인(순차탐색)
     import sys
     input=sys.stdin.readline
     INF=int(1e9)
     
     n,m=map(int,input().split())	#노드의 개수, 간선의 개수
     start=int(input())				#시작노드
     graph=[[] for i in range(i+1)]	#노드에 대한 정보를 담는 리스트
     visited=[False]*(n+1)			#방문체크
     distance=[INF]*(n+2)			#최단 거리 테이블
     
     for _ in range(m):
         a,b,c=map(int,input().split())	#a->b로 가는 비용:c
         graph[a].append((b,c))
         
     def get_smallest_node():
         min_value=INF
         index=0
         for i in range(1, n+1):
             if distance[i]<min_value and not visited[i]:
                 min_value=distance[i]
                 index=i
         return index
         
     def dijkstra(start):
         distance[start]=0
         visited[start]=True
         for j in graph[start]:
             distance[j[0]]=j[1]
         for i in range(n-1):
             now = get_smallest_node()
             visited[now]=True
             for j in graph[now]:
                 cost=distance[now]+j[i]
                 if cost<distance[j[0]]:
                     distance[j[0]]=cost
     dijkstra(start)
     
     for i in range(1,n+1):
         if distance[i]==INF:
             print("못 가!")
         else:
             print(distance[i])
     #아무래도 동빈씨는 간단하다 의 의미를 모르시는듯,,,
     ```

     간단(?)한 다익스트라 알고리즘의 시간 복잡도 : O(V^2), V : 노드의 개수

     ```python
     #개선된 다익스트라 알고리즘
     #우선순위가 가장 높은 데이터를 가장 먼저 삭제하는 우선순위 큐를 구현하기 위한 힙구조 사용
     #라이브러리 속도 : PriorityQueue > heapq
     #현재 가장 가까운 노드를 저장하기 위한 목적으로만 우선순위 큐 추가 이용
     import heapq
     import sys
     input=sys.stdin.readline
     INF=int(1e9)
     
     n,m=map(int,input().split())
     start=int(input())
     graph=[[]for i in range(n+1)]
     distance=[INF]*(n+1)
     
     for _ in range(m):
         a,b,c=map(int,input().split())
         graph[a].append((b,c))
         
     def dijkstra(start):
         q=[]
         heapq.heappush(q,(0,start))
         distance[start]=0
         while q:
             dist, now=heapq.heappop(q)
             if distance[now]<dist:
                 continue
             for i in graph[now]:
                 cost=dist+i[1]
                 if cost<distance[i[0]]:
                     distance[i[0]]=cost
                     heapq.heappush(q, (cost,i[0]))
     dijkstra(start)
     for i in range(1,n+1):
      if distance[i]==INF:
             print("INF")
      else:
             print(distance[i])
  ```
   
  개선된 다익스트라 알고리즘의 시간 복잡도 : O(ElogV), E : 간선의 개수 V : 노드의 개수
   
  =>다익스트라 알고리즘 : 한 지점에서 다른 특정 지점까지의 최단 경로를 구해야 하는 경우
   
- 플로이드 워셜 알고리즘
   
     모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우
   
     다이나믹 프로그래밍(노드의 개수가 N일때, N번 반복하며 점화식에 맞게 2차원 리스트 갱신)
   
     ```python
     INF=int(1e9)
     
     n=int(input())
     m=int(input())
     graph=[[INF]*(n+1) for _ in range(n+1)]
     
     for a in range(1, n+1):
         for b in range(1, n+1):
             if a==b:
                 graph[a][b]=0
                 
     for _ in range(m):
         a,b,c=map(int,input().split())
         graph[a][b]=c
         
  for k in range(1, n+1):
         for a in range(1, n+1):
          for b in range(1, n+1):
                 graph[a][b]=min(graph[a][b], graph[a][k]+graph[k][b])
     ```
   
   - p.385 플로이드
   
     ```python
     INF=int(1e9)
     
     n=int(input())
     m=int(input())
     graph=[[INF]*(n+1) for _ in range(n+1)]
     
     for a in range(1, n+1):
         for b in range(1, n+1):
             if a==b:
                 graph[a][b]=0
     
     for _ in range(m):
         a,b,c=map(int,input().split())
         if graph[a][b]!=c:
             c=min(graph[a][b],c)
         graph[a][b]=c
     
     for k in range(1, n+1):
         for a in range(1, n+1):
             for b in range(1, n+1):
                 graph[a][b]=min(graph[a][b], graph[a][k]+graph[k][b])
     
  for i in range(1, n+1):
         for j in range(1, n+1):
          print(graph[i][j], end=' ')
         print("")
     ```
   
- p.386 정확한 순위
   
     ```python
     
     ```
   
     

