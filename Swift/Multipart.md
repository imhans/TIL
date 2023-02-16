# multipart/form-data

> HTTP 통신 중 미디어 파일 업로드 시 필요한 Content-Type

- 서로 붙어있는 여러 개의 메세지를 포함하여 하나의 복합 메세지로 전달
- MIME multipart 메시지는 boundary 파라미터를 포함하여 구분
- form의 시작과 끝을 서버에서 알수있도록 unique하게 설정 (e.g. UUID)
- header와 header 사이, header와 body사이 등 매 line마다 개행 필요
- body data(fields)를 구분하는 것은 --boundary\r\n 와 Content-Disposition

## Object
```swift
public extension Data {

    mutating func append(
        _ string: String,
        encoding: String.Encoding = .utf8
    ) {
        guard let data = string.data(using: encoding) else {
            return
        }
        append(data)
    }
}

// Object
public struct MultipartRequest {
    
    public let boundary: String
    
    private let separator: String = "\r\n"
    private var data: Data

    public init(boundary: String = UUID().uuidString) {
        self.boundary = boundary
        self.data = .init()
    }
    
    private mutating func appendBoundarySeparator() {
        data.append("--\(boundary)\(separator)")
    }
    
    private mutating func appendSeparator() {
        data.append(separator)
    }

    private func disposition(_ key: String) -> String {
        "Content-Disposition: form-data; name=\"\(key)\""
    }

    // add chunks of body data
    public mutating func add(
        key: String,
        value: String
    ) {
        appendBoundarySeparator()
        data.append(disposition(key) + separator)
        appendSeparator()
        data.append(value + separator)
    }
    // add a body chunk for files
    public mutating func add(
        key: String,
        fileName: String,
        fileMimeType: String,
        fileData: Data
    ) {
        appendBoundarySeparator()
        data.append(disposition(key) + "; filename=\"\(fileName)\"" + separator)
        data.append("Content-Type: \(fileMimeType)" + separator + separator)
        data.append(fileData)
        appendSeparator()
    }

    public var httpContentTypeHeadeValue: String {
        "multipart/form-data; boundary=\(boundary)"
    }

    public var httpBody: Data {
        var bodyData = data
        // ending a boundary
        bodyData.append("--\(boundary)--")
        return bodyData
    }
}
```

## Usage
```swift
var multipart = MultipartRequest()
for field in [
    "firstName": "Hans",
    "lastName": "Yim"
] {
    multipart.add(key: field.key, value: field.value)
}

multipart.add(
    key: "file",
    fileName: "multipart.mp4",
    fileMimeType: "video/mp4",
    fileData: "fake-video-data".data(using: .utf8)!
)

/// Create a regular HTTP URL request & use multipart components
let url = URL(string: "https://...")!
var request = URLRequest(url: url)
request.httpMethod = "POST"
request.setValue(multipart.httpContentTypeHeadeValue, forHTTPHeaderField: "Content-Type")
request.httpBody = multipart.httpBody

/// Fire the request using URL sesson or anything else...
let (data, response) = try await URLSession.shared.data(for: request)

print((response as! HTTPURLResponse).statusCode)
print(String(data: data, encoding: .utf8)!)
```


## Using Alamofire
> iOS, macOS를 위한 Swift 기반 HTTP 네트워킹 라이브러리

```swift
static func uploadVideoFile(item: InteractionItem, completion: String, fileUrl: URL) async throws -> String {
    guard let urlHttp = URL(string: AppData.interactionSubmitHttpUrl) else {
        fatalError("Invalid URL")
    }
    let header: HTTPHeaders = [
        "Content-Type": "multipart/form-data"
    ]
    let parameters: [String: Any] = [
        "user_seq": AppData.userInfo.userSeq,
        "project_seq": AppData.userInfo.projectSeq,
        "trial_index": AppData.userInfo.trialIndex,
        "interaction_type": item.type.code,
        "interaction_number": item.number,
        "complete_yn": completion
    ]    
    let request = AF.upload(multipartFormData: { MultipartFormData in
        // body
        for (key, value) in parameters {
            MultipartFormData.append("\(value)".data(using: .utf8)!, withName: key)
        }
        // file
        MultipartFormData.append(fileUrl, withName: "uploadFile", fileName: fileUrl.lastPathComponent, mimeType: "video/mp4")
    }, to: urlHttp, method: .post, headers: header)
    .validate(statusCode: 200..<500)
    .responseDecodable(of: ResponseBody.self) { response in
        switch response.result {
            // handle response here
            case .success(let response):
            case .failure(let error):
        }
    }
    // add async return here
}
```


## 출처
- [multipart/form-data 이용해서 사진/이미지 배열 업로드하기](https://lena-chamna.netlify.app/post/uploading_array_of_images_using_multipart_form-data_in_swift/)
- [Easy multipart file upload for Swift](https://theswiftdev.com/easy-multipart-file-upload-for-swift/)
- [Alamofire official](https://github.com/Alamofire/Alamofire)