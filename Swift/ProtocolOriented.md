# Swift Protocol 기본 개념

- 특정 역할을 하기 위한 메소드, 프로퍼티, 기타 요구사항 등의 청사진
- 프로토콜의 요구사항을 충족시키는 모든 타입은 해당 프로토콜을 conform 한다고 함
- extension: 기존 타입의 기능을 확장하도록 도와주는 기능으로 수평적 확장의 개념, 프로토콜 초기 구현에 활용

## 프로토콜 사용
- 구조체, 클래스, 열거형 등에 사용
- 하나의 타입으로 사용되기 때문에 아래와 같이 타입 사용이 허용되는 모든 곳에 사용 가능
    - 함수, 메소드, 이니셜라이저의 파라미터 혹은 리턴 타입
    - 상수, 변수, 프로퍼티의 타입
    - 배열, 딕셔너리의 원소 타입


## 프로토콜 초기 구현 (Protocol Default Implementation aka POP)
- 프로토콜과 익스텐션의 결합
- 익스텐션을 통해 프로토콜의 요구기능 구현
- 기능 override 가능
``` swift
// 프로토콜 정의
protocol Person {
    var name: String { get }
    var age: Int { get }

    func getName() -> String
    func getAge() -> Int 
}

// 프로토콜 초기구현
// extension에서 초기구현하므로 conform 시 다시 구현할 필요 없음
extension Person {
    func getName() -> String {
        return self.name
    }
    func getAge() -> Int { 
        return self.age
    }
}

struct student: Person {
    var name: String
    var age: Int
}
```


# Protocol-Oriented Programming 
> At WWDC 2015, Apple announced that Swift is the world’s first Protocol-Oriented Programming (POP) language.

Struct는 Value Type
- Thread-safe, faster to initialize, and take less memory comparing to reference type(class)

OOP의 상속(Inheritance)
- superclass에 종속적
- subclass는 불필요한 변수와 함수를 모두 가져와야 함

## POP(Protocol-Oriented Programming)
- Value type의 Inheritance
- 필수 property와 method 정의
- protocol extensio에서는 computed property만 구현가능
- 다중상속과 수평적 확장 -> 필요한 API만 사용해 유연하게 구성가능
- where Self와 같은 Generic type 활용 가능
    - (부연설명 추가)

## Presentation Layer층에 MVVM + POP로 간단한 예시 만들기
- 


## 출처
- [프로토콜이란](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-protocol-f18c82571dad)
- [프로토콜 지향 프로그래밍 POP](https://duwjdtn11.tistory.com/618)
- [Swift – 프로토콜 지향 프로그래밍](https://blog.yagom.net/531/)
- [Handling SwiftUI Views Using Protocol-Oriented Programming](https://betterprogramming.pub/handle-swiftui-views-using-protocol-oriented-programming-d07741f58f4b)
- [Getting Started with SwiftUI and Combine Using MVVM and Protocols for iOS](https://medium.com/swlh/getting-started-with-swiftui-and-combine-using-mvvm-and-protocols-for-ios-d8c37731a1d9)