# AVFoundation Framework

## Intro
- Audiovisual Assets 작업, device cameras 제어, audio 처리 및 system audio와의 상호작용 구성
- iOS, macOS, watchOS용 시청각 미디어 프레임워크
- Apple 플랫폼에서 시청각 미디어를 검사, 재생, 캡처 및 처리하기 위한 광범위한 작업을 함께 포함하는 몇 가지 주요 기술 영역을 결합
- Topics
    - Common, Playback, Capture, Editing, Audio
- AVKit과의 차이
    - AVFoundation이 상대적으로 더 low-level framework이기에 커스텀 시 적합
    - AVKit은 AVFoundation 위에 구축된 companion framework, view-level interface에 가까워 표준 UI 만들시에 용이
    <img src="https://user-images.githubusercontent.com/38341386/214844072-2127e223-7a3c-4689-ba13-49a54a25746a.png">

## Capture
- AVFoundation 캡처시스템은 iOS 및 macOS에서 비디오, 사진 및 오디오 캡처 서비스를 위한 높은 수준의 아키텍처를 제공
- 캡쳐 아키텍쳐의 메인은 입력(인풋), 출력(아웃풋), 세션
- 입력은 카메라, 마이크와 같은 기기의 내장 하드웨어를 포함한 미디어 소스
- 출력은 입력에서 획득한 미디어 소스로 데이터를 생성
- 세션은 입력과 출력의 연결고리
<img src="https://user-images.githubusercontent.com/38341386/214845700-e3a19280-7ef7-4656-9843-85b9af24aa14.png">


## 출처
- [AVFoundation Apple Documentation](https://developer.apple.com/documentation/avfoundation)
- [AVFoundation Overview](https://developer.apple.com/av-foundation/)