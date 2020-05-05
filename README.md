
## 동기 blocking
하나의 작업이 끝나면 다른 작업을 시작한다.
```python
def action(i):
    time.sleep(1)
    print("I'm Action "+str(i))
for i in range(3):
    action(i)
```

## 동기 non-blocking
하나의 작업이 끝나기전에 다른 작업을 시작할 수 있고 다른 작업이 끝났는지 확인하며 현재 작업을 진행한다.  
thread는 비동기로 실행되었고 concurrency로 실행되었지만
nonBlockingAction 3개가 작업이 끌났는지 확인하기 때문에
동기 nonblocking으로 볼 수 있다.  
확실한 예시는 select나 polling 등이 있다
```python
state = {"complete":0}
def nonBlockingAction(i, state):
    time.sleep(1)
    print("I'm Action "+str(i))
    state["complete"]+=1
def threadingProcess(i, state):
    nonBlockingAction(i,state)
for i in range(3):
    threading.Thread(target=threadingProcess, args=(i, state)).start()
otherWork = False
while state["complete"]<3 :
    if not otherWork:
        print("other work")
        otherWork = True
```

## 비동기 non-blocking
하나의 작업이 끝나기전에 다른 작업을 시작할 수 있고 다른작업이 끝날때 까지 확인하지 않는다.
```python
async def asyncAction(i):
    await asyncio.sleep(1)
    print("I'm Action "+str(i))
process = [asyncAction(0),asyncAction(1),asyncAction(2)]
async def asyncNonBlocking():
    await asyncio.gather(*process)
asyncio.run(asyncNonBlocking())
```  
## 비동기 프로그래밍
프로그램의 주 실행흐름을 멈추어서 기다리는 부분 없이 다음작업을 수행할 수 있도록
프로그래밍 하는 것을 말한다.  
함수를 값처럼 사용하는 방식이나 콜백 함수를 이용하는 것도
비동기라고 볼 수 있다.  
비동기인지 동기인지는 blocking의 여부와 concurrency인지는 다른 개념이다.

 
