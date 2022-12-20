# [SwiftUI] ScrollView Offset
- 앱 사이즈가 복잡해지고 커졌을 때 퍼포먼스 이슈 검토필요
- custom struct that can obtain the offset and some of its uses
- Key is moving the geometry reader above the ScrollView's content, and then hide it with a negative padding on the actual content
- The .frame(in: .named("scroll")) is required to get the frame relative to the ScrollView and obtain a 0 offset when at the top, without the height of the safe area. If you try using .frame(in: .global) you will get a non-zero value when the scroll view is at the top (20 or 44, depends on the device)

## Struct
```swift
struct ScrollViewOffset<Content: View>: View {
  let content: () -> Content

  init(@ViewBuilder content: @escaping () -> Content) {
    self.content = content
  }

  var body: some View {
    ScrollView {
      offsetReader
      content()
        .padding(.top, -8) // for some reason there offsetReader take some spaces above content()
    }
    .coordinateSpace(name: "frameLayer")
  }

  var offsetReader: some View {
    GeometryReader { proxy in
      Color.clear
        .preference(
          key: OffsetPreferenceKey.self,
          value: proxy.frame(in: .named("frameLayer")).minY
        )
    }
    .frame(height: 0)
  }
}
```

## Usage
```swift
@State private var scrollOffset: CGFloat = .zero

  var body: some View {
    ScrollViewOffset {
      LazyVStack {
        ForEach(0..<100) { index in
          Text("\(index)")
        }
      }
    } onOffsetChange: {
      scrollOffset = $0
    }
  }
  
  var opacity: Double {
    switch scrollOffset {
    case -100...0:
      return Double(-scrollOffset) / 100.0
    case ...(-100):
      return 1
    default:
      return 0
    }
  }
```

## 출처
- [SwiftUI ScrollView offset 101](https://www.fivestars.blog/articles/scrollview-offset/)