# 열린 PR 정리 (browser 저장소)

2026-11-17 기준, [jam-browser/browser](https://github.com/jam-browser/browser/pulls) 에 열려 있는 PR 4건이에요.
**아직 main 에 합쳐지지 않았어요.** 이 문서의 다른 페이지들은 합쳐지기 전(main) 기준이에요.

> **합쳐진 PR:** [#6 브라우저 계정 · 이중 암호화 비밀번호 뱅크 · 친구/자유 입장 Jam](https://github.com/jam-browser/browser/pull/6) (main `01cfcb5`) → [계정 · 비밀번호 뱅크 · 친구](account.md), [보안](security.md#브라우저-계정--비밀번호-뱅크)
> 이 변경 때문에 아래 #1 · #2 가 main 과 **충돌**해요 (`src/main.js`, #2 는 `package.json` 도). 합치려면 작성자가 main 을 받아 충돌을 풀어야 해요.

| PR | 제목 | 작성 | 기준 가지 | 규모 | 상태 |
| --- | --- | --- | --- | --- | --- |
| [#1](https://github.com/jam-browser/browser/pull/1) | Chrome 호환성: User-Agent 정리 + Chrome 확장 프로그램 지원 | wwg316, 11-17 13:48 | `main` | +356 / −2, 11파일 | 열림 · **충돌** (`src/main.js`) |
| [#2](https://github.com/jam-browser/browser/pull/2) | Jam 테스트 환경(`npm run jam`) + E2E 포트 설정 | wwg316, 11-17 14:51 | `main` | +187 / −18, 14파일 | 열림 · **충돌** (`src/main.js`, `package.json`) |
| [#3](https://github.com/jam-browser/browser/pull/3) | 영상 소리 켜기: 전체 음소거 제거 + 탭별 음소거 | wwg316, 11-17 14:52 | `#2` 위에 쌓음 | +44 / −7, 8파일 | 열림 · 충돌 없음 |
| [#4](https://github.com/jam-browser/browser/pull/4) | 유튜브 재생목록에서 다음 영상으로 넘어갈 때 Jam 이 무한 반복되던 문제 | wwg316, 11-17 15:35 | `#3` 위에 쌓음 | +79 / −23, 5파일 | 열림 · 충돌 없음 |

**합치는 순서:** #1 은 따로. #2 → #3 → #4 는 차례로 쌓여 있어서 이 순서대로 합쳐야 해요 (#3 을 빼면 #4 는 main 기준으로 다시 정리해야 해요).

---

## #1 Chrome 호환성 + Chrome 확장 프로그램

**바뀌는 것**

- **User-Agent:** 기본 UA 에서 `Electron/…`, `jam-browser/…` 표시를 빼고 Chrome 처럼 주 버전만 남김 (`Chrome/156.0.0.0`). 앱 전체와 웹페이지 세션 둘 다 적용
  - 목적: Electron 기반 브라우저를 막는 사이트(예: Google 로그인의 "안전하지 않은 브라우저") 대응
- **Chrome 확장 프로그램 지원:** `electron-chrome-extensions` (MIT) 추가
  - 확장의 탭 만들기·선택·닫기를 앱 탭과 연결, 앱에서 탭을 바꾸면 확장에도 알림
- **Chrome 웹 스토어에서 설치:** `electron-chrome-web-store` (MIT) 추가, 확장 자동 업데이트
  - 설치할 때 권한 확인 창. "모든 사이트" 권한이면 Jam 탭 내용도 읽을 수 있다고 경고
  - 아이콘 우클릭 → 삭제하면 파일까지 제거
- **화면:** 툴바에 확장 아이콘 줄과 🧩 웹 스토어 버튼, 화면 보안 정책에 `crx:` 이미지 허용
- **빌드:** `postinstall` 단계에서 `scripts/build-toolbar-preload.js` 가 `src/generated/toolbar-preload.js` 를 만듦 (gitignore)
- **테스트:** `test/e2e/extensions.e2e.js` 추가 (UA, Client Hints, 확장 아이콘 줄, 웹 스토어 연동 등 8개)

**파일:** `src/main.js` (+105), `src/renderer/app.js` (+30), `src/preload.js`, `src/renderer/index.html`, `styles.css`, `package.json` · `package-lock.json` (의존성 2개), `.gitignore`, `scripts/build-toolbar-preload.js`, `test/e2e/*`

**PR 에 적힌 확인 결과:** `npm test`, E2E `jam-core` · `login-follow-safety` · `customize` · `extensions` 통과.
웹 스토어에서 실제 확장 설치는 아직 수동 확인 전. `resolution` E2E 의 "스크롤 따라가기 (다른 크기)" 1건은 작성자 PC 에서 main 에서도 실패했다고 적혀 있음.

**합치기 전에 정할 것**

- UA 를 Chrome 처럼 바꿔서 사이트의 Electron 차단을 피하는 방식이 각 사이트 정책에 맞는지 (특히 Google 로그인은 의도적으로 내장 브라우저를 막고 있음)
- 확장 프로그램은 Jam 탭 내용(주소, 화면)을 읽을 수 있어요 → [보안](security.md) 문서에 반영 필요
- 확장 프로그램은 웹페이지의 로그인 칸도 읽을 수 있어서, 🔑 로 채운 비밀번호를 볼 수 있어요 (비밀번호 뱅크 자체 파일은 못 읽음)
- `postinstall` 스크립트: npm 설정에 따라 설치 스크립트가 막히는 환경에서는 `src/generated/` 가 안 만들어질 수 있음
- 새 의존성 2개(MIT)의 라이선스 고지

---

## #2 Jam 테스트 환경(`npm run jam`) + E2E 포트 설정

**바뀌는 것**

- **`npm run jam`** (`scripts/jam-dev.js`): 한 PC 에서 호스트 · 게스트 앱 두 개와 로컬 중계 서버(`../backend`, `127.0.0.1:3031`)를 한 번에 실행, Ctrl+C 로 전부 종료
  - 옵션: `--guests N`, `--fresh`(프로필 초기화), `--no-relay`, `--debug`(빈 디버깅 포트를 골라 출력)
  - 프로필은 `.jam-dev/` 에 유지 (gitignore)
  - 개발 실행일 때만 창 제목에 `[host]` · `[guest]` 표시 (설치된 앱은 그대로)
- **E2E 포트 설정:** Windows 가 Hyper-V/WSL 용으로 예약하는 포트(예: 9247–9346, 8740–8839)와 겹치면 기본 포트(9301/9302, 8765)로 실행이 안 되던 문제
  - `JAM_E2E_HOST_PORT` · `JAM_E2E_GUEST_PORT` · `JAM_E2E_SITE_PORT` 로 바꿀 수 있음 (`test/e2e/ports.js`, 기본값은 그대로)
  - `run-e2e.ps1`: 매번 이전 테스트 프로필(`ud-host`/`ud-guest`)을 지우고 시작, 디버깅 포트에 못 붙으면 원인 안내
- **README:** 테스트 환경 사용법 추가

**파일:** `scripts/jam-dev.js` (새 파일, +117), `src/main.js` (+6, 창 제목), `test/e2e/*` (포트 설정), `README.md`, `package.json`, `.gitignore`

**PR 에 적힌 확인 결과:** `npm test`, `npm run jam -- --debug` 로 띄운 창에서 `relay.e2e.js`, 바꾼 포트로 `customize.e2e.js` 통과, Ctrl+C 종료 확인.

**영향:** 사용자에게 보이는 동작은 바뀌지 않아요 (개발 실행일 때 창 제목만). 개발·테스트 편의.

---

## #3 영상 소리 켜기: 전체 음소거 제거 + 탭별 음소거

**바뀌는 것**

- **전체 음소거 제거:** 지금 main 은 모든 창·탭을 `setAudioMuted(true)` 로 음소거하고 다시 켤 방법이 없음 → 기본으로 소리가 나게 바꿈
- **탭별 음소거:** 소리 나는 탭에 🔊 표시, 누르면 그 탭만 🔇 / 해제. 탭 우클릭 메뉴에 "탭 음소거"
  - 나에게만 적용, Jam 참여자와 공유하지 않음
- main 프로세스가 webview 의 `audio-state-changed` 를 앱 화면에 전달 (`tab-audio`)
- 개발·테스트에서 소리가 겹치지 않게: `npm run jam` 의 게스트 창과 E2E 앱은 `--mute-audio` 로 실행

**파일:** `src/main.js`, `src/preload.js`, `src/renderer/app.js` (+28), `customize.js`, `styles.css`, `README.md`, `scripts/jam-dev.js`, `test/e2e/run-e2e.ps1`

**PR 에 적힌 확인 결과:** 실제 앱 두 창에서 🔊/🔇 동작, `--mute-audio` 창은 소리 없음, `jam-core.e2e.js`(동영상 동기화 · 광고 예외 포함), `npm test` 통과.

**합치기 전에 정할 것**

- **처음 정한 요구사항("앱에서 소리를 내지 않는다")과 반대**예요. 합치면 기본 동작이 "소리 남"으로 바뀌어요.
  - 다른 방법: 기본은 음소거로 두고, 탭별 🔊 버튼이나 설정으로 켤 수 있게 하는 절충안
- 합치면 문서의 [사용 설명서](user-guide.md) · [보안](security.md) · [문제 해결](troubleshooting.md) 의 "음소거" 설명을 바꿔야 해요

---

## #4 유튜브 재생목록 무한 반복 수정

**고치는 문제:** Jam 중 유튜브 **재생목록**에서 다음 영상으로 넘어가면, 영상이 1~2초마다 처음부터 다시 시작되며 넘어가지 않음 (일반 영상은 문제없음)

**원인 (PR 설명):**

- 재생목록 주소(`watch?v=…&list=…&index=…`)를 열면 유튜브가 `index` · `pp` 를 스스로 바꿈
- Jam 이 이를 "다른 페이지로 이동"으로 보고 보냄 → 받은 쪽이 다시 불러옴 → 거기서도 `index` 가 바뀌어 되돌려 보냄 → 무한 반복

**바뀌는 것**

- `shared/site.js`: `docKey` / `sameDoc` 추가 — 유튜브 영상 페이지는 영상(`v`)과 재생목록(`list`)만 비교 (`index`, `pp`, `t` 등 무시). 서버와 앱이 같은 규칙 사용
- `src/renderer/app.js`: 이동 판단(마지막 동기화 주소, 받은 이동, 제목)을 정확한 주소 비교 대신 `sameDoc` 으로
- `src/webview-preload.js`: 유튜브는 다음 영상에서도 같은 `<video>` 를 다시 써서 "새 영상"으로 인식되지 않던 문제 → 영상이 바뀌면 새 영상으로 보고 재생 위치를 다시 맞춤. 광고가 끝난 뒤에도 기준이 없으면 새로 잡음
- `server/jam-server.js`: 제목 갱신 비교도 `sameDoc`
- `test/sync-test.js`: 같은 문서 판단 단위 테스트 추가 (27개)

**영향:** 버그 수정. 사용자에게 보이는 동작은 "재생목록에서도 정상 동작". 다만 **#3 위에 쌓여 있어서** 그대로 합치면 #3(소리 켜기)도 같이 들어와요.

---

## 합친 뒤 이 문서 정리

PR 이 합쳐지면 해당 항목을 이 페이지에서 지우고, 다른 문서(사용 설명서 · 보안 · 문제 해결 · 개발)를 새 동작에 맞게 고쳐요.
