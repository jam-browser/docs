# 릴리스 절차 (개발자용)

## 브라우저 (jam-browser/browser)

```bash
# 1. package.json 의 version 올리기 (예: 0.1.1)
npm test
npm run dist                                  # dist/JamBrowser-Setup-0.1.1.exe, .blockmap, latest.yml
cp dist/JamBrowser-Setup-0.1.1.exe dist/JamBrowser-Setup.exe   # 버전 없는 이름의 사본 (고정 다운로드 링크용)
git tag v0.1.1 && git push origin main v0.1.1
gh release create v0.1.1 --repo jam-browser/browser \
  dist/JamBrowser-Setup-0.1.1.exe dist/JamBrowser-Setup-0.1.1.exe.blockmap dist/latest.yml dist/JamBrowser-Setup.exe
```

- `latest.yml` 을 꼭 같이 올려야 설치된 앱이 새 버전을 찾아요.
- `JamBrowser-Setup.exe` 를 같이 올려야 README 의 "설치 파일 받기" 링크(`releases/latest/download/JamBrowser-Setup.exe`)가 새 버전을 가리켜요.
- 설치된 앱은 켤 때 5초 뒤, 그다음 4시간마다 확인하고, 다시 켤 때 적용해요.
- 릴리스 전에 [개발 · 테스트](development.md) 의 E2E 도 돌려 보세요.
- 태그만 올리면 자동으로 빌드되게 하려면 `ci/github-release.yml` 을 `.github/workflows/release.yml` 로 옮겨 올리세요 (gh 토큰에 `workflow` 권한 필요).

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
