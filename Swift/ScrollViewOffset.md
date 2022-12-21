# [SwiftUI] ScrollView Offset
- scrollViewOffset property를 이용하여 view update 시 rendering issue 발생..

## Usage
```swift
@State private var scrollViewOffset: CGFloat = .zero
@State private var startOffset: CGFloat = .zero

  var body: some View {
    ScrollViewReader { proxy in
      ScrollView { 
        VStack {
          contentView
        }
        .padding()
        .overlay(
          GeometryReader { geometry in
            DispatchQueue.main.asyn {
              if startOffset == 0 {
                self.startOffset = geometry.frame(in: .global).minY
              }
              let offset = geometry.frame(in: .global).minY
              self.scrollViewOffset = offset - startOffset

              print(self.scrollViewOffset)
            }
          }
          .frame(width: 0, height: 0)
          ,alignment: .top
        )
      }
    }
```

## 출처
- [SwiftUI ScrollView offset 101](https://www.fivestars.blog/articles/scrollview-offset/)