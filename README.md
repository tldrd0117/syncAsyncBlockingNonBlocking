
## 동기 blocking
#### 하나의 작업이 끝나면 다른 작업을 시작한다.
```python
def action(i):
    time.sleep(1)
    print("I'm Action "+str(i))
process = [action,action,action]
for i in range(len(process)):
    process[i](i)
```

## 동기 non-blocking
#### 하나의 작업이 끝나기전에 다른 작업을 시작할 수 있고 다른 작업이 끝났는지 확인하며 현재 작업을 진행한다.
```python
state = {"complete":0}
def nonBlockingAction(i, state):
    time.sleep(1)
    print("I'm Action "+str(i))
    state["complete"]+=1
process = [nonBlockingAction,nonBlockingAction,nonBlockingAction]
process = list(map(lambda i: threading.Thread(target=process[i], args=(i,state)), range(len(process))))
for i in range(len(process)):
    process[i].start()
otherWork = False
while state["complete"]<3 :
    if not otherWork:
        print("other work")
        otherWork = True
```

## 비동기 non-blocking
#### 하나의 작업이 끝나기전에 다른 작업을 시작할 수 있고 다른작업이 끝날때 까지 확인하지 않는다.
```python
async def asyncAction(i):
    await asyncio.sleep(1)
    print("I'm Action "+str(i))
process = [asyncAction(0),asyncAction(1),asyncAction(2)]
async def asyncNonBlocking():
    await asyncio.gather(*process)
asyncio.run(asyncNonBlocking())
```

