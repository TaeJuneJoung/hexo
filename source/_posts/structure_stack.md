---
title: 자료구조] Stack
date: 2019-12-09 22:51:30
tags: ['자료구조', 'Stack']
categories: ['자료구조']
---

- 데이터를 제한적으로 접근할 수 있는 구조
  - 한쪽 끝에서만 자료를 넣거나 빼낼 수 있는 구조
- FILO

대표적인 스택의 활용

- 컴퓨터 내부의 프로세스 구조의 함수 동작 방식



## 기능

- push() : 데이터를 스택에 넣기
- pop() : 데이터를 스택에서 꺼내기



## 스택 구조와 프로세스 스택

스택 구조는 프로세스 실행 구조의 가장 기본

- 함수 호출시 프로세스 실행 구조를 스택과 비교해서 이해 필요

```python
def recursive(data):
    if data < 0:
        print('ended')
    else:
        print(data)
        recursive(data-1)
```



## 스택의 장단점

### 장점

- 구조가 단순해서, 구현이 쉽다
- 데이터 저장/읽기 속도가 빠르다

### 단점

- 데이터 최대 갯수를 미리 정해야 한다
  - 파이썬의 경우 재귀 함수는 1000번까지만 호출 가능
- 저장 공간의 낭비가 발생할 수 있음
  - 미리 최대 갯수만큼 저장 공간을 확보해야함



## 스택 구현

**파이썬 기본 기능 사용하기**

- append(push), pop

```python
data_stack = list()

data_stack.append(1)
data_stack.append(2)

print(data_stack) #[1, 2]
data_stack.pop() #2
```



**파이썬 append, push 사용하지 않고 구현**

```python
data_stack = list()

def push(data):
    data_stack.append(data)
    
def pop():
    data = data_stack[-1]
    del data_stack[-1]
    return data
```

