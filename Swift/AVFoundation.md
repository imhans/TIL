# AVFoundation Framework

## Intro
- Audiovisual Assets 작업, device cameras 제어, audio 처리 및 system audio와의 상호작용 구성
- iOS, macOS, watchOS용 시청각 미디어 프레임워크
- Apple 플랫폼에서 시청각 미디어를 검사, 재생, 캡처 및 처리하기 위한 광범위한 작업을 함께 포함하는 몇 가지 주요 기술 영역을 결합
- Topics
    - Common, **Playback**, **Capture**, Editing, Audio
- AVKit과의 차이
    - AVFoundation이 상대적으로 더 low-level framework이기에 커스텀 시 적합
    - AVKit은 AVFoundation 위에 구축된 companion framework, view-level interface에 가까워 표준 UI 만들시에 용이
    <img src="https://user-images.githubusercontent.com/38341386/214844072-2127e223-7a3c-4689-ba13-49a54a25746a.png">

## Playback
- Player는 media asset의 재생 및 컨트롤(예: 재생 시작 및 중지, 특정 시간 탐색)을 가능하게 하는 컨트롤러 오브젝트
- 한 번에 하나의 media asset만 관리함
- 또한 여러 asset이 순차적으로 재생되도록 큐에 넣는 Queue Player를 제공
- Players: AVPlayer, AVQueuePlayer, ...
- Player Items: AVPlayerItem, ...
- Presentation: AVPlayerLayer, AVSynchronizedLayer
    <img src="https://user-images.githubusercontent.com/38341386/215780235-74179c5f-b54d-4a15-97a2-eae2a2188f25.png">
AVPlayer와 AVPlayerItem은 Nonvisual objects기 때문에, view에 보여주고 싶다면 AVKit 프레임워크의 AVPlayerViewController 혹은 AVPlayerLayer를 사용해야 함
- AVPlayerViewController은 편하게 사용 가능한 풀옵션 패키지
- AVPlayerLayer는 수작업 커스터마이징 버전
- AVSynchronizedLayer는 AVPlayerItem에 싱크하여 다양한 애니메이션 효과를 줄 수 있음

## Capture
- AVFoundation 캡처시스템은 iOS 및 macOS에서 비디오, 사진 및 오디오 캡처 서비스를 위한 높은 수준의 아키텍처를 제공
- 캡쳐 아키텍쳐의 메인은 입력(인풋), 출력(아웃풋), 세션
- 입력은 카메라, 마이크와 같은 기기의 내장 하드웨어를 포함한 미디어 소스
- 출력은 입력에서 획득한 미디어 소스로 데이터를 생성
- 세션은 입력과 출력의 연결고리
<img src="https://user-images.githubusercontent.com/38341386/214845700-e3a19280-7ef7-4656-9843-85b9af24aa14.png">

AVCaptureSession
- 캡처 동작을 구성하고 입력 장치에서 출력 캡처로의 데이터 흐름을 조정하는 개체
- An object that configures capture behavior and coordinates the flow of data from input devices to capture outputs


## 출처
- [AVFoundation Apple Documentation](https://developer.apple.com/documentation/avfoundation)
- [AVFoundation Overview](https://developer.apple.com/av-foundation/)
- [AVPlayer](https://developer.apple.com/documentation/avfoundation/avplayer)
- [AVCaptureDevice](https://developer.apple.com/documentation/avfoundation/avcapturedevice)