# Protocol Oriented Network Layer

 >A protocol oriented network, testable network layer for applications

### 구조
- Request
- API Call
    - API Handler; Request Handler, Response Handler
    - API Loader
- Response

### 디렉토리
```
Networking
├─ APIHandler.swift
├─ APILoader.swift
├─ APIEnvironment.swift
└─ APIPath.swift
Services
└─ LoginAPI.swift
```


### APIHandler.swift
```swift
enum HTTPMethod: String {
    case get = "GET"
    case post = "POST"
    case put = "PUT"
    case patch = "PATCH"
    case delete = "DELETE"
}

protocol RequestHandler {
    associatedType RequestDataType
    func makeRequest(from data: RequestDataType) -> URLRequest?
}
protocol ResponseHandler {
    associatedType ResponseDataType
    func parseResponse(data: Data, response: HTTPURLResponse) throws -> ResponseDataType
}
typealias APIHandler = RequestHandler & ResponseHandler

extension RequestHandler {
    func set(parameters:[String:Any], urlRequest: inout URLRequest) {}
    func setQueryParams(parameters:[String:Any], url: URL) -> URL {} 
    func setDefaultHeaders(request: inout URLRequest) {} 
}
extension ResponseHandler {
    // Generic function
    func defaultParseResponse<T: Codable>(data: Data, response: HTTPURLResponse) throws -> T {
        let jsonDecoder = JSONDecoder()
        do {
            let body = try jsonDecoder.decode(T.self, from: data)
            if response.statusCode == 200 {
                return body
            } else {
                throw ServiceError(httpStatus: response.statusCode, message: "Unknown Error")
            }
        } catch {
            throw ServiceError(httpStatus: response.statusCode, message: error.localizedDescription)
        }
    }
}
struct ServiceError: Error, Codable {
    let httpStatus: Int
    let message: String 
}
```

### APIEnvironment.swift
```swift
enum Environment {
    case development
    case staging
    case production

    var baseUrl: String {
        "https://\(subDomain()).\(domain())"
    }
    var domain: String {
        case development: "abc.com"
        case staging: "bcd.com"
        case production: "cde.com"
    }
    var subDomain: String {
        switch self {
            case .development, .staging, .production:
            "api"
        }
    }
    func route() -> String {
        "/api/v1"
    }
}
```

### APILoader.swift
```swift
class APILoader<T: APIHandler> {
    let apiRequest: T
    let urlSession: URLSession

    init(apiRequest: T, urlSession: urlSession = .shared) {
        self.apiRequest = apiRequest
        self.urlSession = urlSession
    }

    func loadAPIRequest(requestData: T.RequestDataType, completionHandler: @escaping (T.ResponseDataType?, Error?) -> ()) {
        if let urlRequest = apiRequest.makeRequest(from: requestData) {
            urlSession.dataTask(with: urlRequest) { (data, response, error) in 
                guard let data = data, let httpResponse = response as? HTTPURLResponse else {
                    return completionHandler(nil, error)
                }
                do {
                    let parsedResponse = try self.apiRequest.defaultParseResponse(data: data, response: httpResponse)
                    completionHandler(parsedResponse)
                } catch {
                    completionHandler(nil, error)
                }
            }.resume()
        }
    }
}
```

### APIPath.swift
```swift
#if DEBUG
let environment = Environment.development
#else
let environment = Environment.production
#endif

let baseURL = environment.baseURL

struct Path {
    var login: String { return "\(baseURL)/loginapi" }
}
```

### LoginAPI.swift
```swift
struct LoginAPI: APIHandler {}
```

### 출처
- [Protocol oriented Network layer with MVVM Swift | Clean Architecture](https://www.youtube.com/watch?v=RDQqd2_AyA0)