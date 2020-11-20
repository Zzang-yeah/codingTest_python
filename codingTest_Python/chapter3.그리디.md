# chapter3.그리디

1. 그리디 알고리즘이란?

   현재 상황에서 지금 당장 좋은 것만 고르는 방법->매순간 가장 좋아보이는 것을 선택, 현재 선택이 나중에 미칠 영향에 대해서는 고려하지 않는다.

   사전에 외우고 있지 않아도 풀 수 있는 가능성이 높은 문제 유형 ex)가장 큰 순서대로, 가장 작은 순서대로

   *다익스트라 알고리즘은 그리디 알고리즘이면서 암기가 필요한 알고리즘

   1) 예제3-1

   ```python
   #예제3-1 거스름돈
   #가장 큰 단위부터 거슬러주기
   n=int(input())   #손님에게 거슬러 줘야할 돈 입력받기
   count=0      #거슬러줘야할 돈 - 거슬러준돈
   while(n!=0):
       if(n>500):
           count+=n//500
           n%=500
       elif (n>100):
           count+=n//100
           n%=100
       elif(n>50):
           count+=n//50
           n%=50
       else:
           count+=n//10
           n%=10
   print(count)
   ```

   -정답

   ```python
   n=1260
   count=0
   
   #큰 단위의 화폐부터 차례대로 확인
   coin_type=[500,100,50,10]
   
   for coin in coin_types:
   	count+=n//coin	#해당 화폐로 거슬러 줄 수 있는 동전의 개수 세기
   	n%=coin
   print(count)
   ```

2. 그리디 알고리즘의 정당성

   모든 문제에 적용할 수 있는 것은 아님->탐욕적으로 문제에 접근했을 때 정확한 답을 찾을 수 있다는 보장이 있을때 사용! ex)예제 3-1은 가지고 있는 동전 중에서 큰 단위가 항상 작은 단위의 배수이므로 작은 단위의 동전들을 종합해 다른 해가 나올 수 없기 때문에 정확한 답을 찾을 수 있다는 보장이 있음

   2) 실전문제2. 큰 수의 법칙

   ```python
   #실전문제 3-2
   n,m,k=map(int,input().split()) #배열의 크기, 숫자가 더해지는 횟수, 최대 연속횟수
   arr=list(map(int,input().split()))  #배열 입력받기
   
   arr.reverse()
   num1=arr[0] #첫번째로 가장 큰 수
   num2=arr[1] #두번째로 가장 큰 수
   
   arrsum=0    #큰수
   count=0     #연속 횟수 카운트
   for i in range(m):
       if(count>k):	#가장 큰 수의 최대 연속 횟수를 초과한 경우
           arrsum+=num2
           count=0
       else:			#가장 큰 수의 최대 연속 횟수가 미만인 경우
           arrsum+=num1
           count+=1
   print(arrsum)
   ```

   -정답

   ```python
   #n,m,k를 공백으로 구분하여 입력받기
   n,m,k=map(int,input().split())
   #n개의 수를 공백으로 구분하여 입력받기
   data=list(map(int,input().split()))
   
   data.sort()	#입력받은 수들 정렬하기
   first=data[n-1]	#가장 큰 수
   second=data[n-1] #두번째로 큰 수
   
   result=0
   
   while True:
       for i in range(k):	#가장 큰 수를 k번 더하기
           if m==0:	#m이 0이라면 반복문 탈출
               break
           result+=first
           m-=1	#더할때마다 1씩 빼기
       if m==0:	#m이 0이라면 반복문 탈출
           break
       result+=second	#두번째로 큰수를 한번 더하기
       m-=1	#더할때마다 1씩 빼기
   print(result)
           	
   ```

   3) 실전문제 3. 숫자 카드 게임

   ```python
   #실전문제 3-3
   n,m=map(int,input().split())    #행의개수, 열의 개수
   result=0
   for i in range(n):
       arr=list(map(int,input().split()))  #행 하나 입력받기
       minarr=min(arr)     #행에서 가장 작은 수
       result=max(result,minarr)   #이전의 것과 비교해서 큰 것을 result에 저장
   print(result)
   ```

   4)실전문제 4. 1이 될때까지

   ```python
   #실전문제  3-4
   n,m=map(int,input().split())    #어떤 수, 나눗셈할 수
   count=0		#연산 횟수
   while(n!=1):	#n이 1이 될때까지
       if(n>=m):	#n이 나눗셈할 수과 같거나 크면
           n/=m	#나누기
       else:		#n이 나눗셈할 수보다 작으면
           n-=1	#-1
       count+=1
   print(count)
   ```

