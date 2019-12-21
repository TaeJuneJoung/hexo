---
title: Python] 얕은복사 | 깊은복사
date: 2019-12-10 21:35:52
tags: ['Python', 'Shallow Copy', 'Deep Copy']
categories: ['Python']
---

## Shallow Copy, 얕은 복사

### 변수 할당 얕은 복사

```python
list_data = ['1','2','3', [1,2,3], 1,2,3]

list_copy = list_data

print(id(list_data))
print(id(list_copy))

# 단일 리스트 값 변경
list_data[0] = '5'
print(list_data)
print(list_copy)
print(id(list_data))
print(id(list_copy))

# 2중 리스트 값 변경
list_data[3][0] = 5
print(list_data)
print(list_copy)
print(id(list_data))
print(id(list_copy))
```

> 결과값
>
> ```
> 2327860600520
> 2327860600520
> ['5', '2', '3', [1, 2, 3], 1, 2, 3]
> ['5', '2', '3', [1, 2, 3], 1, 2, 3]
> 2327860600520
> 2327860600520
> ['5', '2', '3', [5, 2, 3], 1, 2, 3]
> ['5', '2', '3', [5, 2, 3], 1, 2, 3]
> 2327860600520
> 2327860600520
> ```

 변수를 그대로 할당하여 사용할 경우에는,
같은 `주소`를 가리켜 하나의 값이 변하였을 때 다른 변수의 값도 변하게 된다.
메모리 주소지에 있는 값이 변하여서 해당 주소지를 쓰는 변수는 동일하게 변하는 것이다. 



### 슬라이싱을 이용한 얕은 복사

```python
print('-------`:사용`------')
list_data = ['1','2','3', [1,2,3], 1,2,3]
list_copy = list_data[:]
print(id(list_data))
print(id(list_copy))

# 단일 리스트 값 변경
list_data[0] = '5'
print(list_data)
print(list_copy)
print(id(list_data))
print(id(list_copy))

# 2중 리스트 값 변경
list_data[3][0] = 5
print(list_data)
print(list_copy)
print(id(list_data))
print(id(list_copy))
```

> 결과값
>
> ```
> -------`:사용`------
> 2327860646088
> 2327860440200
> ['5', '2', '3', [1, 2, 3], 1, 2, 3]
> ['1', '2', '3', [1, 2, 3], 1, 2, 3]
> 2327860646088
> 2327860440200
> ['5', '2', '3', [5, 2, 3], 1, 2, 3]
> ['1', '2', '3', [5, 2, 3], 1, 2, 3]
> 2327860646088
> 2327860440200
> ```

 리스트 슬라이싱을 통해서 값만 가져오면 해결할 수 있다고 생각할 수 있으나,
내부에 리스트가 있거나 2중 리스트 이상의 형태일 경우에는 동일한 문제가 발생한다. 



## Deep Copy, 깊은 복사

### Copy.Deepcopy를 이용한 깊은 복사

```python
import copy

list_data = ['1','2','3', [1,2,3], 1,2,3]
list_copy = copy.deepcopy(list_data)
print(id(list_data))
print(id(list_copy))

# 단일 리스트 값 변경
list_data[0] = '5'
print(list_data)
print(list_copy)
print(id(list_data))
print(id(list_copy))

# 2중 리스트 값 변경
list_data[3][0] = 5
print(list_data)
print(list_copy)
print(id(list_data))
print(id(list_copy))
```

> 결과값
>
> ```
> 2327860646664
> 2327860646472
> ['5', '2', '3', [1, 2, 3], 1, 2, 3]
> ['1', '2', '3', [1, 2, 3], 1, 2, 3]
> 2327860646664
> 2327860646472
> ['5', '2', '3', [5, 2, 3], 1, 2, 3]
> ['1', '2', '3', [1, 2, 3], 1, 2, 3]
> 2327860646664
> 2327860646472
> ```

주소값도 다르며, 값들이 영향을 받지 않음을 알 수 있다.
이중리스트도 영향을 받지 않은 것을 보니 `깊은 복사`가 이뤄졌다. 



## [참고사항]Immutable의 복사

 Immutable 객체는 깊은 복사를 하여도 얕은 복사를 한 결과가 도출된다. 

```python
immutable_tuple = (1,2,(3,4), '5')
copy_tuple = immutable_tuple

print(id(immutable_tuple))
print(id(copy_tuple))
```

> 결과값
>
> ```
> 2327861436808
> 2327861436808
> ```



```python
import copy

immutable_tuple = (1,2,(3,4), '5')
copy_tuple = copy.deepcopy(immutable_tuple)

print(id(immutable_tuple))
print(id(copy_tuple))
```

> 결과값
>
> ```
> 2327861191688
> 2327861191688
> ```
