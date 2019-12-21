---
title: Python] Mutable vs Immutable
date: 2019-12-10 21:25:10
tags: ['Python', 'Mutable', 'Immutable']
categories: ['Python']
---

## Mutable, 가변 객체

- List, 리스트
- Dictionary, 딕셔너리
- Set, 집합



## Immutable 불변 객체

- String, 문자열

- Number, 숫자형

  Integer, Float, Complex

- Bool, 논리형

- Tuple, 튜플

- FrozenSet, 불변집합

  : 파이썬의 변경 불가인 집합 객체



## List와 Tuple 비교

Mutable과 Immutable의 대표적인 List와 Tuple을 가지고 비교해보자.



### is와 ==

- is: 주소값 비교

  `id()`: 객체의 주소값 반환

- ==: 값 비교

```python
r1 = (1,2,3)
r2 = (1,2,3)

print(r1 is r2) #컴파일시에는 True로 나옴(Idel과 Jupyter에서는 False)
print(r1 == r2)

print(id(r1), id(r2))

r1 = [1,2,3]
r2 = [1,2,3]

print(r1 is r2)
print(r1 == r2)

print(id(r1), id(r2))
```

>결과값
>
>```
>False
>True
>1954010284680 1954010286768
>False
>True
>1954009597832 1954009597896
>```

위의 내용으로 설명을 하고자 하였는데 어떤 Tool을 쓰느냐에 따라 값이 달라지는 부분이 있고,
다른 Type에 대해서는 어려운 부분이 있으니 다른 방도로 진행 



```python
def add(v1, v2):
    v1 += v2

v1 = (1,2,3)
v2 = (4,5,6)

add(v1, v2)
print(v1)

v1 = [1,2,3]
v2 = [4,5,6]

add(v1, v2)
print(v1)
```

> 결과값
>
> ```
> (1, 2, 3)
> [1, 2, 3, 4, 5, 6]
> ```



 `튜플`일 경우는 값이 추가되지 않았다는 점을 확인할 수 있다.
`리스트`인 경우는 값이 변하였다.
**왜 이러한 차이가 발생할까?** 

```python
def add(v1, v2):
    v1 += v2
    print(id(v1))

v1 = (1,2,3)
print(id(v1))
v2 = (4,5,6)

add(v1, v2)

v1 = [1,2,3]
print(id(v1))
v2 = [4,5,6]

add(v1, v2)
```

> 결과값
>
> ```
> 1954010286840
> 1954009922824
> 1954009597064
> 1954009597064
> ```

함수 안에서의 주소값을 찍어서 같은지 다른지를 확인하면 된다.
`튜플`의 경우는 다르지만, `리스트`의 경우에는 같다.
`튜플`에서는 새로운 객체가 생성되어 값을 할당받는다. 즉, Immutable객체는 새로운 객체로 선언된다 .



```python
v1 = 'Python'
print(id(v1))
v2 = 'Test'

add(v1, v2)

v1 = 3
print(id(v1))
v2 = 5

add(v1, v2)
```

> 결과값
>
> ```
> 1953967907592
> 1954010299824
> 140715235971968
> 140715235972128
> ```



 `str`타입과 `Number`타입도 Immutable하기에 같은 결과가 나옴을 확인할 수 있다.
여기서 잠시 참고하고 넘어갈 부분도 있다. Number는 Immutable하지만 

```python
v1 = 0
print(id(v1))
v2 = 0

add(v1, v2)
```

의 결과는 어떻게 나올까?



> 결과값
>
> ```
> 140715235971872
> 140715235971872
> ```



 같음을 볼 수 있다. 'Immutable할 때는 주소값이 다르다고 했는데 왜 같지?'
모순이 발생함을 알 수 있는데, 이는 파이썬 정책 때문이다. 

https://docs.python.org/3/c-api/long.html#c.PyLong_FromLong

내용을 확인해보면 파이썬은 **-5~256**값은 저장해두고 사용함을 알 수 있다.



Mutable과 Immutable의 개념은 중요하다.

이후에 다룰 내용들과 다음에 나올 **리스트(배열)의 얕은/깊은 복사에 기초가 되기 때문이다.**