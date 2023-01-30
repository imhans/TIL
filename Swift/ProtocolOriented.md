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

## Collections of Protocol Types
프로토콜 타입은 배열 또는 딕셔너리와 같은 Collection의 타입으로 사용 가능

```swift
protocol MediaDataRepresentable {
    associatedtype Media
    var data: [Media] { get }
    var score: Int { get set }
}
protocol TextRepresentable {
    var textualDescription: String { get }
}
// PictureRepresenter
struct PictureRepresenter: MediaDataRepresentable {
    typealias Media = Color
    
    var score: Int = 0
    var data: [Media] {
        var vr = [Media]()
        vr.append(Color.red)
        return vr
    }
}
extension PictureRepresenter: TextRepresentable {
    var textualDescription: String {
        return "Array is \(data)"
    }
}
// VideoRepresenter
struct VideoRepresenter: MediaDataRepresentable {
    typealias Media = (Color, Color)
    var score: Int = 5
    var data: [Media] = [(Color.blue, Color.yellow)]
}

extension VideoRepresenter: TextRepresentable {
    var textualDescription: String {
        return "Array is \(data)"
    }
}
// Class
class MediaDataManager {
    var mediaRepresenters: [TextRepresentable] = []
    func getMediaRepresenters() {
        // mediaRepresenters의 타입은 TextRepresentable 이지만 appended된 원소는 PictureRepresenter 또는 VideoRepresenter
        mediaRepresenters.append(PictureRepresenter())
        mediaRepresenters.append(VideoRepresenter())
    }
    func printTextDescriptions() {
        for mediaRepresenter in mediaRepresenters {
            // TextRepresentable의 textualDescription에 access
            print("\(mediaRepresenter.textualDescription)")
        }
    }
}
```
}
- mediaRepresenters 배열의 element는 PictureRepresenter 또는 VideoRepresenter
- 그렇지만 동시에 TextRepresentable 타입이며, TextRepresentable은 textualDescription 프로퍼티를 가지기 때문에 루프를 통해 안전하게 mediaRepresenter.textualDescription에 접근가능

## 출처
- [프로토콜이란](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-protocol-f18c82571dad)
- [프로토콜 지향 프로그래밍 POP](https://duwjdtn11.tistory.com/618)
- [Swift – 프로토콜 지향 프로그래밍](https://blog.yagom.net/531/)
- [Handling SwiftUI Views Using Protocol-Oriented Programming](https://betterprogramming.pub/handle-swiftui-views-using-protocol-oriented-programming-d07741f58f4b)
- [Getting Started with SwiftUI and Combine Using MVVM and Protocols for iOS](https://medium.com/swlh/getting-started-with-swiftui-and-combine-using-mvvm-and-protocols-for-ios-d8c37731a1d9)
- [Collections of Protocol Types](https://bbiguduk.gitbook.io/swift/language-guide-1/protocols#collections-of-protocol-types)