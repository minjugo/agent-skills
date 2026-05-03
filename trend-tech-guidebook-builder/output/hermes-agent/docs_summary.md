# Hermes Agent — 공식 문서 요약

> Task 2-A (Docs Researcher) 산출물
> 생성: 2026-05-02T00:00:00Z

---

## 1. 공식 정의

> "An autonomous agent that lives on your server, remembers what it learns, and gets more capable the longer it runs." [출처: https://hermes-agent.nousresearch.com/]

> "The self-improving AI agent built by Nous Research" with "a built-in learning loop — it creates skills from experience, improves them during use" [출처: https://hermes-agent.nousresearch.com/docs/]

공식 태그라인은 "An Agent That Grows With You"이며, 코딩 코파일럿이나 챗봇 API 래퍼와 다른, 사용자 인프라(서버) 위에서 영속적으로 실행되며 메모리·스킬을 누적해 자기개선하는 자율 에이전트로 자기 자신을 정의한다. [출처: https://hermes-agent.nousresearch.com/]

---

## 2. 왜 만들었나 (Origin / Why)

- 코딩 코파일럿/챗봇 래퍼는 영속적인 메모리·자기개선이 없어 매 세션 처음부터 시작해야 한다는 한계를 가진다. Hermes는 이를 극복하기 위해 "사용자 서버에 상주하면서 학습을 누적하는 에이전트"를 지향한다고 명시. [출처: https://hermes-agent.nousresearch.com/]
- "내장 학습 루프(built-in learning loop)"를 통해 경험으로부터 스킬을 만들고, 사용 중 개선하며, 세션 간 지식을 보존하는 것이 핵심 설계 의도. [출처: https://hermes-agent.nousresearch.com/docs/]
- Nous Research가 직접 구축·운영(예: 핵심 컨트리뷰터가 12개 인스턴스를 일상 사용)하며, 백엔드/트레이닝 팀의 모니터링·이슈 조사·데이터셋 작업에도 활용 중. [출처: https://hermes-agent.nousresearch.com/docs/user-stories]

(주의: "self-improving", "the agent that grows with you"는 공식 자기 표현 — 인용부호로 격리)

---

## 3. 아키텍처 / 컴포넌트 개요

| 컴포넌트 | 역할 | 출처 |
|---|---|---|
| Persistent Memory | 세션 간 메모리 보존 + 풀텍스트 검색 리콜 | https://hermes-agent.nousresearch.com/docs/ |
| Skill System | 자동 생성·개선되는 스킬 패키지(툴+프롬프트+설정 번들) | https://hermes-agent.nousresearch.com/docs/getting-started/learning-path |
| Tools (built-in) | 68개 내장 도구(터미널, 파일, 웹 등) | https://hermes-agent.nousresearch.com/docs/ |
| Subagent Delegation | 격리된 대화/실행 환경에서 병렬 서브에이전트 스폰 | https://hermes-agent.nousresearch.com/ |
| Sandboxing Backends | local / Docker / SSH / Singularity / Modal (5종) — 다른 출처는 Daytona 포함 6종으로 표기 | https://hermes-agent.nousresearch.com/ , https://hermes-agent.nousresearch.com/docs/ |
| Messaging Gateway | 단일 게이트웨이로 15+ 메신저(Telegram, Discord, Slack, WhatsApp, Signal, Matrix 등) 연결 | https://hermes-agent.nousresearch.com/docs/ |
| MCP Server Integration | Model Context Protocol 기반 외부 도구/서버 확장 | https://hermes-agent.nousresearch.com/docs/ |
| Cron Scheduler | 자연어 스케줄링/무인 자동화 | https://github.com/NousResearch/hermes-agent |
| TUI / CLI | React/Ink 기반 TUI + 클래식 CLI(v0.11.0에서 재작성) | https://github.com/NousResearch/hermes-agent/releases |
| API Server | OpenAI 호환 `/v1` 엔드포인트 (기본 port 8642, localhost) | https://docs.openwebui.com/getting-started/quick-start/connect-an-agent/hermes-agent/ |

---

## 4. 주요 기능 (Top 5~10)

1. **Persistent Memory & Skills (영속 학습)**: 메모리는 사실을 자동 저장·관련도 기반 회수, 스킬은 단계별 절차를 유사 작업 시 재호출. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]
2. **Self-Improving Learning Loop**: 경험에서 스킬을 만들고 사용 중 개선. v0.12.0의 "Curator"가 스킬 라이브러리를 자동 grade/관리. [출처: https://hermes-agent.nousresearch.com/docs/ , https://github.com/NousResearch/hermes-agent/releases]
3. **15+ 메신저 게이트웨이**: 단일 게이트웨이에서 Telegram/Discord/Slack/WhatsApp/Signal/Matrix/Email/Microsoft Teams 등 통합. [출처: https://hermes-agent.nousresearch.com/docs/ , https://github.com/NousResearch/hermes-agent/releases]
4. **Subagent Parallelization**: 격리된 서브에이전트를 동시 다중 작업에 스폰. [출처: https://hermes-agent.nousresearch.com/]
5. **Multi-Provider 모델 지원**: OpenRouter/Nous Portal/NVIDIA NIM/OpenAI/Anthropic/Gemini/Ollama/vLLM/llama.cpp/SGLang 등 200+ 모델. [출처: https://github.com/NousResearch/hermes-agent , https://hermes-agent.nousresearch.com/docs/reference/faq]
6. **Sandboxing Backends**: local/Docker/SSH/Singularity/Modal/Daytona 등 다중 실행 환경. [출처: https://hermes-agent.nousresearch.com/ , https://hermes-agent.nousresearch.com/docs/]
7. **Cron / Scheduled Automations**: 자연어 스케줄링으로 무인 작업 트리거. [출처: https://github.com/NousResearch/hermes-agent , https://hermes-agent.nousresearch.com/docs/getting-started/learning-path]
8. **68 Built-in Tools + MCP**: 내장 도구 68종에 더해 Model Context Protocol로 확장. [출처: https://hermes-agent.nousresearch.com/docs/]
9. **Voice / Vision / Image Gen / TTS**: 음성 모드, 비전, 이미지 생성, 텍스트-투-스피치 통합. [출처: https://hermes-agent.nousresearch.com/ , https://hermes-agent.nousresearch.com/docs/]
10. **API Server (OpenAI-compatible)**: `/v1` 호환 API 서버로 OpenWebUI 등 외부 UI 연동. [출처: https://docs.openwebui.com/getting-started/quick-start/connect-an-agent/hermes-agent/]

---

## 5. 가격 / 라이선스 / 계정

| 항목 | 값 | 출처 |
|---|---|---|
| 라이선스 | MIT (open source) | https://github.com/NousResearch/hermes-agent |
| 본체 비용 | "Hermes Agent itself is free and open-source (MIT license). You pay only for the LLM API usage from your chosen provider." | https://hermes-agent.nousresearch.com/docs/reference/faq |
| 계정 | 별도 Hermes 계정 불필요. LLM 제공자(OpenAI/Anthropic/OpenRouter 등) API 키만 필요 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| 텔레메트리 | "does not collect telemetry, usage data, or analytics" — 데이터는 `~/.hermes/`에 로컬 저장 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| 최신 버전 | v0.12.0 (2026-04-30, "Curator release") | https://github.com/NousResearch/hermes-agent/releases |

---

## 6. 시스템 요구사항

- **OS**: Linux / macOS / WSL2 / Android(Termux). Windows는 네이티브 미지원 — "Not natively. Hermes Agent requires a Unix-like environment. On Windows, install WSL2 and run Hermes from inside it." [출처: https://hermes-agent.nousresearch.com/docs/reference/faq , https://hermes-agent.nousresearch.com/docs/getting-started/quickstart]
- **Python**: 3.11+ ("Python too old → Requires Python 3.11+"). [출처: https://hermes-agent.nousresearch.com/docs/reference/faq , https://github.com/NousResearch/hermes-agent]
- **모델 컨텍스트**: 최소 64,000 토큰 컨텍스트 모델 필요 — "a model with at least 64,000 tokens of context". [출처: https://hermes-agent.nousresearch.com/docs/getting-started/quickstart]
- **하드웨어**: 명시적 최소 사양은 공식 문서에 없음. 사용자 스토리 기준 $5~$20 VPS, Raspberry Pi 4, 단일 edge GPU(4B Gemma) 등에서 실행 사례 존재. [출처: https://hermes-agent.nousresearch.com/docs/user-stories]
- **의존성**:
  - `uv` 패키지 매니저 (없으면 `curl -LsSf https://astral.sh/uv/install.sh | sh`)
  - 선택: 메신저별 `pip install "hermes-agent[telegram]"` 등 extras
  - 선택: RL 트레이닝용 `atroposlib`, `tinker`
  - Android: `.[all]` 대신 `.[termux]` (ctranslate2 wheel 부재)
  [출처: https://hermes-agent.nousresearch.com/docs/reference/faq , https://github.com/NousResearch/hermes-agent]

---

## 7. 설치 (공식 권장)

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc   # 또는 source ~/.zshrc
```

[출처: https://hermes-agent.nousresearch.com/docs/getting-started/quickstart , https://github.com/NousResearch/hermes-agent]

OS별 차이:
- **Linux / macOS**: 위 스크립트 그대로. 설치 위치는 `~/.local/bin/hermes` (sudo 사용 금지). [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]
- **Windows**: 네이티브 미지원 → WSL2 안에서 위 명령 실행. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]
- **Android(Termux)**: 지원되며 별도 install path 존재. extras는 `.[termux]` 사용. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]
- **WSL**: gateway는 systemd 대신 foreground (`hermes gateway run`) 권장. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]

---

## 8. 인증 / 모델 설정 / API Key

발급 → 설정 흐름:

1. LLM 공급자(OpenAI / Anthropic / OpenRouter / Nous Portal / NVIDIA NIM / Gemini / Ollama / vLLM / llama.cpp / SGLang 등)에서 API 키 발급. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]
2. 인터랙티브 설정 실행:
   ```bash
   hermes model
   ```
   공급자 선택 마법사가 안내한다. [출처: https://hermes-agent.nousresearch.com/docs/getting-started/quickstart]
3. 저장 위치(분리됨):
   - 비밀값 → `~/.hermes/.env`
   - 설정 → `~/.hermes/config.yaml`
   [출처: https://hermes-agent.nousresearch.com/docs/getting-started/quickstart]
4. 검증/문제 해결: `~/.hermes/.env` 키 충돌 확인, 모델명 mismatch는 `hermes model`로 재확인. 컨텍스트 길이는 `config.yaml`의 model 섹션 `context_length`로 수동 지정 가능. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]

---

## 9. 첫 실행 코드 예제 (Hello World)

```bash
# 설치 후 즉시 실행
hermes            # classic CLI
hermes --tui      # modern TUI (recommended)
```

권장 첫 프롬프트(공식 예시):
> "Summarize this repo in 5 bullets and tell me what the main entrypoint is."

세션 재개 검증:
```bash
hermes --continue    # Resume most recent session
hermes -c            # Short form
```

추가 검증 프롬프트(공식 예시):
> "What's my disk usage? Show the top 5 largest directories."

[출처: https://hermes-agent.nousresearch.com/docs/getting-started/quickstart]

Python 라이브러리로도 사용 가능: `AIAgent` 클래스를 import. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]

---

## 10. 명령어 / CLI 레퍼런스

전체 위치(공식 docs 트리): https://hermes-agent.nousresearch.com/docs/  → "Using Hermes (CLI, Features, Messaging Platforms)" 섹션. 머신 판독용 entrypoint: `/llms.txt`, `/llms-full.txt`. [출처: https://hermes-agent.nousresearch.com/docs/]

자주 쓰는 명령(공식 자료에서 확인된 것):

| 명령어 | 동작 | 출처 |
|---|---|---|
| `hermes` | 클래식 CLI 진입 | https://hermes-agent.nousresearch.com/docs/getting-started/quickstart |
| `hermes --tui` | TUI 모드(권장) | https://hermes-agent.nousresearch.com/docs/getting-started/quickstart |
| `hermes --continue` / `hermes -c` | 최근 세션 재개 | https://hermes-agent.nousresearch.com/docs/getting-started/quickstart |
| `hermes model` | 공급자/모델 인터랙티브 설정 | https://hermes-agent.nousresearch.com/docs/getting-started/quickstart |
| `hermes gateway` | 메신저/API 게이트웨이 시작 | https://docs.openwebui.com/getting-started/quick-start/connect-an-agent/hermes-agent/ |
| `hermes gateway run` | 포어그라운드 게이트웨이 (WSL 권장) | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `hermes gateway status` | 게이트웨이 상태 확인 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `hermes gateway install` | 게이트웨이 PATH 재스냅샷(macOS Node/ffmpeg 트러블슈팅) | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `hermes backup` | `~/.hermes/` 전체 zip 백업 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `hermes profile export` | 프로파일 단위 내보내기(자격증명 제거) | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `hermes update` | 글로벌 업데이트 + 모든 프로파일 스킬 동기화 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `/compress` (in-session) | 대화 히스토리 압축으로 토큰 절감 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `/reload-mcp` (in-session) | MCP 서버 재로드 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `/model` (in-session) | 이미 설정된 모델 간 전환 | https://hermes-agent.nousresearch.com/docs/reference/faq |
| `-t <toolset>` | 활성 툴셋 제한(성능 최적화) | https://hermes-agent.nousresearch.com/docs/reference/faq |

---

## 11. 통합 가능 플랫폼

**메시징 (단일 gateway, 15+~19개 플랫폼)**:

| 플랫폼 | 통합 방법 / 비고 | 출처 |
|---|---|---|
| Telegram | `pip install "hermes-agent[telegram]"` + bot token, allowlist | https://hermes-agent.nousresearch.com/docs/reference/faq , https://github.com/NousResearch/hermes-agent |
| Discord | gateway 통한 bot 연결, 채널/스레드별 세션 | https://github.com/NousResearch/hermes-agent , https://hermes-agent.nousresearch.com/docs/reference/faq |
| Slack | gateway 통합, 스레드 기반 세션 네이티브 지원 | https://github.com/NousResearch/hermes-agent , https://hermes-agent.nousresearch.com/docs/reference/faq |
| WhatsApp | gateway 통합 | https://github.com/NousResearch/hermes-agent |
| Signal | gateway 통합 | https://github.com/NousResearch/hermes-agent |
| Matrix | 공식 지원 메신저 목록에 포함 | https://hermes-agent.nousresearch.com/docs/ |
| Email | gateway 통합 | https://github.com/NousResearch/hermes-agent |
| iMessage (BlueBubbles) | v0.9.0에서 추가 | https://github.com/NousResearch/hermes-agent/releases |
| WeChat | v0.9.0에서 추가 | https://github.com/NousResearch/hermes-agent/releases |
| Microsoft Teams | v0.11.0에서 19번째 메신저로 추가 | https://github.com/NousResearch/hermes-agent/releases |
| CLI | 기본 진입점 | https://hermes-agent.nousresearch.com/docs/getting-started/quickstart |

**LLM 제공자**: OpenAI, Anthropic(OpenRouter 경유), OpenRouter, Nous Portal, NVIDIA NIM, Google Gemini, Xiaomi MiMo, Kimi/Moonshot, MiniMax, Hugging Face, Ollama, vLLM, llama.cpp, SGLang, AWS Bedrock(v0.11.0), LM Studio / GMI Cloud / Azure AI Foundry / MiniMax OAuth / Tencent Tokenhub(v0.12.0). [출처: https://hermes-agent.nousresearch.com/docs/reference/faq , https://github.com/NousResearch/hermes-agent , https://github.com/NousResearch/hermes-agent/releases]

**실행 백엔드(sandbox)**: local, Docker, SSH, Singularity, Modal, Daytona. [출처: https://hermes-agent.nousresearch.com/ , https://hermes-agent.nousresearch.com/docs/]

**외부 UI / 인프라**:
- OpenWebUI: 공식 가이드 존재. `API_SERVER_ENABLED=true` + `API_SERVER_KEY` 설정 후 `hermes gateway` 실행 → OpenWebUI Connections에 `http://localhost:8642/v1` 추가 (Docker는 `host.docker.internal`). [출처: https://docs.openwebui.com/getting-started/quick-start/connect-an-agent/hermes-agent/]
- MCP 서버: 임의의 MCP 서버 연결 가능(`tools/list` RPC 응답 필수). [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]

---

## 12. 공식 트러블슈팅 / FAQ

(공식 FAQ 페이지 발췌. 출처: https://hermes-agent.nousresearch.com/docs/reference/faq)

**설치**
- `hermes: command not found` → 셸 프로파일 reload 또는 `~/.local/bin/hermes` 확인.
- Python too old → 3.11+ 필요.
- Missing `node`/`nvm`/`pyenv` → `config.yaml`의 `terminal.shell_init_files`에 셸 init 파일 추가.
- `uv: command not found` → `curl -LsSf https://astral.sh/uv/install.sh | sh`.
- 권한 에러 → sudo 사용 금지. 설치 타깃은 `~/.local/bin`.

**공급자/모델**
- `/model`에 공급자가 적게 보임 → `hermes model`로 추가. `/model`은 이미 설정된 것만 표시.
- API key 미동작 → 키-공급자 매칭 확인, `~/.hermes/.env` 충돌 확인.
- 429 rate limit → 대기/플랜 업그레이드/모델 변경.
- 컨텍스트 길이 초과 → `/compress` 또는 더 큰 컨텍스트 모델로 전환.
- 컨텍스트 자동 감지 오류 → `config.yaml`의 model 섹션 `context_length` 수동 설정.
- 첫 실행 400 에러 → 보통 모델명 오타 또는 키 권한 부족 → `hermes model`로 재확인.

**터미널/실행**
- "Dangerous command blocked" → 의도된 안전 기능. `y` 입력으로 승인.
- 게이트웨이에서 `sudo` 실패 → 비대화형이므로 passwordless sudo 설정 또는 CLI 사용.
- Docker 미연결 → 데몬 실행 + `sudo usermod -aG docker $USER`.

**메시징 게이트웨이**
- 봇 무응답 → `hermes gateway status`, allowlist/토큰 검증.
- 메시지 미전달 → 토큰 검증, `~/.hermes/logs/gateway.log` 확인.
- 게이트웨이 시작 실패 → 플랫폼 의존성 설치 (`pip install "hermes-agent[telegram]"`), 포트 충돌 점검.
- WSL 게이트웨이 단절 → systemd 대신 foreground (`hermes gateway run`).
- macOS에서 Node/ffmpeg 미인식 → `hermes gateway install` 재실행으로 PATH 재스냅샷.

**성능**
- 느린 응답 → 빠른 모델 사용, `-t`로 활성 툴셋 축소.
- 토큰 과다 → `/compress`.
- 세션 너무 긴 경우 → `/compress` 후 새 세션 또는 `hermes chat --continue`.

**MCP**
- 서버 미연결 → 바이너리 경로 확인, npm 서버는 Node.js 필요.
- 도구 미노출 → `tools/list` RPC 응답 확인, config 필터 검토, `/reload-mcp`.
- MCP timeout → 서버 자체 로그 확인.

**프로파일**
- 프로파일 vs `HERMES_HOME`: 프로파일은 디렉토리 구조/셸 alias/스킬 자동 동기화 등 관리 레이어 제공.
- 봇 토큰 공유 불가 — 프로파일별 별도 봇.
- 메모리/세션 격리. `--clone-all`로 기존 프로파일 복제 가능.
- `hermes update`는 글로벌 1회 실행 + 모든 프로파일에 스킬 동기화.

**고급**
- 멀티모델 워크플로 → delegation config로 서브에이전트별 모델 라우팅.
- WhatsApp 단일 번호 다중 에이전트 → 미지원. Telegram/Discord 또는 별도 번호 사용.
- Telegram 로그 숨김 → `display.tool_progress: "off"` 후 게이트웨이 재시작.
- Telegram 슬래시 100개 한도 → `skills.platform_disabled.telegram`으로 비활성.
- 전체 셋업 내보내기 → `hermes backup`(zip), `hermes profile export`(자격증명 제거).

**도움 받기**: GitHub Issues 검색, Nous Research Discord, OS·Python 버전·전체 에러 메시지 포함하여 버그 리포트. [출처: https://hermes-agent.nousresearch.com/docs/reference/faq]

---

## 13. 출처 모음

- [공식 사이트 (메인)](https://hermes-agent.nousresearch.com/)
- [공식 Docs 인덱스](https://hermes-agent.nousresearch.com/docs/)
- [Quickstart](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart)
- [Learning Path](https://hermes-agent.nousresearch.com/docs/getting-started/learning-path)
- [FAQ & Troubleshooting](https://hermes-agent.nousresearch.com/docs/reference/faq)
- [User Stories](https://hermes-agent.nousresearch.com/docs/user-stories)
- [GitHub Repo (README)](https://github.com/NousResearch/hermes-agent)
- [GitHub Releases](https://github.com/NousResearch/hermes-agent/releases)
- [OpenWebUI 통합 가이드](https://docs.openwebui.com/getting-started/quick-start/connect-an-agent/hermes-agent/)
- 머신 판독용 entrypoint: `/llms.txt`, `/llms-full.txt` (https://hermes-agent.nousresearch.com/docs/ 에서 안내)

---

## 14. 메타 — 자체 검증

- [x] 모든 fact에 출처 URL이 attach되어 있는가? — 각 항목 끝에 `[출처: ...]` 또는 표 출처 컬럼으로 표기.
- [x] 수사적 자기 자랑 표현을 인용부호 또는 라벨로 격리했는가? — "self-improving", "the agent that grows with you", "An autonomous agent that lives on your server..." 등은 모두 인용부호로 격리하고 §1, §2 도입에 "공식 자기 표현"임을 명시.
- [x] 1~12 섹션이 모두 채워졌는가? — 12개 섹션 모두 작성. 하드웨어 최소사양은 공식 명시 부재 → §6에 "공식 문서에 없음" 명시.
- [x] 비공식 출처(블로그·커뮤니티)가 섞이지 않았는가? — 출처는 모두 ① hermes-agent.nousresearch.com, ② github.com/NousResearch/hermes-agent, ③ docs.openwebui.com(공식 통합 문서) 3개 도메인으로 한정. 사용자 스토리(§2, §6)도 공식 user-stories 페이지에서만 인용.

검증 결과: **PASS**
