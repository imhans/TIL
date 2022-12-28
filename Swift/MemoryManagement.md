# Memory Management and ARC
Memory Management

메모리 관리란 프로그램의 메모리를 제어하는 프로세스
- Stack: 모든 local variables
- Global Data: static variables, constants, and type metadata
- Heap: all dynamically allocated data, everything that has a lifetime

ARC is Swift ownership system
- ARC가 Reference Counting을 컴파일(build) 시 implemented되어 자동으로 관리
- The swiftc compiler inserts calls to *release* and *retain* wherever appropriate.
    - retain: reference count 증가
    - release: reference count 감소
- 메모리 누수(memory leak)가 발생한다는 것은 strong reference counting이 0이 되지 못한다는 의미
- Retain Cycle에 유의해야 함
    - A retain cycle occurs when two objects have strong references to each other and neither object is ever released from memory. By declaring a property as weak, the reference count of the object will not be increased, thus avoiding the retain cycle
- 가비지 컬렉터는 컴파일이 끝난 후 메모리를 동적으로 관리한다는 점에서 상이함

Swift object lifecycle undergoes 5 phases
<img src="https://user-images.githubusercontent.com/38341386/209819869-e77c930c-a439-4876-8762-f082260d1410.png">


구조체 내부에 래퍼런스 타입이 존재하는 경우
- ?

## 출처
- [Advanced iOS Memory Management with Swift: ARC, Strong, Weak and Unowned Explained](https://www.vadimbulavin.com/swift-memory-management-arc-strong-weak-and-unowned/)
- [메모리 참조, ARC, Strong Reference Cycle 이해하기](https://dev-dmsgk.tistory.com/22)