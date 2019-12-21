---
title: Python] Comprehension
date: 2019-12-11 13:01:38
tags: ['Python', 'Comprehension']
categories: ['Python']
---

:  리스트 컴프리헨션은 간편하게 리스트를 만드는 방법 

- List Comprehension
- Tuple Comprehension
- Set Comprehension
- Dictonary Comprehension



 리스트에 0~9의 값을 넣는 로직 만들어보기

```python
list_logic = []
for i in range(10):
    list_logic.append(i)
    
print(list_logic)
```



## List Comprehension, 리스트 컴프리헨션

아래와 같이 리스트 컴프리헨션을 이용하여 위의 내용을 쉽게 한 줄로 만들 수 있다.

```python
list_comprehension = [i for i in range(10)]
print(list_comprehension)
```



## Tuple Comprehension, 튜플 컴프리헨션

튜플 컴프리헨션은 주의할 점이 있다. 

`(i for i in range(10))`으로 진행시, **generator객체**가 나온다.

```python
tuple_comprehension = tuple(i for i in range(10))
print(tuple_comprehension) #(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
print(type(tuple_comprehension)) #<class 'tuple'>
```





## Set Comprehension, 집합 컴프리헨션

```python
set_comprehension = {i for i in range(10)}
print(set_comprehension) #{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
print(type(set_comprehension)) #<class 'set'>
```





## Dictonary Comprehension, 딕셔너리 컴프리헨션

딕셔너리(HashMap형태)는 `key:value` 형태로 만들어줘야하기에 아래와 같이 코드를 작성하면 된다.

```python
dict_comprehension = {i:i for i in range(10)} #key : value
print(dict_comprehension) #{0: 0, 1: 1, 2: 2, 3: 3, 4: 4, 5: 5, 6: 6, 7: 7, 8: 8, 9: 9}
print(type(dict_comprehension)) #<class 'dict'>
```







## 심화 Comprehension

그러면 이제 For문 하나는 해결했으니 Comprehension형태에서 IF문을 사용하며 이중 이상의 For문을 사용하는 방법에 대해서 알아보자.

방식에 대해서는 위에서 언급되었으니 List형태의 Comprehension만 다루기로 한다.



### For + If

#### 1~20까지의 숫자에서  홀수만 뽑아보자

```python
odd_numbers = [i for i in range(1, 20) if i%2]
```



#### 1~50까지의 숫자에서 3의 배수만 뽑아보자

```python
multiple_of_three_numbers = [i for i in range(1, 50) if i%3 == 0]
```



#### 1~50까지의 숫자에서 4의 배수에서 +1이나 +2인 값만 뽑아보자

```python
answer = [i for i in range(1, 50) if i%4 == 1 or i%4 == 2]
```





### 2중For문

#### 구구단을 만들어보자(시작은 2단부터 끝은 9단)

```python
multiplication_table = [f'{i}x{j}={i*j}' for i in range(2, 10) for j in range(1, 10)]
```



### 2중For문 + If문

#### 구구단에서 i나 j가 3의 배수이거나 곱의 결과가 3의 배수인 것을 다 제외

```python
multiplication_if_table = [f'{i}x{j}={i*j}' for i in range(2, 10) for j in range(1, 10) if i%3 and j%3 and i*j%3]
```