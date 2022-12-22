# weak self 왜 사용하는가

## 왜
- Retain Cycle(순환참조)로 인한 Memory Leak(누수)를 방지하기 위함
- Reference type 에서는 Retain Count를 관리해야 함
- weak reference는 optional이며, runtime에도 value를 nil로 만들 수 있음
- self를 참조하는 상황에서 self에 대한 순환참조가 발생하지 않도록 함

## 언제 
- escaping closure 안에서 지연할당의 가능성이 있을 경우
- non escaping closure 안에서는 strong reference cycle을 유발하지 않으므로 weak self 사용할 필요없음
- asyncAftr delay 주는 때와 같은 상황에선 주의해서 사용

## Optional binding
- 클로저 내부에서 self를 임시적으로 strong reference로 가지는 방법
- guard let self = self else { return } 사용하여 self를 unwrapping


## 출처
- [클로저에서의 weak self 에 대해 알아보자](https://bongcando.tistory.com/20)
- [Weak self 무조건 사용하는게 맞는걸까](https://noah0316.github.io/Swift/2022-04-08-[weak-self]-%EB%AC%B4%EC%A1%B0%EA%B1%B4-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B2%8C-%EB%A7%9E%EB%8A%94%EA%B1%B8%EA%B9%8C/)
