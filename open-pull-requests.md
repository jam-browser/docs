# PR 정리 (browser 저장소)

2026-11-18 기준, [jam-browser/browser](https://github.com/jam-browser/browser/pulls) 에 **열려 있는 PR 은 없어요.** 모두 main 에 합쳐졌어요.

| PR | 내용 | 작성 | 문서 |
| --- | --- | --- | --- |
| [#1](https://github.com/jam-browser/browser/pull/1) | Chrome 호환성: User-Agent 정리 + Chrome 확장 프로그램 · 웹 스토어 | wwg316 | [사용 설명서](user-guide.md#브라우저-기능), [보안](security.md#알려진-한계) |
| [#2](https://github.com/jam-browser/browser/pull/2) | Jam 테스트 환경(`npm run jam`) + E2E 포트 설정 | wwg316 | [개발 · 테스트](development.md) |
| [#3](https://github.com/jam-browser/browser/pull/3) | 영상 소리 켜기: 전체 음소거 제거 + 탭별 음소거 | wwg316 | [사용 설명서](user-guide.md#브라우저-기능) |
| [#4](https://github.com/jam-browser/browser/pull/4) | 유튜브 재생목록에서 Jam 이 무한 반복되던 문제 | wwg316 | — |
| [#6](https://github.com/jam-browser/browser/pull/6) | 브라우저 계정 · 이중 암호화 비밀번호 뱅크 · 친구/자유 입장 | 39nhs | [계정](account.md) |
| [#8](https://github.com/jam-browser/browser/pull/8) | 1단계: 중계 서버 주소 목록 · 근처 찾기 · 페어링 | 39nhs | [설계안](design.md) |
| [#9](https://github.com/jam-browser/browser/pull/9) | 2단계: 화면 미러링 + 조작 권한 | 39nhs | [사용 설명서](user-guide.md#화면-미러링-호스트-탭을-영상으로) |
| [#10](https://github.com/jam-browser/browser/pull/10) | 창 크기가 다를 때 보는 영역 사각형 · 커서 위치 어긋남 | wwg316 | — |
| [#11](https://github.com/jam-browser/browser/pull/11) | 3단계: 로컬 파일 같이 보기 | 39nhs | [사용 설명서](user-guide.md#같이-보는-파일-로컬-pdf--html--이미지--텍스트--swf) |

## 합쳐지면서 바뀐 기본 동작

- **소리 (#3):** 처음 요구사항은 "앱에서 소리를 내지 않는다" 였는데, #3 으로 **기본이 "소리 남"** 으로 바뀌었어요. 탭마다 🔊 를 눌러 음소거할 수 있어요. 화면 미러링과 공유 파일은 여전히 소리를 보내지 않아요
- **확장 프로그램 (#1):** 설치한 확장은 Jam 탭 내용(주소 · 화면)과 웹페이지 입력 칸을 읽을 수 있어요 (🔑 로 채운 비밀번호 포함, 비밀번호 뱅크 파일 자체는 못 읽음)
- **창 크기가 다른 사람끼리 (#10):** 커서 · 클릭은 글자 기준, 화면 가장자리에는 예비 앵커 2개
