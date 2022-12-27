# StateObject Lifecycle 알아보기

- StateObject and ObservedObect points the same memory address
- StateObject in for-loop returns multiple instances of StateObject with different memory addresses
- ObservableObject class 를 StateObject property로 가지는 view 구조체는 remain 되어있으며, 다른 곳에서 같은 view 구조체가 redraw(?) 될 때! remain 되어있던 view 구조체의 ObservableObject class는 deinit되고 새로운 class가 init 된다..

>when the owner is the detail in a NavigationView
```swift
class ViewModel: ObservableObject {
	var objectId: String {
		ObjectIdentifier(self).debugDescription
	}

	init() {
		print("-> ViewModel init")
	}

	deinit {
		print("-> ViewModel deinit")
	}
}

struct MainView: View {
	var body: some View {
		NavigationView {
			List(0..<10, id: \.self) { index in
				NavigationLink {
					DetailView()
				} label: {
					Text("\(index)")
				}
			}
		}
	}
}

struct DetailView: View {
	@StateObject var viewModel = ViewModel()

	var body: some View {
		Text("Detail \(self.viewModel.objectId)")
	}
}
```
## 출처
- [Investigation of @StateObject lifecycle](https://daringsnowball.net/articles/stateobject-lifecycle/)