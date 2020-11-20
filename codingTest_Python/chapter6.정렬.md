# chapter6.정렬

1. 기준에 따라 데이터를 정렬

   - 정렬 알고리즘 개요

     정렬(sort):데이터를 특정한 기준에 따라서 순서대로 나열

   - 선택 정렬

     데이터가 무작위로 여러 개 있을 때, 이 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸고, 그 다음 작은 데이터를 선택해 앞에서 두번째 데이터와 바꾸는 과정을 반복

     가장 원시적인 방법

     매번 가장 작은 것을 ''선택''

     ```python
     array=[7,5,9,0,3,1,6,2,4,8]
     
     for i in range(len(array)):
         min_index=i	#가장 작은 원소의 인덱스
         for j in range(i+1, len(array)):
             if array[min_indes]>array[j]:
                 min_index=j
         array[i], array[min_index]=array[min_index], array[i]	#스와프
     print(array)
     #[0,1,2,3,4,5,6,7,8,9]
     ```

     ```python
     #python swap
     array=[3,5]
     array[0],array[1]=array[1],array[0]
     ```

     선택 정렬의 시간 복잡도 : 

     n-1번만큼 가장 작은 수를 찾아서 맨 앞으로 보내는 코드를 반복해야하고 매번 가장 작은 수를 찾기 위해 비교 연산이 필요=>O(N^2), 느령!

     특정한 리스트에서 가장 작은 데이터를 찾을 때

   - 삽입 정렬

     데이터를 하나씩 확인하며, 각 데이터를 적절한 위치에 삽입

     필요할 때만 위치를 바꾸므로 데이터가 거의 정렬되어 있을 때 효율적(선택정렬은 현재 데이터 상태와 상관없이 무조건 모든 원소를 비교하고 위치를 바꿈)

     특정한 데이터가 적절한 위치에 들어가기 이전에 그 앞까지의 데이터는 이미 정렬되어 있다고 가정

     ```python
     array=[7,5,9,0,3,1,6,2,4,8]
     
     for i in range(1, len(array)):
         for j in range(i, 0, -1):
             if array[j]<array[j-1]:	#한칸씩 왼쪽으로 이동
                 array[j], array[j-1]= array[j-1], array[j]
             else:	#자기보다 작은 데이터를 만나면 그 위치에서 멈춤
                 break
     ```

     삽입 정렬의 시간 복잡도:

     O(N^2)이지만 현재 리스트의 데이터가 거의 정렬되어 있는 상태라면 매우 빠르게 동작

     최선의 경우에는 O(N)의 시간 복잡도

   - 퀵 정렬

     기준 데이터를 설정하고 그 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸는 것

     기준(피벗)을 설정한 다음 큰 수와 작은 수를 교환한 후 리스트를 반으로 나누는 방식으로 동작

     피벗을 설정하고 리스트를 분할하는 방법에 따라서 여러가지 방식으로 퀵 정렬을 구분(호어 분할이 가장 대표적, 리스트에서 첫번째 데이터를 피벗으로 정하고 양쪽끝에서부터 확인해가면서 엇갈리는 순간 피벗 재정의)

     ```python
     array=[5,6,9,0,3,1,6,2,4,8]
     
     def quick_sort(array, start, end):
         if start>=end:	#원소가 1개인 경우 종료
             return
         pivot=start	#피벗은 첫번째 원소
         left=start+1
         right=end
         while left<=right:
             #피벗보다 큰 데이터를 찾을때까지 반복
             while left<=end and array[left]<=array[pivot]:
                 left+=1
             #피벗보다 작은 데이터를 찾을때까지 반복
             while right>start and array[right]>=array[pivot]:
                 right-=1
             if left>right:	#엇갈렸다면 작은 데이터와 피벗을 교체
                 array[right],array[pivot]=array[pivot],array[right]
             else : 	#엇갈리지 않았다면 작은 데이터와 큰 데이터를 교체
                 array[left],array[right]=array[right],array[left]
         #분할 이후 왼쪽 부분과 오른쪽 부분에서 각각 정렬 수행
         quick_sort(array, start, right-1)
         quick_sort(array, right+1 , end)
     
     quick_sort(array, 0, len(array)-1)
     ```

     퀵 정렬의 시간복잡도 : O(NlogN) 최악의 경우 O(N^2)->이미 데이터가 정렬되어 있는 경우 느리게 작동

   - 계수 정렬

     특정한 조건이 부합할 때만 사용할 수 있지만 매우 빠른 정렬 알고리즘

     데이터의 크기 범위가 제한되어 정수 형태로 표현할 수 있을 때만 사용가능->모든 범위를 담을 수 있는 크기의 리스트를 선언해야하기 때문

     ```python
     array=[7,5,9,0,3,1,6,2,9,1,4,8,0,5,2]
     
     count=[0]*(max(array)+1)
     
     for i in range(len(array)):
         count[array[i]]+=1
         
     for i in range(len(count)):
         for j in range(count[i]):
             print(i,end=' ')
     ```

     계수 정렬의 시간 복잡도 : O(N+K)(데이터의 개수 : N, 데이터 중 최대값의 크기 : K)

     앞에서부터 데이터를 하나씩 확인하면서 리스트에서 적절한 인데긋의 값을 1씩 증가시킬 뿐만 아니라 추후에 리스트의 각 인덱스에 해당하는 값들을 확인할 때 데이터 중 최댓값의 크기만큼 반복을 수행해야함

     기수 정렬(동작은 느리지만 처리할 수 있는 정수의 크기는 더 크고, 복잡함)과 더불어 가장 빠름

     계수 정렬의 공간 복잡도 : ex)0,999999->비효율적

     =>데이터의 크기가 한정되어 있고 데이터의 크기가 많이 중복되어 있을수록 유리, 정렬해야 하는 데이터의 개수가 매우 많을때도 효과적으로 사용가능O(N+K)

   - 정렬 라이브러리

     ```python
     array=[7,5,9,0,3,1,6,2,4,8]
     
     result=sorted(array)
     ```

     ```python
     array=[7,5,9,0,3,1,6,2,4,8]
     
     array.sort()
     print(array)
     ```

     key 매개변수를 입력으로 받을 수 있음, key값으로는 하나의 함수가 들어가야 하며 이는 정렬 기준이 됨

     ```python
     array=[('바나나',2),('사과',5),('당근',3)]
     
     def setting(data):
         return data[1]
     result = sorted(array, key=setting)
     print(result)
     ```

     

   