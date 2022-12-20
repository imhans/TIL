# NukeUI
- @available iOS 14+
- Lazy image loading for Apple platforms: SwiftUI, UIKit, AppKit
- iOS 15+ 의 asyncImage와 같음
- 상태관리 또한 가능
- LazyImage is built on top of FetchImage

## 사용
```swift
LazyImage(
  source: "https://example.com/image.jpeg", 
  resizingMode: .center
)
  .frame(height: 300)

LazyImage(url: $0) { state in
    if let image = state.image {
        image // Displays the loaded image
    } else if state.error != nil {
        Color.red // Indicates an error
    } else {
        Color.blue // Acts as a placeholder
    }
}
```
## 출처
- [NukeUI documentation](https://kean-docs.github.io/nukeui/documentation/nukeui/)