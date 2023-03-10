# Tuist 알아보기
> XCode 프로젝트를 생성/관리하는 Command Line Tool


- 비슷한 툴인 XCodeGen은 yml/json 언어로 관리하지만 Tuist는 swift 언어로 관리
- 협업 시 충돌방지 목적으로 사용
- 협업 시 주로 xcodeproj 혹은 xcworkspace에서 잦은 conflict 발생
- Tuist에서는 command 명령어를 통해 xcodeproj, xcworkspace 생성
- 그 후 Tuist의 Project.swift 파일을 통해 의존성/타겟/프로젝트 설정들을 관리
- 클린 아키텍처를 위한 모듈화(Modularization) 구성에 유용
- CocoaPods 지원안함


### 출처
- [Tuist Github Readme.md](https://github.com/tuist/tuist)
- [모듈화 개념 - Tuist로 프로젝트 관리 방법](https://ios-development.tistory.com/1006)
- [당근마켓) XcodeGen 에서 Tuist 로 전환하기](https://medium.com/daangn/xcodegen-%EC%97%90%EC%84%9C-tuist-%EB%A1%9C-%EC%A0%84%ED%99%98%ED%95%98%EA%B8%B0-3f0156e0c2ea)