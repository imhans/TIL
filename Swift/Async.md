# Async/sync

- Swift 5.5
- 비동기 사용 이유 = 시간절약
- async vs sync: 작업 보내는 시점에서 기다릴지 말지에 대해 다루는 것
- serial vs concurrent: Queue(대기열)로 보낸 작업들을 여러  개의 쓰레드로 보낼지 한개의 쓰레드로 보낼지에 대해 다루는 것

## 비동기 작업 시 completionHandler의 문제점
- completion 미호출의 가능성 -> 오류처리 힘들어짐
- 난해한 들여쓰기와 클로저들
- 콜백지옥

## Combine에서 async/await로 변경
- 공식문서에 의하면, async/await 방식이 completion closer와 Combine의 필요성을 없앤다함
- converting your asynchronous call points to Combine improves your code’s consistency and readability.
- AnyPublisher를 async throws 와 async 메소드인 data(from:)로 대체
- 각 단계 return 수정 
- sink로 값을 받아오던 것과 다르게 Task로 받아오는 작업 수행
```swift
// Combine
func fetchImageWithCombine2() {
    dataService.downloadWithCombine()
        .receive(on: DispatchQueue.main)
        .sink { completion in
            switch completion {
            case .failure(let error):
                print(error.localizedDescription)
                break
            case .finished:
                break
            }
        } receiveValue: { [weak self] image in
            guard let self = self else { return }
            self.image = image
        }
        .store(in: &cancellables)
}

// Async/await
func fetchImageWithAsyncTask() {
        Task {
            await fetchImageWithAsync()
        }
    }
    
func fetchImageWithAsync() async {
    do {
        guard let image = try await dataService.downloadWithAsync() else {
            return
        }
        await MainActor.run {
            self.image = image
        }
    } catch {
        print(error.localizedDescription)
    }
}
```
- UI update 시 [weak self]로 강한 참조 사이클 피함
- async 데이터 리턴 시 에러 throw가 가능하기 때문에 do-catch 필수
- image 업데이트 할 때 MainActor 클래스 내부에서 업데이트 진행

## 출처
- [Using Combine for Your App’s Asynchronous Code](https://developer.apple.com/documentation/combine/using-combine-for-your-app-s-asynchronous-code)
- [Combine에서 async/await로 변경하기](https://www.hohyeonmoon.com/blog/combine-to-async-await/)
- [Async/Await, @escaping, Combine](https://velog.io/@j_aion/SwiftUI-AsyncAwait-escaping-Combine)