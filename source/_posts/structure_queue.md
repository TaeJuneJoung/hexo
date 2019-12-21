---
title: 자료구조] Queue
date: 2019-12-09 22:52:30
tags: ['자료구조', 'Queue']
categories: ['자료구조']
---

## 구조

**FIFO,** 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 구조



## 용어

**Enqueue**: 큐에 데이터를 넣는 기능

**Dequeue**: 큐에서 데이터를 꺼내는 기능



## 파이썬 Queue 라이브러리 활용해서 큐 자료 구조 사용하기

- queue 라이브러리에는 다양한 큐 구조로 **Queue(), LifoQueue(), PriorityQueue**... 등 제공

1. Queue(): 가장 일반적인 큐 자료 구조

   ```python
   import queue
   
   data_queue = queue.Queue()
   data_queue.put('coding')
   data_queue.put(2)
   data_queue.qsize() #2
   
   data_queue.get() #'coding'
   data_queue.qsize() #1
   
   data_qeuue.get() #2
   data_queue.qsize() #0
   ```

   

2. LifoQueue(): 스택 구조와 비슷하다

   ```python
   import queue
   data_queue = queue.LifoQueue()
   
   data_queue.put('coding')
   data_queue.put('2')
   
   data_queue.get() #2
   ```

   

3. PriorityQueue(): 데이터마다 우선순위를 넣어서, 우선순위가 높은 순으로 데이터 출력

   (숫자가 낮은 것이 우선순위가 높은 것)

   ```python
   import queue
   
   data_queue = queue.PriorityQueue()
   data_queue.put((10, 'coding')) #(우선순위, 데이터)
   data_queue.put((5, 1))
   data_queue.put((15, 'korea'))
   
   data.queue.get() #(5, 1)
   ```



## 어디에 큐가 많이 쓰이는가?

: **멀티 태스킹을 위한 프로세스 스케쥴링 방식을 구현**하기 위해 많이 사용됨(운영체제 참조)

> 한 가지 일을 하다가, 잠시 멈추고 또 다른 일을 하고, 또 멈추고 다른 일을 하다 보면 언젠가는 결국 모든 일이 마치게 된다. 이 동작이 엄청 빨라지면 한 번에 여러가지 일을 하는 것처럼 보이게 된다.
>
> **시분할 시스템**
>
> : 다중 사용자 지원을 위해 컴퓨터 응답 시간을 최소화하는 스케쥴링
>
> **멀티태스킹**
>
> : 단일 CPU에서 여러 응용 프로그램이 동시에 실행되는 것처럼 보이도록 하는 시스템
>
> **프로세스 스케쥴링**
>
> : 보다 효율적인 멀티태스킹이 되기 위해서 프로세스들을 적절히 분배하는 작업
>
> **멀티 프로세싱**
>
> : 하나의 응용프로그램이 여러 CPU를 사용하면서, 빠르게 실행시키는 시스템
>
> 
>
> 시분할 처리와 멀티 태스킹의 차이는
>
> **시분할 시스템**은 **다중 사용자 지원**을 위해 컴퓨터 응답 시간을 최소화하기 위한 목적
>
> **멀티 태스킹**은 **단일 CPU에서 여러 응용 프로그램이 동시에 실행되는 것처럼 보이도록** 하는 목적



## Enqueue, Dequeue 구현

```python
queue_list = list()

def enqueue(data):
    queue_list.append(data)
    
def dequeue():
    data = queue_list.pop(0)
    # data = queue_list[0]
    # del queue_list[0]
    return data
```

