# CMTime
> 시간을 시분초가 아닌 수의 개념으로 표현하는 구조체

```swift
let time = CMTime(value: 60, timescale: 1)
```

- 시간을 분자(value), 분모(timescale)로 표현하여 사용
- 위의 예는 60/1로 60초를 나타냄
- 양수무한대(+infinity), 음수무한대(-infinity), idenrinite(한정없음) 등의 nonnumeric values도 표현 가능
- 덧셈, 뺄셈, 곱샘 등의 연산 가능
    - func CMTimeAdd(CMTime, CMTime) -> CMTime
    - func CMTimeSubtract(CMTime, CMTime) -> CMTime 
    - func CMTimeMultiply(CMTime, multiplier: Int32) -> CMTime
- 미디어(영상) 등의 시간을 다양하게 표현할 수 있게 활용가능; Progress Bar, UISlider, Remaining Time 등

## AVPlayer 영상의 currentTime을 CMTime으로 SwiftUI에 display하기
```swift
extension CMTime {

    var roundedSeconds: TimeInterval {
        return seconds.rounded()
    }

    var hours: Int { return Int(roundedSeconds / 3600) }
    var minute: Int { return Int(roundedSeconds.truncatingRemainder(dividingBy: 3600) / 60) }
    var second: Int { return Int(roundedSeconds.truncatingRemainder(dividingBy: 60)) }

    var positionalTime: String {
        return hours > 0 ? String(format: "%d:$02d:%02d", hours, minute, second) : String(format: "%01d:%02d", minute, second) }
}

private func addPeriodicTimeObserver() {
    // NSEC_PER_SEC: The number of nanoseconds in one second
    let timeScale = CMTimeScale(NSEC_PER_SEC)

    let time = CMTime(seconds: 1.0, preferredTimescale: timeScale)
    // AVPlayer TimeObserver
    timeObserverToken = player.addPeriodicTimeObserver(forInterval: time, queue: .main) { progressTime in
        // @State var currentTime: String
        currentTime = progressTime.positionalTime
    }
}
```

## 출처
- [API Collection - CMTime](https://developer.apple.com/documentation/coremedia/cmtime-u58)