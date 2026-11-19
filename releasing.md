# 릴리스 절차 (개발자용)

## 브라우저 (jam-browser/browser)

```bash
# 1. package.json 과 package-lock.json(맨 위 두 곳)의 version 올리기 (예: 0.2.1)
npm test
npm run dist                                  # dist/JamBrowser-Setup-0.2.1.exe, .blockmap, latest.yml
node tools/sign-update.js --key <브라우저 배포 비밀키.pem> dist/JamBrowser-Setup-0.2.1.exe   # → .exe.sig (만들고 바로 검증)
cp dist/JamBrowser-Setup-0.2.1.exe dist/JamBrowser-Setup.exe   # 버전 없는 이름의 사본 (고정 다운로드 링크용)
git tag v0.2.1 <버전 PR 을 합친 main 커밋> && git push origin v0.2.1
gh release create v0.2.1 --repo jam-browser/browser \
  dist/JamBrowser-Setup-0.2.1.exe dist/JamBrowser-Setup-0.2.1.exe.sig dist/JamBrowser-Setup-0.2.1.exe.blockmap \
  dist/latest.yml dist/JamBrowser-Setup.exe
```

- `latest.yml` 을 꼭 같이 올려야 설치된 앱이 새 버전을 찾아요.
- **`JamBrowser-Setup-<버전>.exe.sig` 를 꼭 같이 올려야 해요.** v0.3.1 부터 앱은 받은 설치 파일을 이 서명과 앱 안의 공개키(`src/update-key.pub.pem`)로 확인한 뒤에만 설치해요. 서명이 없거나 안 맞으면 받은 파일을 지우고 설치하지 않아요.
  - `--key` 대신 환경 변수 `JAM_BROWSER_RELEASE_KEY` 로 비밀키 경로를 줄 수 있어요
  - 서명은 `jam-browser-update|<버전>|<SHA-512>` 에 대한 Ed25519 서명이에요. 버전이 들어가서 옛 버전 설치 파일에 새 버전 이름을 붙여 되돌리는 것도 막아요
  - 브라우저 비밀키는 중계 서버 비밀키와 **다른 키**예요. 관리 방법은 아래 [배포 비밀키](#배포-비밀키) 와 같아요
- `JamBrowser-Setup.exe` 를 같이 올려야 README 의 "설치 파일 받기" 링크(`releases/latest/download/JamBrowser-Setup.exe`)가 새 버전을 가리켜요.
- 설치된 앱은 켤 때 5초 뒤, 그다음 설정한 간격(기본 4시간)마다 확인해요. 설치 시점은 사용자가 설정 → 업데이트 에서 고르고(끌 때 / 쓰지 않을 때 / 직접), 기본은 끌 때예요.
- 릴리스 전에 [개발 · 테스트](development.md) 의 E2E 도 돌려 보세요. 특히 패키지 빌드로 `test/e2e-jam/run.ps1 -Packaged -Tests smoke,update` (update 는 `JAM_BROWSER_RELEASE_KEY` 필요).
- 태그만 올리면 자동으로 빌드되게 하려면 `ci/github-release.yml` 을 `.github/workflows/release.yml` 로 옮겨 올리세요 (gh 토큰에 `workflow` 권한 필요). 이 경우에도 `.sig` 는 비밀키가 있는 PC 에서 만들어 릴리스에 추가해야 해요 (비밀키를 GitHub 에 올리지 않기 위해).

## 중계 서버 (jam-browser/backend)

```bash
npm version patch                              # 1.0.2 → 1.0.3
npm test
git push origin main --follow-tags
node tools/make-release.js --key <배포 비밀키.pem> --publish
```

- `make-release.js` 가 zip 을 만들고, 비밀키로 서명하고, 저장소의 공개키로 바로 검증한 뒤 릴리스를 올려요.
- 설치된 서버들은 5분 안에 서명을 확인하고, 진행 중인 Jam 이 없을 때 업데이트해요.

### 배포 비밀키

- 저장소에 절대 넣지 마세요 (`.gitignore` 에 `*.pem` 이 들어 있어요. 공개키 `release-key.pub.pem` 만 예외).
- **꼭 백업하세요.** 잃어버리면 이미 설치된 서버에 업데이트를 보낼 수 없어서 서버마다 다시 설치해야 해요.
- 키를 바꿔야 할 때: 새 키로 만든 `release-key.pub.pem` 을 **옛 키로 서명한 릴리스**에 넣어 한 번 배포 → 그다음부터 새 키로 서명.

## 체크리스트

- [ ] 테스트 통과 (browser: `npm test` + `test/e2e`, backend: `npm test`)
- [ ] 개인 정보(내 PC 경로, 이메일 등)가 커밋에 없는지
- [ ] 릴리스 노트에 바뀐 점
