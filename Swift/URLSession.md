# URLSession 이해하기

URLSession이란
- 앱과 서버 간 데이터 통신을 위한 API를 제공하는 클래스
- Alamofire와 Moya 또한 URLSession 기반으로 만든 것 
- HTTP 통신과 마찬가지로 Request and Response 구조 가짐
- 유형 중 Singleton shared session은 configuration을 포함하지 않으며, 커스텀하기 위해선 URLSession 생성 시 포함해야 함
    - configurations: .default, .ephemeral, .background 

URLSession 동작순서
1. URLSession 생성 (singleton or with configuration)
2. Task 선택 (-> URLSessionTask)
3. data, response, error 처리
4. dataTask.resume() 하여 네트워크 통신 

URLSessionTask
- 세션 작업 단위, 3가지 유형 존재
- URLSessionDataTask, URLSessionUploadTask, URLSessionDownloadTask

URLComponents
- URL 구성하는 컴포넌트 구조체
- URLQueryItem를 components?.queryItems?.append 하여 Query Parameter를 다룰 때 용이
- absoluteString, scheme, host, path 등 url 기본 요소 접근 가능

URI vs URL
- URI(식별자) contains URL(위치)
- 리소스 접근 방법을 지정하는 프로토콜(http, ftp 등)을 포함한다면 URL

## 출처
- [URLSession Class](https://developer.apple.com/documentation/foundation/urlsession)
- [[iOS] URLSession 개념](https://jeong9216.tistory.com/464)