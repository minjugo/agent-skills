# Hermes Agent — 실사용 튜토리얼 인사이트

> Task 2-B (Tutorial Researcher) 산출물
> 생성: 2026-05-02T00:00:00+09:00
> 조사한 글 수: 13개 시도 / 11개 유효 자료 확보 (한국어 2 / 영어 9)
> 조사 대상: DataCamp, Braincuber, Lushbinary, HermesAtlas, BSWEN, Botlearn, EvoMap, AMD, Wikidocs(KO), Medium(@sathishkraju), GitHub mudrii, Felo(KO), Xugj520(검색 인용), LM Studio integrations(검색 인용)

---

## 1. 따라하기 단계 매트릭스

| 단계 | 공식 가이드 | 실제 (블로거 보고) | 차이 / 주의 |
|---|---|---|---|
| **0. 사전 OS** | "Linux/macOS/WSL2 지원" | Windows 네이티브 시도 → 무한 실패 후 WSL2로 우회 [Botlearn #1] | 공식은 한 줄로 적었지만 실제는 BIOS 가상화/Hyper-V 활성화·커널 업데이트가 추가 필요 [Botlearn #2] |
| **1. 설치 (curl one-liner)** | `curl … install.sh \| bash` 1줄 | 동일하지만 GitHub raw 차단/프록시·SSH 22 차단으로 `curl: (28)` 빈발 [BSWEN #2, Xugj520] | 공식 미명시: `GITHUB_TOKEN` 미리 export 또는 HTTPS clone fallback 필요 [HermesAtlas] |
| **2. PATH 적용** | "셸을 다시 열어주세요" | `hermes: command not found` — 가장 흔한 단일 불만 [EvoMap #2, BSWEN #1] | 설치 성공해도 현 셸은 PATH 모름 → `source ~/.bashrc`/`~/.zshrc` 또는 새 터미널 필수 |
| **3. Python 3.11 확보** | "auto-installed" | 시스템 Python 3.13/3.10가 PATH에 우선 → `tiktoken pyo3 error` / `pathlib` 호환 깨짐 [Xugj520, BSWEN #3, Botlearn #4] | uv가 격리 venv를 만들어주지만 시스템 Python이 먼저 잡히면 실패. 3.11 또는 3.12 권장 |
| **4. 의존도구 (Node 22, ripgrep, ffmpeg)** | "자동 설치" | Apple Silicon에서 `eval "$(/opt/homebrew/bin/brew shellenv)"` 누락 시 ffmpeg/ripgrep 못 찾음 [HermesAtlas] | 공식 설치 스크립트가 Homebrew 경로를 zshrc에 미주입 → 수동 추가 |
| **5. 첫 모델 등록 (`hermes model` / `hermes config set`)** | "API 키만 넣으면 5분" | `Provider error: invalid API key` — 90%가 키 끝 공백 [HermesAtlas]. 또는 OpenRouter region/credits 문제 [Botlearn #7] | 우회: `hermes config set model.api_key "$(cat)"` + Ctrl-D heredoc로 공백 제거 |
| **6. 컨텍스트 윈도 64K 확보** | "OpenRouter 추천" | 64,000 토큰 미만 모델은 startup에서 거절됨 [DataCamp(검색), Braincuber(검색)] | Ollama 로컬 모델은 `OLLAMA_CONTEXT_LENGTH=32768`(또는 더 크게) 미설정 시 컷 |
| **7. `hermes doctor`** | 권장 | 거의 모든 트러블슈팅의 1순위 — "이거 먼저 안 해서 시간 낭비" [EvoMap, BSWEN, HermesAtlas] | 공식이 약하게 권하지만 실전에서는 *항상 먼저* 돌려야 함 |
| **8. 첫 실행 (`hermes`)** | "그냥 실행" | "Connection reset by peer" — Base URL의 `/v1` 누락이 주범 [Botlearn #6, AMD blog] | OpenAI-compatible endpoint는 거의 모두 `/v1` 필요 (LM Studio도 마찬가지) [AMD] |
| **9. 게이트웨이 (Telegram 등)** | `hermes gateway setup` | Telegram bot이 응답 무. 토큰 오타 또는 GATEWAY_HEARTBEAT 미설정 → "사일런트 다운" [BSWEN #6, Xugj520] | `.env`에 `GATEWAY_HEARTBEAT=true` 추가하면 크래시 시 IM에 "service offline" 자동 통지 |
| **10. 스킬 설치** | 마켓플레이스 한 줄 | OpenClaw에서 마이그레이션 시 self-learning은 *기본 비활성* — 모르고 그냥 쓰면 단발 에이전트와 동일 [Medium @sathishkraju] | `persistent memory` + `skill_generation` 명시적 enable 필요 |
| **11. 업데이트 (`hermes update`)** | 한 줄 | `error: Your local changes would be overwritten by merge` [BSWEN #8] | 우회: `git stash && hermes update && git stash pop` |
| **12. 정리** | 별도 안내 없음 | 수 주 사용 후 메모리 누수 — 세션·스킬 파일 누적 [BSWEN #7] | `hermes sessions clean --before YYYY-MM-DD` 주기적으로 |

---

## 2. 자주 막히는 포인트 TOP 10

### Top 1: `hermes: command not found` (PATH 미반영)
**빈도**: 4개 글에서 발견 (EvoMap, BSWEN, HermesAtlas, 검색 결과 다수)
**증상**: 설치 스크립트는 "success"라고 했지만 새 명령이 인식되지 않음
**원인**: 설치 스크립트가 `~/.local/bin`을 rc 파일에 추가했지만 현재 셸이 reload되지 않음. EvoMap이 "single most common Hermes complaint, and it isn't really a Hermes bug, it's a shell behaviour"로 표현.
**우회법**:
1. `source ~/.bashrc` 또는 `source ~/.zshrc` [BSWEN #1]
2. 새 터미널 창을 열어 다시 시도 [HermesAtlas]
3. 그래도 안 되면 `~/.local/bin`이 PATH에 있는지 직접 확인 [HermesAtlas]

### Top 2: `Provider error: invalid API key` (실제론 키 끝 공백)
**빈도**: 3개 글 (HermesAtlas, BSWEN #4, Botlearn #7)
**증상**: 키를 분명히 맞게 붙였는데도 invalid
**원인**: 클립보드 복사 시 trailing whitespace/줄바꿈 — HermesAtlas는 "trailing whitespace (90% of cases)"로 단정
**우회법**:
1. `hermes config set model.api_key "$(cat)"` 입력 후 키 붙여넣고 Ctrl-D [HermesAtlas]
2. 다른 프로바이더의 키 prefix(예: `sk-`, `sk-or-`) 헷갈림 점검 [BSWEN #4]
3. OpenRouter는 region/credits 잔액도 같이 확인 [Botlearn #7]

### Top 3: Python 3.13에서 `tiktoken pyo3` / `pathlib` 호환 깨짐
**빈도**: 3개 글 (BSWEN #3, Botlearn #4, Xugj520 검색 인용)
**증상**: 런타임 크래시. `tiktoken pyo3 error` 또는 pathlib 관련 traceback
**원인**: 의존성 생태계가 3.13 미준비. 공식은 3.11/3.12 권장
**우회법**:
1. uv를 통한 격리 venv가 정상 동작하는지 확인 (시스템 Python이 먼저 잡혀선 안 됨) [BSWEN #3]
2. 명시적으로 3.11 사용: `uv venv venv --python 3.11` [HermesAtlas 매뉴얼 설치]

### Top 4: Windows 네이티브에서 무한 실패 → WSL2로 우회
**빈도**: 2개 글 (Botlearn #1, HermesAtlas)
**증상**: cmd/PowerShell에서 설치 시도 시 `bad interpreter` / `cannot execute`
**원인**: "Windows native is not supported" — 모든 워크로드는 Unix userland에서만 동작 [GitHub mudrii]
**우회법**:
1. PowerShell(관리자)에서 `wsl --install` → 재부팅 → Ubuntu 터미널 안에서 설치 [HermesAtlas]
2. WSL이 또 실패하면 BIOS 가상화 활성화 + Windows Features 점검 [Botlearn #2]

### Top 5: GitHub 차단/프록시로 설치 스크립트 timeout
**빈도**: 3개 글 (BSWEN #2, Botlearn #3, Xugj520)
**증상**: > "curl: (28) Failed to connect to raw.githubusercontent.com" [BSWEN #2]
**원인**: ISP/방화벽이 GitHub 22번 또는 raw 도메인 차단. "GitHub's SSH port (22) may be blocked by ISPs or firewalls" [Xugj520]
**우회법**:
1. HTTPS clone path로 전환, 프록시 환경변수 정렬 [Botlearn #3]
2. Git 미러 사용 또는 수동 clone 후 로컬 설치 [BSWEN #2]
3. `export GITHUB_TOKEN=...` 후 재시도 [HermesAtlas]

### Top 6: Gateway는 떴는데 Telegram 봇 무응답
**빈도**: 2개 글 (BSWEN #6, Xugj520)
**증상**: `hermes gateway start` 후에도 봇이 답을 안 함. 에러도 없음(사일런트)
**원인**: 봇 토큰 오타 또는 `GATEWAY_HEARTBEAT` 미설정으로 크래시가 가려짐
**우회법**:
1. `~/.hermes/logs/gateway.log` 먼저 확인 [BSWEN #6]
2. `.env`에 `GATEWAY_HEARTBEAT=true` 추가 → 크래시 시 IM에 "service offline" 자동 통지 [Xugj520]
3. `@BotFather`에서 토큰 재발급 후 `hermes gateway setup` 재실행 [BSWEN #6]

### Top 7: `Connection reset by peer` / OpenAI-compatible endpoint
**빈도**: 2개 글 (Botlearn #6, AMD blog)
**증상**: 모델 호출 즉시 reset
**원인**: Base URL 끝에 `/v1` 누락 (OpenAI 호환 엔드포인트 규칙)
**우회법**:
1. LM Studio 로컬은 server address + `/v1` 필수, key는 임의값 "lmstudio" [AMD blog]
2. context length는 비워두면 자동 감지 [AMD blog]

### Top 8: Long context 64K 미충족으로 startup 거절
**빈도**: 2개 글 (DataCamp 인용, Braincuber 인용)
**증상**: 작은 모델 선택 시 시작 자체가 안 됨
**원인**: Hermes Agent는 multi-step tool-calling을 위해 최소 64,000 토큰 컨텍스트 요구
**우회법**:
1. OpenRouter 권장 (200+ 모델 접근) [Braincuber]
2. Ollama 로컬은 `OLLAMA_CONTEXT_LENGTH=32768` 이상으로 미리 설정 후 `ollama serve` [Braincuber]

### Top 9: 자기학습/메모리가 기본 비활성 — "OpenClaw에서 넘어왔는데 그대로네"
**빈도**: 1개 글에서 강조 (Medium @sathishkraju) — 광고 의심 라벨이지만 기술 사실 자체는 중요
**증상**: Hermes의 핵심 기능인 self-improving이 발현 안 됨. 단발 에이전트처럼 보임
**원인**: `persistent memory`, `skill_generation`이 기본 off
**우회법**:
1. config 파일에서 명시적 enable [Medium @sathishkraju]
2. `MEMORY.md`가 빈 채면 명시적으로 "remember this" 지시 [Botlearn #15]

### Top 10: `hermes update` 머지 충돌
**빈도**: 1개 글 (BSWEN #8)
**증상**: > "error: Your local changes would be overwritten by merge"
**원인**: 로컬 git 수정사항이 남아있음
**우회법**:
1. `git stash` → `hermes update` → `git stash pop` [BSWEN #8]

---

## 3. 공식 가이드에 누락된 사전 준비물

| 누락된 준비물 | 왜 필요한가 | 출처 |
|---|---|---|
| `~/.local/bin` PATH 수동 검증 | 설치 스크립트가 rc에 추가해도 셸 reload 안 되면 무용 | [EvoMap, BSWEN #1, HermesAtlas] |
| Apple Silicon에서 `eval "$(/opt/homebrew/bin/brew shellenv)"` | ffmpeg/ripgrep 경로 못 찾음 | [HermesAtlas] |
| Linux glibc 2.35+ | 일부 배포판 충족 안 됨, Alpine은 glibc 부재로 다수 caveats | [HermesAtlas] |
| Windows: BIOS 가상화 + Windows Features WSL | "WSL2를 쓰세요"만으로는 부족 | [Botlearn #2] |
| Python 3.11/3.12 명시적 사용 (3.13 제외) | 의존성 미준비 | [BSWEN #3, Xugj520] |
| `OLLAMA_CONTEXT_LENGTH=32768` env 사전 설정 | 64K 컨텍스트 요구 충족 | [Braincuber 검색, Quickstart 인용] |
| `GATEWAY_HEARTBEAT=true` in `.env` | 사일런트 게이트웨이 크래시 가시화 | [Xugj520] |
| Docker 사용 시 `usermod -aG docker $USER` 후 logout/login | 권한 그룹 즉시 반영 안 됨 | [BSWEN #5] |
| `GITHUB_TOKEN` 환경변수 | 설치 시 대용량 바이너리 다운로드 rate limit 우회 | [HermesAtlas] |
| AMD: Adrenalin Edition 최신 드라이버 + Variable Graphics Memory "High" | Ryzen AI Max+ 메모리 분할 부족 시 모델 로드 실패 | [AMD blog] |
| `persistent memory` + `skill_generation` 명시적 enable | 기본 off라 self-improving 미발현 | [Medium @sathishkraju] |
| OpenAI 호환 엔드포인트 끝 `/v1` | 누락 시 즉시 connection reset | [AMD blog, Botlearn #6] |
| 15+ optional extras (`messaging`, `voice`, `mcp`, `slack`, `matrix` 등) | 각 기능이 별도 extras이며 미설치 시 무에러로 기능만 부재 | [GitHub mudrii] |
| sudo로 설치 금지 | 권한 꼬임 → 재설치 시 `sudo rm -rf ~/.hermes/` 필요 | [HermesAtlas] |

---

## 4. 소요 시간 현실 vs 공식

- **공식**: "5분 내 setup", "one-liner installer"
- **실제 보고**:
  - Lushbinary: "약 5분 (광고적 톤, 검증 미실시)" — 신뢰도 낮음 [Lushbinary]
  - DataCamp: 단계별 시연만 (구체 시간 미보고). troubleshooting을 별도로 가이드해 *실제 5분 초과 가능성*을 우회적으로 인정 [DataCamp]
  - BSWEN: 저자가 "수 시간 디버깅 후" 가이드 작성했다고 명시 [BSWEN]
  - HermesAtlas: 매뉴얼 설치 fallback 단계가 9단계 → 자동 설치 실패 시 30분+ 가능
- **추정 평균**: 자동설치 즉시 성공 시 5–15분 / 첫 막힘 1회 발생 시 30–60분 / Windows에서 WSL부터 잡으면 1–2시간
- **가장 시간 잡아먹는 단계** (다수 일치):
  1. PATH/셸 reload (Top 1)
  2. API key 공백 (Top 2)
  3. Python 버전 충돌 (Top 3)
  4. Gateway 사일런트 디버깅 (Top 6)

---

## 5. 현실 팁 (Pro Tips)

1. **항상 `hermes doctor` 먼저** — 실전 트러블슈팅 1순위. "doing this before anything else saves time" [EvoMap, BSWEN, HermesAtlas]
2. **로그 위치 외워두기** — `~/.hermes/logs/{agent.log, errors.log, gateway.log}`. `hermes logs --follow`로 실시간 tailing. "most mysterious failures show up within seconds" [Xugj520, EvoMap]
3. **API key는 heredoc으로 입력** — `hermes config set model.api_key "$(cat)"` + 붙여넣기 + Ctrl-D. trailing whitespace 90% 잡힘 [HermesAtlas]
4. **OpenRouter를 첫 모델로** — 200+ 모델 접근, key 한 개로 스왑 가능, 64K 컨텍스트 충족 [Braincuber 검색]
5. **`/compress` 명령으로 컨텍스트 직접 압축** — API 비용 폭주 방지. Hermes는 "dual compression and Anthropic's prompt caching"으로 비용 관리 [DataCamp]
6. **세션 정리 주기화** — 수 주 후 메모리 비대 → `hermes sessions clean --before YYYY-MM-DD` [BSWEN #7]
7. **Multi-agent에선 role/memory scope를 *엄격히* 분리** — 안 그러면 chaos [Botlearn #12]
8. **장시간 태스크는 stage checkpoint 명시** — context degradation 방지 [Botlearn #16]
9. **PowerShell에서 한글/UTF-8 paste는 파일로** — 직접 paste는 `'utf-8' codec can't encode characters` 같은 에러 유발. 파일에 쓰고 Hermes가 읽게 [Botlearn #18, Xugj520]
10. **WSL 경로는 표준화** — `/mnt/c/...`로 통일, 안 그러면 read/write 권한 inconsistency [Botlearn #19]
11. **AMD Ryzen AI Max+에선 Variable Graphics Memory를 "High"로 BIOS에서 변경** — 메모리 부족 모델 로드 실패 방지 [AMD blog]
12. **Sub-agent context는 부모와 분리되어 있음** — 잊으면 멀티에이전트 전체가 hallucination [EvoMap 7개 이슈 중 sub-agent context]
13. **OpenClaw에서 넘어왔다면 self-learning을 명시 enable** — 안 하면 Hermes의 차별점이 사라짐 [Medium @sathishkraju]
14. **사용 안 하는 extras는 설치하지 말 것** — 에러 없이 기능만 빠지므로 처음엔 명시적으로 `pip install "hermes-agent[mcp,messaging]"` 식으로 [GitHub mudrii]

---

## 6. 버전 호환성 이슈

| 버전 변화 | 깨진 부분 | 대응법 | 출처 |
|---|---|---|---|
| Python 3.13 사용 | `tiktoken pyo3 error`, pathlib 호환 깨짐, 런타임 크래시 | Python 3.11/3.12 명시 사용. `uv venv venv --python 3.11` | [BSWEN #3, Xugj520, Botlearn #4] |
| Python 3.10 이하 | startup 실패 — `requires-python = ">=3.11"` | 3.11+ 업그레이드 | [GitHub mudrii] |
| Hermes v0.10.0 (`repo_review_v2026.4.16`) | docs와 구현 불일치 (audit 파일이 별도 존재) | 공식 docs 외에 audit/repo_review md 참조 | [GitHub mudrii] |
| Gateway 구버전 NameError on startup | 레거시 config 잔재 | stable 버전 업그레이드 + 레거시 config 삭제 | [Botlearn #24] |
| Lushbinary가 v0.7.0 언급 | 그 외 호환성 경고 없음 | 최신 stable 사용 권장 | [Lushbinary] |
| Node.js 22 미만 | MCP 도구 호환 안 됨 | `brew install node` / `apt install nodejs npm` 후 재설치 | [HermesAtlas] |
| Ollama context 32K 미만 | Hermes 64K 요구 미충족 | `OLLAMA_CONTEXT_LENGTH=32768` 이상 export 후 serve | [Braincuber 검색] |
| ROCm 7.2.1 미만 (AMD Ryzen APU) | APU 가속 미지원 | ROCm 7.2.1+ 설치 | [AMD blog 검색 인용] |

---

## 7. 광고 의심 글 (참고만)

조사한 글 중 홍보성이 강해 사실 인용은 자제한 글:

- **https://lushbinary.com/blog/hermes-agent-developer-guide-setup-skills-self-improving-ai/** — 이유: 실측 부재("Content was rephrased for compliance with licensing restrictions" disclaimer), 컨설팅 CTA("Book a free 30-minute call") 반복, "first self-hosted AI agent with a built-in learning loop" 같은 검증되지 않은 마케팅 문구, 실제 코드/에러 메시지 0건. *참고만 했고 사실로는 인용하지 않음.*
- **https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-…** — 이유: 도입부 드라마틱 프레이밍("110k GitHub stars in ten weeks"), 경쟁자(OpenClaw) 부정적 위치(9 CVEs, 12% malware) 강조, "40% faster" 같은 벤치마크 무근거 수치. *단, "self-learning이 기본 off"라는 구체 기술 팩트만 Top 9에 인용 (다른 소스로 부분 교차검증 가능).*
- **https://felo.ai/ko/blog/best-hermes-agent-skills-2026/** — 이유: 자사(Felo) 스킬을 5개 카테고리에 걸쳐 반복 노출, "스킬은 점점 더 강력해집니다"식 과장. *스킬 목록만 사실로 참고, 평가 인용 안 함.*

---

## 8. 출처 모음 (조사한 모든 글)

| 글 | 작성자/매체 | 언어 | 신뢰도 |
|---|---|---|---|
| [Nous Research Hermes Agent: Setup and Tutorial Guide](https://www.datacamp.com/tutorial/hermes-agent) | DataCamp | en | 높음 |
| [Hermes Agent Setup: Complete Tutorial Guide](https://www.braincuber.com/tutorial/hermes-agent-setup-tutorial) | Braincuber (검색 인용) | en | 중간 (직접 fetch 실패, 검색 요약으로 사실 확인) |
| [Hermes Agent Developer Guide: Setup & Self-Improving AI](https://lushbinary.com/blog/hermes-agent-developer-guide-setup-skills-self-improving-ai/) | Lushbinary | en | 낮음 (광고 의심) |
| [Hermes Agent Install Guide (OS별)](https://hermesatlas.com/guide/install/) | HermesAtlas | en | 높음 (구체 에러표·수동 설치 폴백 풍부) |
| [Hermes Agent Troubleshooting: 8 Common Errors](https://docs.bswen.com/blog/2026-04-21-hermes-agent-troubleshooting-guide/) | BSWEN | en | 높음 (저자 직접 디버깅 경험) |
| [Hermes Agent Troubleshooting Guide: 25 Critical Errors](https://www.xugj520.cn/en/archives/hermes-agent-troubleshooting-errors-guide.html) | Xugj520 / Efficient Coder | en | 높음 (검색 인용으로 부분 확인 — 직접 fetch 실패) |
| [Hermes Agent Troubleshooting Quick Checklist (25항)](https://www.botlearn.ai/insights/hermes-agent-troubleshooting-quick-checklist) | Botlearn | en | 높음 |
| [Hermes Agent Not Working? Fix 7 Common Issues](https://evomap.ai/blog/hermes-agent-troubleshooting-7-common-issues) | EvoMap (검색 인용) | en | 중간 (직접 fetch 403, 검색에서 핵심만 확보) |
| [Run Hermes Agent Locally on AMD Ryzen AI Max+ Processors and Radeon GPUs](https://www.amd.com/en/blogs/2026/run-hermes-agent-locally-on-amd-ryzen-ai-max-processors-and-radeon-gpus.html) | AMD (검색 인용) | en | 높음 (벤더 공식이지만 실측 셋업 단계 구체) |
| [Hermes Agent: 성장하는 AI 에이전트 실전 가이드](https://wikidocs.net/book/19414) | 채찍피티 / WikiDocs | ko | 중간 (목차·구조 위주, 실측 미완 라벨이 명시적으로 붙어있음) |
| [I Switched from OpenClaw to Hermes Agent](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca) | @sathishkraju | en | 낮음 (광고 의심, 1개 사실만 인용) |
| [hermes-agent-docs (비공식 종합)](https://github.com/mudrii/hermes-agent-docs) | mudrii | en | 중간 (저자 코멘트 부족, 메타 정보 위주) |
| [Best Hermes Agent Skills 2026](https://felo.ai/ko/blog/best-hermes-agent-skills-2026/) | Felo | ko | 낮음 (자사 스킬 홍보) |

---

## 9. 메타 — 자체 검증

- [x] **한국어/영어 모두 포함했는가?** — 한국어 2 (Wikidocs, Felo) + 영어 9 = 총 11개 유효
- [x] **모든 사실에 출처 URL이 attach되어 있는가?** — 매트릭스/Top10/팁 항목마다 [출처] 라벨 명시
- [x] **광고 의심 글을 명확히 라벨링했는가?** — 섹션 7에 3개 글 + 사유 명시, 평문 인용 자제
- [x] **"공식과 다른 점"이 매트릭스에 명시되었는가?** — 섹션 1 매트릭스의 3번째 컬럼 전체가 "차이/주의"
- [x] **Top 10 막힘은 실제로 여러 글에서 반복된 것인가?** — Top 1–8은 2개 글 이상 반복. Top 9–10은 1개 글이지만 기술 팩트로 critical하므로 별도 명시 (Top 9는 광고 의심글이라 라벨링)
- [x] **공식 문서 내용 그대로 옮기기 회피?** — 모든 항목은 *"공식이 이렇게 말하지만 실제로는…"* 시점에서 서술
- [x] **커뮤니티 여론·평가 회피?** — "이 도구 좋다/나쁘다" 식 여론은 배제, 막힘·우회법만 추출
- [x] **출처 없는 주장 회피?** — 모든 사실에 출처 라벨
- **제약 사항 (투명 공개)**: 13개 시드 중 4개(Braincuber, Xugj520, EvoMap, AMD)는 직접 WebFetch가 timeout/403/429로 실패. 모두 WebSearch 결과(검색엔진이 본문에서 추출한 발췌)로 핵심 사실만 보강 인용했고, 직접 fetch 성공한 글들과 교차 검증되는 항목 위주로만 사용함.
