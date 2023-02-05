# 네트워크 연결상태 확인하기 (NWPathMonitor)

### NWPathMonitor
> An observer that you use to monitor and react to network changes.

```swift
class NetworkStatus: ObservableObject {
    private let monitor: NWPathMonitor
    private let queue = DispatchQueue.global(qos: .background)
    @Published var isConnected: Bool = false
    
    init() {
       monitor = NWPathMonitor()
    }
    public func startMonitoring() {
        monitor.start(queue: queue)
        monitor.pathUpdateHandler = { [weak self] path in
            self?.isConnected = path.status == .satisfied
            
            if self?.isConnected == true {
                print("is connected")
            } else {
                print("is NOT connected")
            }
        }
    }
    public func stopMonitoring() {
        monitor.cancel()
    }
    public func checkConnection() -> Bool {
        if isConnected { return true }
        else { return false }
    }
}

```

## 출처 
- [NWPathMonitor](https://developer.apple.com/documentation/network/nwpathmonitor)