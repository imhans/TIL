# Protocol-Oriented Programming 
> At WWDC 2015, Apple announced that Swift is the world’s first Protocol-Oriented Programming (POP) language.

Struct는 Value Type
- Thread-safe, faster to initialize, and take less memory comparing to reference type(class)

OOP의 핵심은 상속(Inheritance)
- superclass에 종속적임
- subclass는 불필요한 변수와 함수를 모두 가져와야 함

## POP(Protocol-Oriented Programming)
- Value type의 Inheritance
- 필수 property와 method 정의
- protocol extensio에서는 computed property만 구현가능
- 다중상속과 수평적 확장 -> 필요한 API만 사용해 유연하게 구성가능
- where Self와 같은 Generics 활용 가능
    - (추후 부연설명 추가)



## 출처
- [Swift – 프로토콜 지향 프로그래밍](https://blog.yagom.net/531/)
- [Handling SwiftUI Views Using Protocol-Oriented Programming](https://betterprogramming.pub/handle-swiftui-views-using-protocol-oriented-programming-d07741f58f4b)
- [Getting Started with SwiftUI and Combine Using MVVM and Protocols for iOS](https://medium.com/swlh/getting-started-with-swiftui-and-combine-using-mvvm-and-protocols-for-ios-d8c37731a1d9)
