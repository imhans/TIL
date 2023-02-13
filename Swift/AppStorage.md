# AppStorage

> A property wrapper type that reflects a value from UserDefaults and invalidates a view on a change in value in that user default.

```swift
Declaration
@frozen @propertyWrapper struct AppStorage<Value>
```
- UserDefaults 프로퍼티를 View에서 State처럼 쓰고 싶을 때 선언
- @AppStorage("KEY") var name: Type = Value
- @Binding 사용하여 childView에 전달 가능
- UserDefaults 자체는 NSObject 클래스이기 때문에(state 프로퍼티가 아님) set(_:forKey:)을 통해 value를 mutate하더라도 View의 랜더링을 유발하지 않음


## 출처
- [AppStorage](https://developer.apple.com/documentation/swiftui/appstorage)