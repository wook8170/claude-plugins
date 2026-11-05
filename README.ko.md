# wook8170-plugins

[English](README.md) | 한국어

플러그인 세 개를 담은 Claude Code 플러그인 마켓플레이스다. 플러그인은 각자 자기 GitHub 저장소에 있다. 이 저장소에는 목록 파일 `.claude-plugin/marketplace.json` 만 있다.

## 플러그인

### king-wjang-harness

작업 절차를 지키게 하는 훅·CLI 하네스다. 작업을 설계 · 구현 · 출하 세 트랙으로 나누고, 트랙마다 단계를 둔다. 단계는 관문을 통과해야 닫힌다. 설계 트랙이 열려 있는 동안에는 훅이 소스 파일 쓰기를 막는다. 턴 기록을 정리하지 않으면 세션을 끝낼 수 없다. `.harness/` 폴더가 없는 프로젝트에서는 아무 일도 하지 않는다.

저장소: <https://github.com/wook8170/king-wjang-harness>

### lanekeeper (Lanekeeper — Agent Workbench)

여러 Claude Code 에이전트를 레인으로 나눠 병렬로 돌리는 작업대다. 작업마다 모델을 고른다. 동시에 실행하는 레인과 슬롯 수를 제한하고, 위험한 git 조작을 막는다. 세션 사이의 핸드오프와 검토도 맡는다. 안전 훅은 설치하면 바로 켜진다. 작업 훅은 `.lanekeeper/config.json` 이 있는 저장소에서만 동작한다.

저장소: <https://github.com/wook8170/lanekeeper>

Lanekeeper 의 0.1.5 까지 이름은 `wb` 였다. `wb@wook8170-plugins` 를 설치했다면 먼저 `claude plugin uninstall wb@wook8170-plugins` 로 지운다. 그다음 `lanekeeper@wook8170-plugins` 를 설치한다. `~/.wb` 와 `.wb/` 의 설정은 그대로 읽힌다. 옮기고 싶으면 폴더 이름을 `~/.lanekeeper` 와 `.lanekeeper/` 로 바꾼다.

### verifying-production-readiness (VPR)

출하 여부(go/no-go)를 판정하는 스킬이다. 테스트 통과만으로는 판정하지 않고, 직접 관측한 근거로만 판정한다. 결함은 대장에 적고, lint 스크립트가 그 대장을 검사한다. 실측 근거가 없으면 lint 가 「출하 가능」 판정을 거부한다. 조건부 판정에 조건이 적혀 있지 않을 때도 거부한다.

저장소: <https://github.com/wook8170/verifying-production-readiness>

## 세 플러그인의 관계

- 하네스는 단계 규율을 맡는다. 단계마다 무엇을 해도 되는지, 언제 단계를 닫을 수 있는지를 정한다.
- Lanekeeper 는 에이전트를 돌리는 방식을 맡는다. 레인 · 안전 장치 · 품질 점검이 여기에 속한다.
- VPR 은 출하 판정을 맡는다.

하네스와 Lanekeeper 는 출하 판정이 필요할 때 VPR 을 호출한다. 그런데 VPR 은 두 플러그인 어디에도 들어 있지 않으므로 따로 설치해야 한다.

## 설치

먼저 마켓플레이스를 한 번 등록한다.

```bash
claude plugin marketplace add wook8170/claude-plugins
```

그다음 필요한 플러그인을 설치한다. 마켓플레이스 이름은 `marketplace.json` 에 적힌 대로 `wook8170-plugins` 다.

```bash
claude plugin install king-wjang-harness@wook8170-plugins
claude plugin install lanekeeper@wook8170-plugins
claude plugin install verifying-production-readiness@wook8170-plugins
```

세션 안에서는 `/plugin marketplace add wook8170/claude-plugins` 와 `/plugin install <이름>@wook8170-plugins` 로 같은 일을 한다.

목록 파일은 판을 고정하지 않고, 각 항목은 해당 저장소의 기본 브랜치를 가리킨다. 설치되는 판은 그 저장소의 `plugin.json` 에 적힌 값을 따른다. 그래서 작성자가 그 값을 올리면 새 판을 받을 수 있다. 새 판은 `claude plugin update <이름>@wook8170-plugins` 로 받는다.

## 요구 사항

- Node.js 18 이상(하네스와 Lanekeeper 가 Node 스크립트를 실행한다).
- Claude Code 2.1.295 이상. Lanekeeper 의 안전 훅이 이 판을 요구한다. 나머지 두 플러그인은 최소 판을 따로 정하지 않았다.
- VPR 은 `bash` 와 `python3`(표준 라이브러리만)도 쓴다.

## 사용자 매뉴얼

| 플러그인 | 읽을 곳 |
|---|---|
| king-wjang-harness | [README](https://github.com/wook8170/king-wjang-harness/blob/main/README.ko.md) 의 「빠른 시작」 · 「명령 레퍼런스」 절 |
| lanekeeper | [docs/ko](https://github.com/wook8170/lanekeeper/tree/main/docs/ko)(한국어), [docs/en](https://github.com/wook8170/lanekeeper/tree/main/docs/en)(영어) |
| verifying-production-readiness | [README](https://github.com/wook8170/verifying-production-readiness#readme) 와 [스킬 README](https://github.com/wook8170/verifying-production-readiness/tree/main/skills/verifying-production-readiness) |

## 라이선스

이 저장소의 목록 파일은 MIT 라이선스를 따른다. 자세한 내용은 [LICENSE](LICENSE) 를 참고한다. 플러그인마다 자기 저장소에 라이선스가 따로 있고, 세 플러그인 모두 MIT 다.
