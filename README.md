# Jam Browser 문서

친구와 같은 웹페이지를 함께 보는 Windows 데스크톱 브라우저, **Jam Browser** 의 문서예요.

## 현재 버전 (2026-11-17 기준, 각 저장소 main)

| | 버전 | 기준 커밋 |
| --- | --- | --- |
| 브라우저 | [v0.1.0](https://github.com/jam-browser/browser/releases/tag/v0.1.0) | `5e40e00` |
| 중계 서버 | [v1.0.2](https://github.com/jam-browser/backend/releases/tag/v1.0.2) | `47a97f5` |

이 문서는 위 main 코드 기준이에요. 아직 합쳐지지 않은 변경은 [열린 PR 정리](open-pull-requests.md) 에 따로 모아 뒀어요.

## 문서

| 문서 | 내용 |
| --- | --- |
| [사용 설명서](user-guide.md) | 설치, Jam 시작·참여, 기능, 정해진 값, 꾸미기, 단축키 |
| [포트 · 포트 포워딩](ports.md) | 어떤 포트를 열어야 하는지, 공유기 설정 방법 |
| [중계 서버](relay-server.md) | 멀리 있는 친구와 하기 위한 서버 설치와 자동 업데이트 |
| [보안](security.md) | 암호화, 참여 승인, 제한 값, 다운로드 보관, 클립보드 경고 등 |
| [개인정보](privacy.md) | 무엇이 누구에게 공유되는지 |
| [문제 해결](troubleshooting.md) | 접속이 안 될 때, 경고 창, 알려진 문제, 로그 위치 |
| [구조](architecture.md) | 저장소와 구성 요소, 동작 흐름 |
| [개발 · 테스트](development.md) | 소스로 실행, 테스트, 실제 앱 E2E (개발자용) |
| [릴리스 절차](releasing.md) | 브라우저 · 중계 서버 새 버전 내는 방법 (개발자용) |
| [열린 PR 정리](open-pull-requests.md) | 아직 main 에 합쳐지지 않은 변경 4건과 영향 |

## 저장소

| 저장소 | |
| --- | --- |
| [browser](https://github.com/jam-browser/browser) | 브라우저 앱 (설치 파일은 [Releases](https://github.com/jam-browser/browser/releases/latest)) |
| [backend](https://github.com/jam-browser/backend) | 중계 서버 (Windows, 서명된 자동 업데이트) |
| [docs](https://github.com/jam-browser/docs) | 이 문서 |

이 프로젝트는 특정 회사·서비스와 관련이 없어요. [MIT 라이선스](LICENSE)
