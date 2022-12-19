# Grand Central Dispatch(GCD)
- 더욱 복잡한 작업은 Operation 사용; GCD 기반하여 데이터와 기능을 캡슐화한 객체
- 동시성 프로그래밍: 작업(Task)을 여러 쓰레드에서 동시에 분산처리하기 위함
- 쓰레드 관리는 iOS가 알아서 해줌
- Thread: the lowest level of execution

DispatchQueu.global().async: 클로저 안의 작업을 글로벌큐에 비동기적으로 보내. 기다리진 않을거야
```swift
let queue = DispatchQueue.global()
queue.async {
    
}
```


## 출처
- [inflearn](https://www.inflearn.com/course/iOS-Concurrency-GCD-Operation)