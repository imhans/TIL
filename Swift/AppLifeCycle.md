# [SwiftUI] Observe App's LifeCycle 

- iOS 13+ 
- NotificationCenter(iOS 4+) and Combine(iOS 13+)


```swift
class NotificationCenterViewModel: ObservableObject {
    private var subscriptions = Set<AnyCancellable>()

    func getNotificationPublisher(for event: NotificationEvent) -> AnyPublisher<NotificationCenter.Publisher.Output, NotificationCenter.Publisher.Failure> {
        return NotificationCenter.default
            .publisher(for: event.name)
            .eraseToAnyPublisher()    
    }
    
    func getLifeCyclePublisher() -> AnyPublisher<NotificationCenter.Publisher.Output, NotificationCenter.Publisher.Failure> {
        return Publishers.MergeMany(
            getNotificationPublisher(for: .foreground),
            getNotificationPublisher(for: .background),
            getNotificationPublisher(for: .active)
        )
        .eraseToAnyPublisher()
    }
}

struct ContentView: View {
    @ObservedObject var viewModel = NotificationCenterViewModel()
    @State var currentState: NotificationCenterViewModel.NotificationEvent = .background

    var body: some View {
        VStack {
            Text("View")
                .onReceive(viewModel.getLifeCyclePublisher()) event in {
                    print("\(event.name)")
                }
        }
    }
}

extension NotificationCenterViewModel {
    enum NotificationEvent {
        case foreground
        case background
        case active
        case terminate

        var name: Notification.Name {
            switch self {
                case .foreground:
                    return UIApplication.willEnterForegroundNotification
                case .background:
                    return UIApplication.backgroundRefreshStatusDidChangeNotification
                case .active:
                    return UIApplication.didBecomeActiveNotification
                case .terminate:
                    return UIApplication.willTerminateNofitication
            }
        }
    }
}
```

## 출처
- [3 Ways to Observe App’s Lifecycle in SwiftUI](https://betterprogramming.pub/swiftui-3-ways-to-observe-apps-life-cycle-in-swiftui-e9be79e75fef)