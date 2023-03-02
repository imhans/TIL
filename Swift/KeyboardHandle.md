# [SwfitUI] Keyboard 다루기

- iOS 14 이하
- iOS 15+ 는 FocusState 이용 가능
- TextField 등에 의해 키보드가 나타날 시 UX에 불편을 줄 수 있음

## KeyboardHandler ObservableObject 
- UIResponder.keyboardWillShowNotification와 UIResponder.keyboardWillHideNotification 프로퍼티를 subscribe
- keyboard appear 시 keyboard height만큼 bottom padding 추가

```swift
import Combine
import SwiftUI

final class KeyboardHandler: ObservableObject {
    @Published private(set) var keyboardHeight: CGFloat = 0
    
    private var cancellable: AnyCancellable?
    
    private let keyboardWillShow = NotificationCenter.default.publisher(for: UIResponder.keyboardWillShowNotification)
        .compactMap { ($0.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? CGRect)?.height }
    
    private let keyboardWillHide = NotificationCenter.default.publisher(for: UIResponder.keyboardWillHideNotification)
        .map { _ in CGFloat.zero }
    
    init() {
        cancellable = Publishers.Merge(keyboardWillShow, keyboardWillHide).subscribe(on: DispatchQueue.main)
            .assign(to: \.self.keyboardHeight, on: self)
    }
        
}

@StateObject private var keyboardHandler = KeyboardHandler()

ScrollView(.vertical){
    /// ...
}
.padding(.bottom, keyboardHandler.keyboardHeight)
.animation(.default)
```


## View 탭으로 키보드 off
- keyboardType 설정 시 numberPad 같은 case는 확인 버튼이 없음
- 아래 함수를 최상위 View .onTapGuesture 등으로 해결
```swift
extension View {
    func endTextEditing() {
        UIApplication.shared.sendAction(#selector(UIResponder.resignFirstResponder), to: nil, from: nil, for: nil)
    }
}
```

## 출처 
- [SwiftUI TextField에 키보드 포커싱](https://cartoonpoet.github.io/swiftui/2021/07/13/%ED%82%A4%EB%B3%B4%EB%93%9C%ED%8F%AC%EC%BB%A4%EC%8A%A4.html)