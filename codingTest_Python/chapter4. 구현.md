# chapter4. 구현

1. 피지컬로 승부하기

   구현 : 머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정

   구현하기 어려운 문제 ex)알고리즘은 간단한데 코드가 지나칠 만큼 길어지는 문제, 특정 소수점 자리까지 출력해야 하는 문제, 문자열이 입력으로 주어졌을 때 한 문자 단위로 끊어서 리스트에 넣어야하는(파싱을 해야 하는)문제 등 -> 사소한 조건 설정이 많은 문제

   완전 탐색 : 모든 경우의 수를 주저 없이 다 계산하는 해결 방법

   시뮬레이션 : 문제에서 제시한 알고리즘을 한 단계씩 차례대로 직접 ㅎ수행

2. 구현 시 고려해야할 메모리 제약 사항

   파이썬은 자료형을 지정할 필요도 없고 매우 큰 수의 연산도 지원 파이썬 짱!

   - int 자료형 데이터의 개수에 따른 메모리 사용량

     | 데이터의 개수(리스트의 길이) | 메모리 사용량 |
     | ---------------------------- | ------------- |
     | 1,000                        | 약 4KB        |
     | 1,000,000                    | 약 4MB        |
     | 10,000,000                   | 약 40MB       |

3. 채점환경

   파이썬은 느려서 c/c++에 비해 2배의 수행 시간 제한을 적용하기도 함

4. 구현 문제에 적용하는 방법

   1)예제4-1 상하좌우

   ```python
   #예제4-1
   n=int(input())  #공간의 크기
   position=[1,1]
   s=list(map(str,input().split()))
   direction=['L','R','U','D']
   
   for i in s:
       if i==direction[0]:
           if position[1]==1:
               continue
           position[1]-=1
       elif i==direction[1]:
           if position[1]==n:
               continue
           position[1]+=1
       elif i==direction[2]:
           if position[0]==1:
               continue
           position[0]-=1
       else:
           if position[0]==n:
               continue
           position[0]+=1
   print(position)
   ```

   2)예제4-2 시각

   ```python
   #예제 4-2
   n=int(input())
   
   result=0
   for i in range(n+1):
       if '3' in str(i):   #3이 들어간 경우
           result+=60*60
       else:               #안 들어간 경우(15(03,13,23,43,30-39,53)*60/나머지 45분*15)
           result+=1575
   print(result)
   ```

   정답에선 3중 for문 사용해서 완전탐색

   3)실전문제 2. 왕실의 나이트

   ```python
   #실전문제2
   position=input()
   num=[0,0]
   num[0]=int(ord(position[0]))
   num[1]=int(ord(position[1]))
   
   #수평2수직1
   def hor(a, arr):
       count=0
       arr[0]+=2*a
       if (arr[0]<97) or (arr[0]>104):
           return 0
       arr[1]-=1
       if arr[1]>1:
           count+=1
       arr[1]+=2
       if arr[1]<9:
           count+=1
       return count
   
   #수직2수평1
   def ver(a, arr):
       count=0
       arr[1]+=2*a
       if (arr[1]<1) or (arr[1]>8):
           return 0
       arr[0]-=1
       if arr[0]>97:
           count+=1
       arr[0]+=2
       if arr[0]<104:
          count+=1
       return count
   
   result=0
   result+=hor(1, num)
   result+=hor(-1,num)
   result+=ver(1,num)
   result+=ver(-1,num)
   print(result)
   ```

   ->에러뜸

   4)실전문제3. 게임개발

   ```
   뇌정지
   ```

   



