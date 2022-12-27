# [SwiftUI] body: some View (feat. View protocol)

- View protocol을 conform하는 모든 View들은 각각 다른 구조체
- some 키워드를 통해 만들어진 타입은 Opaque Types
- 즉, some View는 특정 타입을 지칭하는게 아닌 'View protocol을 충족하는 어떠한 타입'을 의미


View protocol 내부구조
```swift
@available(iOS 13.0, macOS 10.15, tvOS 13.0, watchOS 6.0, *)
public protocol View {

    associatedtype Body : View
    
    @ViewBuilder var body: Self.Body { get }
}
```
- body는 View protocol을 conform하는 Body 타입의 변수


>The type of view representing the body of this view.
>
>When you create a custom view,
>Swift infers this type from your implementation of the required body property.

## 출처
- [[SwiftUI] View protocol과 body 변수에 대한 고찰 (some 키워드를 사용하는 이유)](https://hasensprung.tistory.com/m/129)