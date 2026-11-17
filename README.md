# Jam Browser 문서

친구와 같은 웹페이지를 함께 보는 Windows 데스크톱 브라우저, **Jam Browser** 의 문서예요.

### ⬇ [Jam Browser 설치 파일 받기 (Windows, 최신 버전)](https://github.com/jam-browser/browser/releases/latest/download/JamBrowser-Setup.exe)

## 현재 버전 (2026-11-17 기준, 각 저장소 main)

| | 버전 | 기준 커밋 |
| --- | --- | --- |
| 브라우저 | 설치 파일 [v0.2.0](https://github.com/jam-browser/browser/releases/tag/v0.2.0), main 은 그 이후 | `5ba71a3` |
| 중계 서버 | [v1.0.3](https://github.com/jam-browser/backend/releases/tag/v1.0.3) (대역폭 한도) | `026ea65` |

이 문서는 위 main 코드 기준이에요. v0.2.0 새 기능: **브라우저 계정 · 비밀번호 뱅크 · 친구/자유 입장 Jam** → [계정 · 비밀번호 뱅크 · 친구](account.md).
main 에만 있고 다음 릴리스에 들어갈 것:
- **중계 서버 주소 목록 · 근처 Jam(같은 네트워크) · 링크 없이 확인 코드로 들어가기** → [사용 설명서](user-guide.md#근처-jam-같은-와이파이-초대-링크-없이), [설계안 1단계](design.md)
- **화면 미러링 + 조작 권한(1P/2P 게임)** → [사용 설명서](user-guide.md#화면-미러링-호스트-탭을-영상으로), [보안](security.md#화면-미러링--원격-조작), [설계안 2단계](design.md)
- **Chrome 확장 프로그램 · 웹 스토어, 탭별 소리 · 음소거** (wwg316, #1 · #3) → [사용 설명서](user-guide.md#브라우저-기능)
- **로컬 파일 같이 보기 (PDF · HTML · 이미지 · 텍스트 · SWF)** → [사용 설명서](user-guide.md#같이-보는-파일-로컬-pdf--html--이미지--텍스트--swf), [보안](security.md#같이-보는-파일), [설계안 3단계](design.md)
합쳐진 PR 과 바뀐 기본 동작은 [PR 정리](open-pull-requests.md) 에 모아 뒀어요.

## 문서

| 문서 | 내용 |
| --- | --- |
| [사용 설명서](user-guide.md) | 설치, Jam 시작·참여, 기능, 정해진 값, 꾸미기, 단축키 |
| [계정 · 비밀번호 뱅크 · 친구](account.md) | 브라우저 계정(잠금), 이중 암호화 비밀번호 뱅크, 친구 코드, Jam 입장 방식(수락 · 친구만 · 자유) |
| [포트 · 포트 포워딩](ports.md) | 어떤 포트를 열어야 하는지, 공유기 설정 방법 |
| [중계 서버](relay-server.md) | 멀리 있는 친구와 하기 위한 서버 설치와 자동 업데이트 |
| [보안](security.md) | 암호화, 참여 승인 · 입장 방식, 계정 금고 암호화 구조, 제한 값, 다운로드 보관, 클립보드 경고 등 |
| [개인정보](privacy.md) | 무엇이 누구에게 공유되는지 |
| [문제 해결](troubleshooting.md) | 접속이 안 될 때, 경고 창, 알려진 문제, 로그 위치 |
| [구조](architecture.md) | 저장소와 구성 요소, 동작 흐름 |
| [개발 · 테스트](development.md) | 소스로 실행, 테스트, 실제 앱 E2E (개발자용) |
| [릴리스 절차](releasing.md) | 브라우저 · 중계 서버 새 버전 내는 방법 (개발자용) |
| [설계안 v2](design.md) | 주소 미러링 / 화면 미러링(1P·2P), 로컬 PDF · HTML 같이 보기, 근처 찾기, 계정으로 이어 보기, 브라우저 · 보안 기능 계획 |
| [PR 정리](open-pull-requests.md) | 합쳐진 PR 목록과 바뀐 기본 동작 (소리 · 확장 프로그램) |

## 저장소

| 저장소 | |
| --- | --- |
| [browser](https://github.com/jam-browser/browser) | 브라우저 앱 (설치 파일은 [Releases](https://github.com/jam-browser/browser/releases/latest)) |
| [backend](https://github.com/jam-browser/backend) | 중계 서버 (Windows, 서명된 자동 업데이트) |
| [docs](https://github.com/jam-browser/docs) | 이 문서 |

이 프로젝트는 특정 회사·서비스와 관련이 없어요. [MIT 라이선스](LICENSE)
