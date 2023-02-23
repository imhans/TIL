# Property Wrapper

> A property wrapper is essentially a type that wraps a given value in order to attach additional logic to it

- 컴파일러에게 @propertyWrapper가 붙은 타입(class, struct, or enum)은 특별한 로직을 지니고 있음을 알려줌
- @propertyWrapper 타입은 non-static property인 wrappedValue를 포함해야 함
- 이 wrappedValue 프로퍼티에 반복되는 로직을 연결하는 것이 핵심 (예를 들어 UserDefaults)
    - UserDefaults: An interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app.
- Boilerplate code의 제거와 reuseable code 증가에 용이
- @Published, @Binding, @State, @ObservedObject 등이 모두 property wrapper에 해당


## UserDefaults with @propertyWrapper
```swift
@propertyWrapper
struct Storage<T> {
    let key: String
    let defaultValue: T
    let storage: UserDefaults

    var wrappedValue: T {
        get {
            self.storage.object(forKey: self.key) as? T ?? self.defaultValue 
        }
        set {
            self.storage.set(newValue, forKey: self.key)
        }
    }

    init(key: String, defaultValue: T, storage: UserDefaults = .standard) {
        self.key = key
        self.defaultValue = defaultValue
        self.storage = storage
    }
}

class UserData {
    @Storage(key: "auto_signIn_key", defaultValue: false)
    static var autoSignInEnable: Bool
}
```


- A default object must be a property list—that is, an instance of NSData, NSString, NSNumber, NSDate, NSArray, or NSDictionary.
- If you want to store any other type of object, you should typically archive it to create an instance of NSData.

```swift
@propertyWrapper
struct StorageObject<T> {
    let key: String
    let defaultValue: T
    let storage: UserDefaults

    var wrappedValue: T {
        get {
            if let data = storage.data(forKey: key) as? Data {
                return JsonDecoder().decode(defaultValue.self, from: data)
            }
            return defaultValue
        }
        set {
            storage.set(JsonDecoder().encode(newValue), forKey: key)
        }
    }

    init(key: String, defaultValue: T, storage: UserDefaults = .standard) {
        self.key = key
        self.defaultValue = defaultValue
        self.storage = storage
    }
}

class UserData {
    @StorageObject(key: "persons_key", defaultValue: [])
    static var persons: [Person]
}
```

## 출처
- [Property wrappers in Swift](https://www.swiftbysundell.com/articles/property-wrappers-in-swift/)
- [WWDC2019](https://developer.apple.com/videos/play/wwdc2019/402/)
- [UserDefaults](https://developer.apple.com/documentation/foundation/userdefaults)