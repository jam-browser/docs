# 개발 · 테스트 (개발자용)

현재 main 기준이에요. Windows 10/11, Node.js LTS 가 필요해요.

## 브라우저 (jam-browser/browser)

```bash
git clone https://github.com/jam-browser/browser.git
cd browser
npm install
node node_modules/electron/install.js   # npm 이 Electron 본체를 받지 않았을 때 (설치 스크립트가 막힌 환경)
npm start                               # 실행
npm test                                # 서버 26 + 중계 연동 9 + 비밀번호 뱅크 · 신원 13 + 입장 방식 · 친구 11
npm run dist                            # 설치 파일 → dist/
npm run icon                            # 앱 아이콘 다시 만들기 → build/icon.png
```

> 네트워크 드라이브(NAS)에서는 npm 설치가 깨지거나 Electron 이 실행되지 않을 수 있어요. 로컬 디스크에서 작업하세요.

### 중계 연동 테스트

`npm test` 의 두 번째 부분(`test/relay-test.js`)은 backend 저장소의 중계 서버를 써요. 아래 중 하나에 있으면 자동으로 찾고, 없으면 건너뛰어요.

- 환경 변수 `JAM_RELAY_PATH` (예: `C:\src\backend\jam-relay.js`)
- browser 폴더 옆의 `jam-backend-build`, `jam-backend`, `backend` 폴더

backend 폴더에서는 `npm ci` 를 먼저 해 두세요 (`ws` 필요).

### 비밀번호 뱅크 테스트는 Electron 안에서도

Electron 의 암호 라이브러리(BoringSSL)는 Node 와 지원 범위가 달라요 (예: `chacha20` 없음). 암호 코드를 바꾸면 Electron 런타임으로도 돌려 보세요.

```powershell
$env:ELECTRON_RUN_AS_NODE = '1'; .\node_modules\electron\dist\electron.exe test\vault-test.js
```

### 실제 앱 E2E (`test/e2e/`)

앱 두 개(호스트 · 게스트)를 원격 디버깅 포트로 띄워 자동으로 조작해요. 자세한 건 browser 저장소의 `test/e2e/README.md`.

| 스크립트 | 확인하는 것 |
| --- | --- |
| `jam-core.e2e.js` | 승인 후 참여, 다른 사이트 잠금, 권한, 화면 영역, 커서 숨김, 동영상 동기화, 광고 예외, 종료 |
| `resolution.e2e.js` | 해상도·확대율이 달라도 사각형·커서·스크롤 위치가 맞는지 |
| `login-follow-safety.e2e.js` | 로그인 대기 → 자동 합류, 패널 숨김 + 사람 따라가기, 클립보드 경고, 다운로드 보관, Jam 이 데려간 다운로드 차단 |
| `customize.e2e.js` | 스피드 다이얼, 검색 엔진, 북마크, 기록, 찾기, 확대, 사이트 CSS, 탭, 레이아웃·테마·프리셋 |
| `relay.e2e.js` | 중계 서버를 거친 참여 · 이동 · 채팅 · 화면 영역 · 종료 |

```powershell
Start-Process node -ArgumentList "test\e2e\test-site.js"            # 테스트 사이트 (포트 8765)
& .\test\e2e\run-e2e.ps1 jam-core.e2e.js (Resolve-Path .)
```

- 사용하는 포트: 호스트 앱 9301, 게스트 앱 9302, 테스트 사이트 8765 (지금 main 에서는 고정. 바꾸는 기능은 [열린 PR #2](open-pull-requests.md))
- `run-e2e.ps1` 은 실행 중인 Jam Browser 창을 모두 닫고 시작해요
- 인터넷 연결이 필요해요 (위키백과, example.com, 위키미디어 영상)
- 중계 E2E 를 실제 서버로: `$env:JAM_RELAY_ADDR = "서버주소:3031"` 후 `relay.e2e.js` 실행 (기본 `127.0.0.1:3031`)
- **작업 화면을 가리지 않게:** 앱을 직접 띄울 때 `$env:JAM_TEST_OFFSCREEN = '1'` 로 실행하면 (`run-e2e.ps1` 에는 아직 없음) 앱 창이 화면 밖에 뜨고 포커스를 뺏지 않아요. 이때는 창이 가려진 것으로 처리되지 않도록 Electron 에 `--disable-features=CalculateNativeWinOcclusion --disable-backgrounding-occluded-windows --disable-renderer-backgrounding` 을 함께 넘기세요 (없으면 웹페이지에 입력이 전달되지 않아요). 웹페이지 안 클릭은 CDP 대신 `webview.sendInputEvent` 로 보내세요
- 새 계정 프로필로 시작해야 처음 화면(계정 만들기)부터 확인돼요. 계정이 있는 프로필은 잠금 화면이 먼저 떠요

### Jam 기능 E2E (`test/e2e-jam/`)

근처 찾기 · 계정 · 화면 미러링 · 파일 · 설정 · 핑 · 대역폭 제한 중계 · 설치본 점검 · 자동 업데이트 서명을 실제 앱 두 개로 확인해요. 창은 화면 밖에 뜨고, 테스트마다 로그에서 메인 프로세스 오류를 검사해요. 자세한 건 browser 저장소의 `test/e2e-jam/README.md`.

```powershell
powershell -ExecutionPolicy Bypass -File test\e2e-jam\run.ps1                    # 전부
powershell -ExecutionPolicy Bypass -File test\e2e-jam\run.ps1 -Tests ping,files  # 일부
```

### CI

`ci/test.yml` 을 `.github/workflows/test.yml` 로 옮기면 PR · main 마다 `npm test` 가 Windows 에서 돌아요 (gh 토큰에 `workflow` 권한 필요: `gh auth refresh -s workflow`).

## 중계 서버 (jam-browser/backend)

```bash
git clone https://github.com/jam-browser/backend.git
cd backend
npm ci
npm test        # 중계 서버 7개 + 자동 업데이트 7개
npm start       # 포트 3031
```

자동 업데이트 테스트는 가짜 GitHub 서버를 띄워 서명 위조 · 파일 변조 · 고장 난 버전 · 진행 중인 Jam 대기를 확인해요.

## 저장소 규칙

- 개인 정보(내 PC 경로, 개인 이메일, 키)는 커밋하지 않아요. 작성자 이메일은 GitHub noreply 주소를 써요.
- backend 의 배포 비밀키(`*.pem`)는 절대 저장소에 넣지 않아요 (`.gitignore` 에 포함, 공개키 `release-key.pub.pem` 만 예외).
