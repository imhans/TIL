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
class TaskViewModel: ObservableObject {
    @Published var image: UIImage? = nil
    
    func fetchImage() async {
        do {
            guard let url = URL(string: "https://picsum.photos/1000") else { return }
            if #available(iOS 15.0, *) {
                let (data, _) = try await URLSession.shared.data(from: url, delegate: nil)
                self.image = UIImage(data: data)
            } else {
                // Fallback on earlier versions
            }
        } catch {
            print(error.localizedDescription)
        }
    }
}
struct TaskView: View {
    @StateObject private var taskVM = TaskViewModel()
    
    var body: some View {
        VStack {
            if let image = taskVM.image {
                Image(uiImage: image)
                    .resizable()
                    .scaledToFit()
                    .frame(width: 200, height: 200)
            }
        }
        .onAppear(perform: {
            Task {
                await taskVM.fetchImage()
            }
        })
    }
}
```
- UI update 시 [weak self]로 강한 참조 사이클 피함
- async 데이터 리턴 시 에러 throw가 가능하기 때문에 do-catch 필수
- image 업데이트 할 때 MainActor 클래스 내부에서 업데이트 진행

## Task
A unit of asynchronous work.
- Priority 부여 가능
- async 코드는 concurrnet context에서만 실행 가능하기 때문에 다른 async func 혹은 Task{ await ...() } 에서 사용 가능
- Task는 비동기함수(await)를 사용 가능한 플랫폼
- await 마킹은 potential suspension point 일 수 있다는 점 참고, 스레드 제어권을 포기하는 시점; 양보(yielding)
- system의 다른 thread에서 중단된 함수를 픽업하여 resume하는 느낌


## 출처
- [Using Combine for Your App’s Asynchronous Code](https://developer.apple.com/documentation/combine/using-combine-for-your-app-s-asynchronous-code)
- [Combine에서 async/await로 변경하기](https://www.hohyeonmoon.com/blog/combine-to-async-await/)
- [Async/Await, @escaping, Combine](https://velog.io/@j_aion/SwiftUI-AsyncAwait-escaping-Combine)
- [Task | Apple Developer Documentation](https://developer.apple.com/documentation/swift/task)