# chapter7.이진탐색

1. 범위를 반씩 좁혀가는 탐색

   - 순차 탐색

     리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법

     보통 정렬되지 않은 리스트에서 데이터를 찾아야할 때 사용

     리스트 내에 데이터가 아무리 많아도 시간만 충분하다면 항상 원하는 원소를 찾을 수 있음

     리스트의 데이터에 하나씩 방문하며 특정한 문자열과 같은지 검사하므로 구현도 간단

     ```python
     def sequential_search(n,target,array):
         for i in range(n):
             if array[i]==target:
                 return i+1
     
     print("생성할 원소 개수를 입력한 다음 한 칸 띄고 찾을 문자열을 입력하세요")
     input_data=input().split()
     n=int(input_data[o])
     target=input_data[1]
     
     print("앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다.")
     array=input().split()
     
     print(sequential_search(n,target,array))
     ```

     순차 탐색의 시간복잡도 : O(N)

   - 이진 탐색

     배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘, 탐색 범위가 절반씩 좁혀가며 탐색

     변수 3개(시작점, 끝점, 중간점)이용, 찾으려는 데이터와 중간점위치에 있는 데이터를 반복적으로 비교

     ```python
     #재귀함수로 구현
     def binary_search(array, target, start, end):
         #target을 못 찾은 경우
         if start>end:
             return None	#파이썬은 NULL대신 None사용
         mid = (start+end)//2
         
         #target==mid
         if array[mid]==target:
             return mid
         #target<mid
         elif array[mid]>target:
             return binary_search(array,target,start,mid-1)
         #target>mid
         else:
             return binary_search(array, target, mid+1,end)
         
     #원소의 개수(n)와 찾고자하는 문자열(target) 입력받기
     n,target=list(map(int,input().split()))
     #전체 원소 입력받기
     array=list(map(int,input().split()))
     
     #이진 탐색
     result=binary_search(array,target,0,n-1)
     if result==None:
         print("원소 존재X")
     else:
         print(result+1)
     ```

     ```python
     #반복문으로 구현
     def binary_search(array, target, start, end):
         while start<=end:
             mid=(start+end)//2
             #target==mid
             if array[mid]==target:
                 return mid
             #target<mid
             elif array[mid]>target:
                 end=mid-1
             #target>mid
             else:
                 start=mid+1
         return None
     ```

     이진 탐색의 시간 복잡도 : O(log N)

   - 트리 자료구조

     그래프 자료구조의 일종으로 DB시스템이나 파일 시스템과 같은 곳에서 많은 양의 데이터를 관리하기 위한 목적으로 사용

     특징 :

     -부모 노드와 자식 노드의 관계로 표현

     -최상단 노드 : 루트노드

     -최하단 노드 : 단말노드

     -일부를 떼어내도 트리구조(=서브 트리라고 칭함)

     -파일 시스템과 같이 계층적이고 정렬된 데이터를 다루기에 적합

   - 이진 탐색 트리

     이진 탐색이 동작 할 수 있도록 고안된, 효율적인 탐색이 가능한 자료구조

     특징 :

     -부모 노드보다 왼쪽 자식 노드가 작음

     -부모 노드보다 오른쪽 자식 노드가 큼

     =>왼쪽 자식 노드<부모 노드<오른쪽 자식 노드

   - 빠르게 입력받기

     입력 데이터가 많은 경우 input()을 사용하면 시간초과 가능성 존재!

     ->sys라이브러리 readline()함수 사용!

     ex)c++ : cin, cout > scanf, printf, java : Scanner > BufferedReader

     ```python
     import sys
     input_data=sys.stdin.readline().rstrip()
     ```

2. 실전 문제 - 부품찾기

   - 문제 

     부품 N개가 존재하고 각각은 정수 형태의 고유한 번호가 존재

     손님이 M개 종류의 부품을 대량으로 구매요청

     가게 안에 부품이 모두 있는 지 확인하는 프로그램작성

     손님이 요천한 부품 번호의 순서대로 부품을 확인해 부품이 있으면 yes, 없으면 no출력

     ```python
     #내가 푼 것
     def binary_search(array, target, start, end):
         while start<=end:
             mid=(start+end)//2
             #target==mid
             if array[mid]==target:
                 return mid
             #target>mid
             elif array[mid]>target:
                 end=mid-1
             #target<mid
             else:
                 start=mid+1
         return None
     
     n=int(input())
     arrayN=list(map(int,input().split()))
     m=int(input())
     arrayM=list(map(int,input().split()))
     
     arrayN.sort()
     for i in range(m):
         a=binary_search(arrayN, arrayM[i], 0, n)
         if a!=None:
             print("Yes", end=' ')
         else:
             print("No", end=' ')
     ```

     ```python
     #존재하는지만 확인하면 되니까 in써서 봐도 됨 /책에선 왜 굳이 set을 썼는지...?
     n=int(input())
     arrayN=list(map(int,input().split()))
     m=int(input())
     arrayM=list(map(int,input().split()))
     
     arrayN.sort()
     for i in arrayM:
         if i in arrayN:
             print("Yes")
         else:
             print("No")
     ```

3. 실전 문제 - 떡볶이 떡 만들기

   - 문제

     떡의 길이가 일정하지 않음, 한 봉지 안에 들어가는 떡의 총 길이는 절단기로 잘라서 맞춰줌

     절단기에 높이(H)를 지정하면 줄지어진 떡을 한 번에 절단

     손님이 왔을 때 요청한 총 길이가 M일때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램 작성

     ```python
     n,m=map(int,input().split())	#n:떡의 개수, m:손님이 요청한 총 떡의 길이
     ttok=list(map(int,input().split()))	#떡의 개별 높이
     
     #array:떡의 개별 높이/target=m/start=0/end=떡의 개별 높이 중 최대값
     def cut_ttok(array,target, start,end):
         while start<=end:
             mid=(start+end)//2
             result=0	
             #이진탐색으로 구한 mid로 떡을 개별적으로 자른 후 총 떡의 길이(result)
             for i in array:
                 #떡의 길이가 mid보다 작으면 무시하고 진행
                 if i - mid <0:
                     continue
                 #떡의 길이가 mid보다 커야만 잘리기 때문에 이때만 더해줌
                 else:
                     result+=i-mid
             #mid로 자른 후 총 떡의 길이=손님이 요청한 총 떡의 길이일 경우 mid(절단기 높이)리턴
             if result==target:
                 return mid
             #mid로 자른 후 총 떡의 길이!=손님이 요청한 총 떡의 길이일 경우
             else:
                 #mid로 자른 후 총 떡의 길이 < 손님이 요청한 총 떡의 길이->절단기 높이 감소
                 if result < target:
                     end=mid-1
                 #mid로 자른 후 총 떡의 길이 > 손님이 요청한 총 떡의 길이->절단기 높이 증가
                 else:
                     start=mid+1
     
     h=cut_ttok(ttok, m, 0, max(ttok))
     print(h)
     ```

4. 367p

   ```python
   n, x=map(int,input().split())
   array=list(map(int,input().split()))
   
   start_index=0
   end_index=0
   
   if x in array:
       #처음 시작 원소 인덱스찾기
       start=0
       end=len(array)
       while start<=end:
           mid=(start+end)//2
           if array[mid]>x:
               end=mid-1
           elif array[mid]<x:
               start=mid+1
           #array[mid]==x
           else:
               if array[mid-1]!=x or mid==0:
                   start_index=mid
                   break
               else:
                   end=mid-1              
       start=0
       end=len(array)
       #마지막 원소 인덱스찾기
       while start<=end:
           mid=(start+end)//2
           if array[mid]>x:
               end=mid-1
           elif array[mid]<x:
               start=mid+1
           #array[mid]==x
           else:
               if (mid==len(array)-1) or (array[mid+1]!=x):
                   end_index=mid
                   break
               else:
                   start=mid+1
       print(end_index-start_index+1)
   else:
       print("-1")
   ```

   



