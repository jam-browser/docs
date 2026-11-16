# 중계 서버

같은 와이파이가 아닌 친구와 Jam 을 하려면, 호스트와 친구가 모두 접속할 수 있는 **중계 서버**가 하나 있으면 돼요.
코드와 설치 파일: [jam-browser/backend](https://github.com/jam-browser/backend)

## 어떤 서버를 쓰나

- Windows 10/11 또는 Windows Server, **가장 싼 사양(1 vCPU / 1GB)** 이면 충분해요
- 클라우드 VPS 또는 항상 켜 두는 집 PC
- 필요한 것: Node.js LTS, 들어오는 **TCP 3031** ([포트 안내](ports.md))

## 설치 (요약)

```powershell
winget install OpenJS.NodeJS.LTS
# backend 릴리스의 jam-relay-<버전>.zip 을 받아 풀고, 관리자 PowerShell 에서:
powershell -ExecutionPolicy Bypass -File windows\install.ps1
```

- `C:\JamRelay` 에 설치, 부팅 때 자동 시작 (작업 스케줄러, LOCAL SERVICE 계정)
- 방화벽 TCP 3031 열림
- 앱에는 `서버주소:3031` 입력

자세한 옵션(포트 변경, TLS/wss, 제거)은 backend 저장소 README 를 보세요.

## 자동 업데이트

`Jam Relay Updater` 작업이 6시간마다 (그리고 부팅 10분 뒤) backend 저장소의 최신 릴리스를 확인해요.

1. 새 버전이 있으면 `jam-relay-<버전>.zip` 과 `.sig` 를 받아요
2. **지금 설치된 버전 안의 공개키로 서명을 확인**해요. 배포 비밀키로 서명된 게 아니면 설치하지 않아요
   - GitHub 계정이 털려도, 배포 비밀키 없이는 서버에 가짜 업데이트를 넣을 수 없어요
   - 버전 번호까지 묶어 서명해서, 옛날 파일을 새 버전인 척 올려도 통과하지 못해요
3. 새 버전을 잠깐 다른 포트로 띄워서 응답하는지 **자체 점검**
4. 통과하면 교체 표시(`current.txt`)를 바꿔요
5. 중계 서버는 **진행 중인 Jam 이 없을 때** 스스로 재시작하면서 새 버전으로 바뀌어요 (Jam 중에는 끊지 않아요. 6시간 넘게 계속 바쁘면 교체 — 호스트 앱은 자동으로 다시 연결)
6. 새 버전이 응답하지 않으면 **이전 버전으로 되돌리고**, 그 버전은 다시 시도하지 않아요

| 확인할 것 | 위치 |
| --- | --- |
| 업데이트 기록 | `C:\JamRelay\logs\updater.log` |
| 중계 서버 기록 | `C:\JamRelay\logs\relay.log` |
| 지금 버전 | `http://127.0.0.1:3031/version` 또는 `C:\JamRelay\current.txt` |
| 끄기 | `C:\JamRelay\config.json` 에서 `"autoUpdate": false` |
| 지금 바로 확인 | 작업 스케줄러에서 `Jam Relay Updater` 실행 |

## 중계 서버가 알 수 있는 것 / 없는 것

| 알 수 있음 | 알 수 없음 |
| --- | --- |
| 접속한 IP, 접속 시간, 데이터 양 | 보는 페이지 주소·제목, 채팅, 이름, 커서·화면 위치, 초대 코드의 비밀키 |

중계 서버는 아무것도 저장하지 않고, 로그에는 시간과 IP 앞부분만 남겨요.

## 운영할 때 참고

- 친구끼리 무료로 쓰면 보통 문제없어요. 불특정 다수에게 서비스로 열거나 돈을 받는다면 전기통신사업법상 부가통신사업 신고 대상인지 확인하세요.
- 한 Jam 최대 16명, IP 당 동시 연결 30개 제한이 있어요.
