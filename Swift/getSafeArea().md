# [SwiftUI] 디바이스 배젤 유무에 따라 safeArea padding 주기

```swift
func getSafeArea() ->UIEdgeInsets  {
    return UIApplication.shared.windows.first?.safeAreaInsets ?? UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
}
```