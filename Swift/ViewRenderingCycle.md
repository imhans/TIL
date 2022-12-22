# **[SwiftUI] ScrollView Offset 사용하다 깨달은 View Rendering Cycle**

## **배경**
- ScrollView의 스크롤 움직임에 따른 offset value를 이용하여 몇몇 View에 offset value가 변함에 따라 opacity 및 이미지 변경을 주는 작업 중 스크롤 퍼포먼스 이슈생김, 부드러운 스크롤링이 아닌 뚝뚝 끊기는 스크롤링
- 여러가지 시행착오 결과, offset value가 변동될 때마다(=스크롤링 할때마다) 특정 뷰만이 아닌 ScrollView를 포함하는 부모뷰 전체가 랜더링이 되고 있다는 가설이 세워짐
- 테스트 결과 예상이 맞았고 View Rendering Cycle 중 body computed property가 호출되는 방식을 바탕으로 스크롤 움직임 시 래깅 문제를 해결함

## **View Rendering Cycle**
<img src="https://user-images.githubusercontent.com/38341386/209132904-6ac6a409-8571-4bb3-9a62-37893d4dff1a.png" width="80%">

- 위의 그림에서 New State은 @State property의 update를 의미하고 External event는 Combine publisher를 의미(예를 들면, ObservableObject class의 @Published property 등)


> Note that only views that are connected to the state can provide custom equality implementation. 
> Views not connected to the state are always re-rendered.
- **쉽게 얘기하면, Updating phase에 일련의 절차를 거친 후 전체 View hierarchy가 속한 body computed property 자체를 re-render한다. 하지만 그 중 State property들은 업데이트가 없으면 re-render 되지 않는다.**
- State property가 업데이트 될 때마다 property를 소유하는 View의 body 안 모든 뷰가 re-rendered 되어왔다는 의미
- State property는 sourt of truth 일 뿐이며 view rendering과는 별도로 생각해야하는데 무의식적으로 State property만 업데이트 될 것이라 치부해왔음
- 그러나 offset property는 value가 소수점 단위로 바뀌는데 그때마다 전체 View가 re-rendered 되었다고 하니 처음의 래깅 문제가 이해가 되었음
- *그렇다면 state이 변화할 때 전체 View를 re-rendering 하고 싶지 않다면 어떻게 해야할까*

## **Add another View with a body property**
```swift
struct ViewRenderingCycle: View {
    @State private var counter: Int = 0
    @State private var hello: String = "Hello"
    
    @State private var startOffset: CGFloat = .zero
    @State private var scrollOffset: CGFloat = .zero // 0. New state updating
    
    @State private var blue: Color = Color.blue
    
    var body: some View {
        ZStack {
            ScrollViewReader { reader in
                ScrollView(.vertical, showsIndicators: false) {
                    HStack {
                        Text("HEADER")
                            .padding(.vertical, 20)
                    }
                    .frame(maxWidth: .infinity, alignment: .center)
                    .background(blue) // 1. ViewRenderingCycle View의 State -> re-rendering X
                    .opacity(-scrollOffset > 300 ? 0.5 : 1)
                    
                    VStack(spacing: 20) {
                        Counter(counter: counter)
                        Hello(hello: hello)
                    }
                    .padding(20)
                    .background(Color.random) // 2. ViewRenderingCycle View body -> re-rendering O
                    // offset tracking part
                    .overlay (
                        GeometryReader { proxy -> Color in
                            DispatchQueue.main.async {
                                if startOffset == 0 {
                                    self.startOffset = proxy.frame(in: .global).minY
                                }
                                let offset = proxy.frame(in: .global).minY
                                self.scrollOffset = offset - startOffset
                                
                                print(self.scrollOffset)
                            }
                            
                            return Color.clear
                        }
                            .frame(width: 0, height: 0)
                        ,alignment: .top
                    )
                    
                }
            }
        }
        .background(Color.random) // 3. ViewRenderingCycle View body -> re-rendering O
    }
}

extension ViewRenderingCycle {
    struct Counter: View {
        let counter: Int
        var body: some View {
            Text("\(counter)")
                .frame(width: 150, height: 30)
                .background(Color.random) // 4. Counter view의 body에 속하므로 re-rendering X
        }
    }
    struct Hello: View {
        let hello: String
        var body: some View {
            Text(hello)
                .frame(width: 150, height: 30)
                .background(Color.random) // 5. Hello view의 body에 속하므로 re-rendering X
        }
    }
}
```
- ViewRenderingCycle 뷰의 하위뷰를 정의할 때 body property를 가지는 struct로 함
- 위 예제 코드의 Counter View는 counter와 body를 개별적으로 가지는데 이들 값이 변경되지 않는 이상 ViewRenderingCycle View의 body가 re-rendering 된다고 해서 Counter의 body가 호출되지는 않음

## **결론**
- View 구성은 struct로 하여 re-rendering을 최소화 할 것
- View hierarchy 짤 때 덩어리 별로 분리할 것




### **Body Computed Property**
```swift
@available(iOS 13.0, macOS 10.15, tvOS 13.0, watchOS 6.0, *)
public protocol View {
  associatedtype Body : View
  
  @ViewBuilder var body: Self.Body { get }
}
```



### Easy way to track ScrollView Offset value
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

## **출처**
- [SwiftUI ScrollView offset 101](https://www.fivestars.blog/articles/scrollview-offset/)
- [SwiftUI View Lifecycle](https://www.vadimbulavin.com/swiftui-view-lifecycle/)
- [Why does SwiftUI use “some View” for its view type?](https://www.hackingwithswift.com/books/ios-swiftui/why-does-swiftui-use-some-view-for-its-view-type)