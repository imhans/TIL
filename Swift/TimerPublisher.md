# Timer Publisher

## Timer
A timer that fires after a certain time interval has elapsed, sending a specified message to a target object.

## Timer Publisher
A publisher that repeatedly emits the current date on a given interval.

- Because **Timer.TimerPublisher** conforms to the ConnectablePublisher protocol, it won’t produce elements until you explicitly connect to it. 
- Do this by either calling **connect()**, or using an **autoconnect()** operator to connect automatically when a subscriber attaches.


## Usage: Countdown Timer
```swift
class CountdownTimer: ObservableObject {
    @Published var timer = Timer.publsh(every: 1, on: .main, in: .common).autoConnect()

    func startTimer() {
        self.timer = Timer.publsh(every: 1, on: .main, in: .common).autoConnect()
    }
    func stopTimer() {
        self.timer.upstream.connect().cancel()
    }
}

struct CountdownView: View {
    @StateObject var timer: CountdownTimer = CountdownTimer()
    @State var timeRemaining: String = ""

    var body: some View {
        VStack {
            Text(timeRemaining)
                .onReceive(timer.timer) { _ in
                // update text here
                }
            Button(action: {
                timer.startTimer()
            }) {
                Text("Start Countdown")
            }
        }
        .onAppear {
            timer.stopTimer() // prevent auto-connecting
        }
        
    }

}
```
## 출처
- [Converting to a Timer Publisher](https://developer.apple.com/documentation/combine/replacing-foundation-timers-with-timer-publishers)