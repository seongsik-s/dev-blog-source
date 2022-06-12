---
layout: post
title: "[python] 동기/비동기 프로그래밍의 이해"
tags: [Python, Back-End, Synchronous, Asynchronous]
categories: [Python]
banner:
  image: /assets/images/banners/python.png
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 3.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
---

프론트엔드 개발자라면 JS의 async/await를 들어봤거나, 구현해본 경험이 있을 겁니다. 

python 3.5에서도 async/await 문법이 생기면서, 별도의 라이브러리 사용없이 비동기 프로그래밍이 가능해졌습니다.

최근에 FastAPI로 REST-API 서버를 구축하는 업무를 맡게되면서 서비스로직을 비동기로 처리해야 할 상황이 생겼습니다.

먼저 동기/비동기가 무엇인지, python에서 어떻게 사용하는지 알아보며 더 나아가 고수준의 API를 제공하는 concurrent.futures 모듈에 대해서 다뤄보도록 하겠습니다.

<br>

## **동기(synchronous) 프로그래밍**

***

동시에 일어난다는 뜻으로, 요청과 결과가 동시에 일어난다는 의미입니다. 요청이 들어오면 시간이 얼마나 걸리던 결과를 return을 해주어야 합니다.
동기 프로그래밍은 요청에 따른 결과를 반드시 return 해줘야할 때 사용합니다. 

기본적으로 python에서 사용하는 함수는 모두 동기프로그래밍입니다.

아래 request_func() 함수는 synchronous_programming() 함수를 호출하여 리턴값을 받게 되는데 이는 호출과 동시에 함수에서 리턴해주는 결과 'return value'를 꼭 받아야지만 아래 print(result)가 실행됩니다. 만약 synchronous_programming() 에서 리턴을 1시간 후에 한다면, 요청한 client는 1시간을 기다려야 됩니다.

```python
  def synchronous_programming():
    return 'return value'


  def request_func():
    result = synchronous_programming()
    print(result)
```

<br>

## **비동기(Asynchronous) 프로그래밍**

***

꼭 요청에 대한 결과를 기다려서 받아야 합니까? 그것을 해결해주는 것이 바로 비동기 프로그래밍입니다. 

동기 프로그램과 반대로, 요청과 결과가 동시에 일어나지 않아도 됩니다. 결과가 얼마나 걸리든 그 시간동안 다른 작업을 할 수 있으므로 자원을 효율적으로 사용할 수 있는 장점이 있습니다.

python에서 사용하는 방법은 def 문법앞에 async를 붙이면 비동기 함수로 작동하게 됩니다.

파이썬에서는 async가 붙은 함수를 코루틴(coroutine)라고도 부르는데, 동기 함수와 동일하게 호출하면 코루틴(coroutine) 객체로 리턴됩니다.

따라서, async def 함수는 async로 선언된 다른 비동기 함수에 await를 붙여 사용합니다. js에서 async/await 호출 방식과 비슷한 원리입니다.(안다는 가정하에 넘어가겠습니다.)


```python
import time
import asyncio

async def async_func1():
    print('== async_func1 started ==')
    await asyncio.sleep(5)
    print('== async_func1 end ==')

    return 'async_func1'

async def async_func2():
    print('== async_func2 started ==')
    await asyncio.sleep(3)
    print('== async_func2 end ==')

    return 'async_func2'

async def async_func3():
    print('== async_func3 started ==')
    await asyncio.sleep(1)
    print('== async_func3 end ==')

    return 'async_func3'

async def root():
    start = time.time()
    futures = [async_func1(), async_func2(), async_func3()]
    res1, res2, res3 = await asyncio.gather(*futures)
    
    end = time.time()
    print('비동기 처리 시간 : {}'.format(round(end-start)))
    return {"res1":res1, "res2":res2, "res3":res3}


result = asyncio.run(root())
print(result)
```

<br>

실행결과를 보면 실행순서는  
`async_func1() -> async_func2() -> async_func3()`

종료순서는 실행순서와 반대로 끝나는 것을 확인할 수 있다.  
`async_func3() -> async_func2() -> async_func1()`


```console
== async_func1 started ==
== async_func2 started ==
== async_func3 started ==
== async_func3 end ==
== async_func2 end ==
== async_func1 end ==
비동기 처리 시간 : 5
{'res1': 'async_func1', 'res2': 'async_func2', 'res3': 'async_func3'}
```

코드 순서를 자세히 살펴보면  
  1. 최초 root() 함수를 run(실행)
  2. root() -> 시작은 async_func1(), async_func2(), async_func3() 순서대로 비동기로 실행
  3. async_func3() 함수는 1초후 종료
  4. async_func2() 함수는 3초후 종료
  5. async_func1() 함수는 5초후 종료
  6. 모든 비동기 함수가 종료되는 시간은 5초

동기프로그래밍으로 작성하게 되면  

<kbd>async_func1() 5초 후 종료</kbd> -> <kbd>async_func2() 3초 후 종료</kbd> -> <kbd>async_func3() 1초 후 종료</kbd>

`= 총 소요시간 : 9초`

간단한 예제를 통해 파이썬에서 제공하는 async/await 문법을 사용해서 비동기프로그래밍을 테스트해봤습니다.

여기서 알 수 있듯이, 병렬적으로 자원을 사용하기 때문에 동기프로그래밍보다 효율적인 장점이 있습니다.
