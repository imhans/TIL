# Protocol

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
## POP 장점
- inheritance 의 경우 참조추적을 위한 비용증가로 상대적으로 느리지만 pop는 가볍게 구현 가능. 즉 Value type의 상속기능 구현
- 프로토콜 상속, 합성 등을 통해 수평구조의 기능 확장 가능



## 출처
- [프로토콜이란](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-protocol-f18c82571dad)
- [프로토콜 지향 프로그래밍 POP](https://duwjdtn11.tistory.com/618)