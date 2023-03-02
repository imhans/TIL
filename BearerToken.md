# Bearer Token 인증 방식
> 서버에서 클라이언트로 보내지는 인증 토큰
- 토큰 이름 그대로 '보유자' 역할
- 클라이언트가 서버에 요청 보낼 때 요청에 토큰을 첨부하면, 서버는 토큰 정보를 통해 인증여부를 확인
- Http Header에 정보를 실어 나르기 때문에 유저 세션을 유지할 필요가 없고 메모리를 잡아먹지 않음
- 유효기간이 짧은 accessToken에는 디테일한 정보 저장
- 유효기간이 긴 refreshToken은 accessToken 재발급 역할

## 출처
- [OAuth와 JWT 도입하기](https://seungwoolog.tistory.com/95)