# Generics and Associated Types
*추상화를 통해 중복된 코드를 줄이고 구조체, 함수, 뷰들의 재사용성을 높일 수 있을지에 대한 고민에서 이어짐*

### Generics
- 타입에 유연하게 대처 가능
- 제네릭으로 구현한 기능은 재사용에 용이하며, 중복된 표현을 줄여주어 깔끔한 코드 작성이 가능
- 쉬운 예로는 Array에 다양한 타입을 갖도록 할 수 있는 것이 제네릭을 사용했기 때문

### 제네릭을 사용한 구조체 타입
```swift
struct Stack<T> {
    var items = [T]()
    mutating func push(_ items: T) {
        items.append(item)
    }
    mutating func pop() -> T {
        return items.removeLast()
    }
}

// 구조체에 타입 명시하여 유연하게 사용
var anyStack = Stack<Any>()
anyStack.push(114.1)    // [114.1]
anyStack.push("Hello")  // [114.1, "Hello"]
```

### 타입 제약(Type Constraints)
- 특정 class 혹은 protocol을 사용을 준수하도록 타입제약을 줄 수 있음
```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

### Associated Types 
- 프로토콜을 정의할 때 용이하게 사용됨
- 프로토콜이 conformed 되기 전 placeholder name의 역할
- associatedtype Item: Equatable 처럼 타입제약 가능함
```swift
protocol Container {
    associatedtype Item   // Placeholder
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
struct IntStack: Container {
    typealias Item = Int   // 추상유형인 Item을 구체유형 Int로 바꿔줌
    mutating func append(_ item: Item) {
        self.push(item)
    }
    var count: Int { return items.count }
    subscript(i: Int) -> Item {
        return items[i]
    }
}
```

### Generic Where Clauses
- where 키워드로 사용
### Extensions with a Generic Where Clause
- 
### Contextual Where Clauses
- 
### Associated Types with a Generic Where Clause
- 


## 출처
- [Generics - the swift programming language](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)