# Delegate Pattern

>서로 분리된 class들 간의 소통을 도와주는 패턴
- 이벤트를 받는 객체: Delegator, 이벤트를 직접 처리하지 않고 다른 객체에게 위임함
- 이벤트를 직접 처리할 객체: Delegatee, 이벤트를 위임받아 대신 처리할 객체
- delegator - delegatee 간에 서로 강한 참조를 하기 때문에 Strong reference cycle 조심해야 함 (weak delegate 선언)

예시
- SwiftU의 View - (UIViewControllerRepresenter) - UIViewController 연결
- View의 @State property 업데이트 ->  UIViewControllerRepresenter에서 이벤트 발생
- weak var delegate으로 UIViewController에게 위임
```swift
// Delegate Protocol
protocol VideoRecordable {
    func startRecording()
    func stopRecording()
}

// Delegating Object (Delegator)
struct VideoView: UIViewControllerRepresentable {

    weak var delegate: VideoRecordable?

    func makeUIViewController(context: Context) -> ViewController {}

    func updateUIViewController(_ uiViewController: ViewController, context: Context) {}

    func didPressRecordButton(onRecording: Bool) {
        if !onRecording {
            delegate?.startRecording()
        } else {
            delegate?.stopRecording()
        }
    }
}

// Delegate Object (Delegatee)
extension ViewController: VideoRecordable {
    func startRecording() {
        print("Recording started")
    }
    func stopRecording() {
        print("Recording stopped")
    }
}
```

## 출처
- [The Delegate Pattern In Swift](https://dilloncodes.com/delegate-pattern-in-swift)