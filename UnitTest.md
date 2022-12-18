# Unit Test
- 하나의 함수, 메서드를 기준으로 진행되는 가장 작은 단위 테스트
- Behavior driven development 포맷
    - given 주어진 상황
    - when 코드 실행 
    - then 결과 확인
- 테스트 케이스 작성
- 테스트 함수명은 test~로 시작
- @testable import 'target'
- SUT(System Under Test) 테스트 할 타입

```swift
func addNumbers(of numbers: [Int]) -> Int {
    return numbers.reduce(0, +)
}

class StrangeCalculatorTests: XCTestCase {
    var sut: StrangeCalculator! // 1

    override func setUpWithError() throws { // 2
        try super.setUpWithError()
        sut = StrangeCalculator()
    }

    override func tearDownWithError() throws { // 3
        try super.tearDownWithError()
        sut = nil
    }

    // MARK: - addNumbers
    func test_addNumbers호출시_3_7_23을전달했을때_33을반환하는지() {
        // given
        let input = [3, 7, 23]

        // when
        let result = sut.addNumbers(of: input)

        // then
        XCTAssertEqual(result, 33)
    }
}
```

## Code Coverage
- 앱 코드에서의 테스트 진행 정도를 알수있는 툴
- Product - Scheme - Edit Scheme - Test - Options - check Code Coverage

## 비동기 함수 테스트
- expectation(description:): 기대수행 정의
- fulfill(): 동작수행했음을 고지
- wait(for:timeout:): 
```swift
func test_makeRandomValue호출시_randomValue를_잘설정해주는지() {
    // given
    let promise = expectation(description: "It makes random value") // expectation
    sut.randomValue = 50 // 기본값이 0~30에 포함되면 무조건 테스트에 통과하므로 범위에서 벗어난 값을 할당

    // when
    sut.makeRandomValue {
        // then
        XCTAssertGreaterThanOrEqual(self.sut.randomValue, 0)
        XCTAssertLessThanOrEqual(self.sut.randomValue, 30)
        promise.fulfill() // fulfill
    }

    wait(for: [promise], timeout: 10) // wait
}
```

## 의존성 주입, 왜?
- 객체간의 결합도 낮추기 위함, 또 리팩토링 쉬워짐

## Test Double
- 테스트 진행이 어려운 경우 이를 대신해 테스트 진행할수 있도록 만들어주는 객체 like stuntman
- Dummy, Spy, Stub, Mock, FakeObject

## Unit Test 장점
- 즉각적 피드백
- Regression Testing 비용을 줄여줌
- 디버깅 시간의 감소
- 자신감 넘치는 배포로 이어짐

## 출처
- [Unit Test 작성하기](https://yagom.net/courses/unit-test-%ec%9e%91%ec%84%b1%ed%95%98%ea%b8%b0/)
- [Test Double을 알아보자](https://tecoble.techcourse.co.kr/post/2020-09-19-what-is-test-double/)
- [Intro to Swift Unit Testing](https://www.youtube.com/watch?v=ZVSy9oj-7O0)