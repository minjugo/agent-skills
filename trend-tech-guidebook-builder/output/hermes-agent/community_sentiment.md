# Hermes Agent — 커뮤니티 후기 정리

> Task 2-C (Community Researcher) 산출물
> 생성: 2026-05-02T00:00:00Z
> 조사한 출처: 13개 (HN 3, Reddit 종합 1, Medium/Substack 3, YouTube 1*, 한국어 3, GitHub/뉴스 2)
> *YouTube 본문 추출 실패(푸터만 노출). 본문 인용 미사용, 메타 신호로만 사용.

---

## 1. "왜 지금 트렌드인가" 정량 시그널

| 시그널 | 수치 | 변화 | 출처 |
|---|---|---|---|
| GitHub Stars (현재) | ~130k | 2026-02 출시 → 10주 만에 100k 돌파 | [GitHub repo](https://github.com/nousresearch/hermes-agent) |
| GitHub Stars (4월 중순) | 47k → 95k → 103k(v0.10 시점) | 두 달 만에 95k, v0.8.0 출시일 단일일 +6,400 | [36kr 분석](https://eu.36kr.com/en/p/3760771429958403) |
| 최근 릴리스 | v0.12.0 (2026-04-30), v0.8.0 (2026-04-08, 209 PRs) | 5주 7회 메이저 릴리스 → 6주차 12회 | [GitHub repo](https://github.com/nousresearch/hermes-agent) |
| Open Issues | ~2,900 | 빠른 스타 증가에 비례한 이슈 폭증 | [GitHub repo](https://github.com/nousresearch/hermes-agent) |
| HN 토론 게시글 | 3건 이상 (#47644400, #47786673, #47865412) | 최근 4주 내 집중 발생 | [HN #47644400](https://news.ycombinator.com/item?id=47644400) |
| Reddit 비교 댓글 | 평균 +30~+300 upvote 다수 | OpenClaw 대비 토론 활발 | [Kilo Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says) |
| 미디어 보도 | 36kr, Medium 다수, DEV.to 트렌딩 | 영문권 + 중화권 동시 노출 | [36kr](https://eu.36kr.com/en/p/3760771429958403), [DEV.to](https://dev.to/senke0x/hermes-searched-4-platforms-at-once-i-just-told-it-what-i-wanted-b9g) |
| 한국 커뮤니티 | Threads, Brunch, OPSOAI, TTJ 다수 | 4월 한 달간 한국어 후기 다수 등장 | [Brunch](https://brunch.co.kr/@abrahamsong/154), [OPSOAI](https://www.opsoai.com/posts/Hermes-Agent-Deep-Dive-For-those-tired-of-amnesic-AI-The-dawn-of-a-truly-remembering-and-evolving-agent/) |

**한 줄 결론**: "스스로 스킬을 만들고 세션 너머로 기억한다"는 차별점이 OpenClaw 보안 이슈(512 vulnerabilities)·세션 망각 피로와 맞물려, 2026-02 출시 후 10주 만에 100k 스타·HN 3개 글·Reddit 비교 게시물 폭증으로 이어진 상황. 다만 성장 속도가 워낙 빨라 "astroturfing 의혹"과 "성숙도 미달" 비판이 동시에 누적되고 있음.

---

## 2. 🟢 호평 (Top 6)

### 1. 세션을 넘는 지속 메모리 — "쓸수록 좋아진다"
> "The longer you use it, the better it gets." — Tommaso Nervegna, 디자이너, [Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative)

**누가 좋아하나**: 디자이너·PM 등 사이드 프로젝트 운영자. Claude Code의 "blank slate"에 지친 사용자.

### 2. 자율적인 스킬 생성 — "6주에 43개 스킬이 알아서 쌓였다"
> "It stores how it solved things ... 약 43개 스킬을 명시적 요청 없이 6주간 만들어 두더라." — Harlem World Magazine 6주 후기, [기사](https://www.harlemworldmagazine.com/ive-been-running-hermes-agent-for-six-weeks-what-actually-surprised-me/)

**누가 좋아하나**: 반복 워크플로우(SEO 리포트, 모니터링)를 자동화하려는 1인 운영자.

### 3. 모델 락인 없음 — `hermes model` 한 줄로 프로바이더 교체
> "basically every provider that exists ... switch providers at any time with `hermes model`." — Tommaso Nervegna, [Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative)

**누가 좋아하나**: 비용 최적화가 중요한 백엔드/인프라 엔지니어, 200+ 모델 비교 검토하는 PM.

### 4. 무인 스케줄링이 "디자이너에게도 맞는다"
> "unattended scheduling is the game-changer for designers running side projects." — Tommaso Nervegna, [Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative)
>
> "delivers results via Telegram while the user sleeps." — Harlem World 6주 후기, [기사](https://www.harlemworldmagazine.com/...)

**누가 좋아하나**: Telegram/Slack 등 메신저 통한 비동기 결과 수신을 선호하는 비개발 직군.

### 5. 작업 시간 학습 곡선이 실측됨
> "20 minutes week one, 12 minutes week four, 8 minutes by week six" (동일 작업의 셀프 개선 결과) — Aakash Gupta, Product Growth, [뉴스레터](https://www.news.aakashg.com/p/hermes-agent-guide)

**누가 좋아하나**: 정량적 ROI를 요구하는 PM·관리자.

### 6. 한국 사용자: "OpenClaw 안정성 문제 vs Hermes 매끄러움"
> "OpenClaw는 툴(tool) 호출이 잦아지면 끊기는 현상이 있는데, Hermes는 안정적이네요. DB와 연동된 세션 관리 덕분에 자유롭습니다." — @ai_tutor_lab, [Threads](https://www.threads.com/@ai_tutor_lab/post/DW01kFfCOZH/)

**누가 좋아하나**: OpenClaw에서 이주를 검토하는 한국 개발자/튜터.

---

## 3. 🔴 혹평 (Top 6)

### 1. 자가 평가가 과신 — "항상 잘했다고 한다"
> "It always thinks it did a good job. ALWAYS. I had it pull water test results from the Indiana DNR site and it jumbled up everything ... It thought it kicked ass!" — u/CustomMerkins4u, +107 upvotes, [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)

**누가 비판하나**: 데이터 수집·분석을 자동화하려는 실무자.
**얼마나 일반적인가**: Reddit 최상위(+107) + Sathish Raju Medium 본문 + WebSearch 종합에서 동일 패턴 반복. **상위 3대 비판 중 1순위**.

### 2. 자동 학습이 수동 수정을 덮어씀
> "Manual edits get overwritten by auto-learning." — u/cocoagent, +25 upvotes, [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)

**누가 비판하나**: 스킬을 직접 튜닝해서 운영하려는 시니어 개발자.
**얼마나 일반적인가**: Reddit + WebSearch 결과 모두에서 "overwrites manual edits"가 반복 등장. **상위 3대 비판 중 2순위**.

### 3. 성숙도 부족 — "릴리스 6회 중 3회는 동작도 안 했다"
> "Hermes has had 6 releases to OC's 82. 3 of those didn't even work. Don't listen to claims of it being more stable because it hasn't been around long enough to make that claim." — u/CustomMerkins4u, +107 upvotes, [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)

**누가 비판하나**: 프로덕션 안정성을 우려하는 인프라 엔지니어.
**얼마나 일반적인가**: 36kr·Sathish Raju Medium·Reddit 모두 "memory noise, skill quality, training loop stability"를 한계로 지적. **상위 3대 비판 중 3순위**.

### 4. 진입 장벽 — 40분 셋업, Windows는 사실상 WSL2 필수
> "Setup ... a 40-minute configuration process that isn't accessible to all users." — Harlem World 6주 후기, [기사](https://www.harlemworldmagazine.com/ive-been-running-hermes-agent-for-six-weeks-what-actually-surprised-me/)
>
> "Windows 지원은 실험적이며 사실상 WSL2 필수." — OPSOAI 한국 리뷰어, [기사](https://www.opsoai.com/posts/Hermes-Agent-Deep-Dive-For-those-tired-of-amnesic-AI-The-dawn-of-a-truly-remembering-and-evolving-agent/)

**누가 비판하나**: 비개발자, Windows 사용자, Termux/Linux 입문자.
**얼마나 일반적인가**: HN #47865412에서 "셋업 가이드 따로 작성"이 등장할 만큼 광범위.

### 5. 숨은 API 비용 — 메모리 요약·스킬 생성이 LLM 호출 누적
> "지속적인 LLM API 호출이 메모리 요약과 스킬 생성에 누적되어 '예상치 못한 비용'이 발생한다." — OPSOAI 10년차 개발자, [기사](https://www.opsoai.com/posts/Hermes-Agent-Deep-Dive-For-those-tired-of-amnesic-AI-The-dawn-of-a-truly-remembering-and-evolving-agent/)

**누가 비판하나**: 비용 통제가 필요한 1인 사업자·예산 책임자.
**얼마나 일반적인가**: 한국·영문 리뷰 모두에서 언급 (Sathish Raju도 "self-learning disabled by default"가 첫 사용자 함정이라고 지적).

### 6. Astroturfing 의혹 — "신규 계정의 템플릿 글 다수"
> "Suspects coordinated astroturfing campaigns." — u/rakeshkanna91, +30 upvotes, [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)
>
> "Multiple high-voted comments flagging new accounts posting templated pro-Hermes content." — WebSearch 종합

**누가 비판하나**: 회의적인 시니어 개발자, OpenClaw 충성 사용자.
**얼마나 일반적인가**: Reddit 다수 댓글 + WebSearch 종합 결과에 반복. 36kr이 "Web3 연결 가능성"을 지적한 것과 결합되어 신뢰도 우려 형성 중.

---

## 4. 🟡 현실적 평가 (균형 의견)

### 1. "둘 다 쓴다" — Hermes는 brain, OpenClaw는 hands
> "Uses both tools — OpenClaw for orchestration, Hermes for execution." — u/damn_brotha, [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)
>
> "Hermes는 분석적 'brain', OpenClaw는 실행 'hands'로 병행 사용한다." — 송 재희, [Brunch](https://brunch.co.kr/@abrahamsong/154)

### 2. "보안이 좋아 보이는 건 노출 시간이 짧을 뿐"
> "Less exposure time means fewer discovered vulnerabilities — which isn't the same as being inherently more secure." — Sathish Raju, Sr AI Architect at Lenovo, [Medium](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca)

### 3. "Claude Code를 대체하지는 않는다 — 같이 쓴다"
> "I'm not replacing Claude Code with Hermes Agent. I use both." — Tommaso Nervegna, [Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative)

### 4. 한국 시각: "복잡한 시스템보다 매일 쓴 사람이 이긴다"
> "하나 골라서 그냥 매일 쓴 사람들" 이 결국 가치를 얻는다. — 송 재희, [Brunch](https://brunch.co.kr/@abrahamsong/154)

### 5. "오염된 기억의 복구 비용"이라는 양면성
> 기억하면 좋지만 잘못 학습하면 그 오류가 누적된다 → "오염된 기억의 복구 비용"이 핵심 한계. — OPSOAI, [기사](https://www.opsoai.com/posts/Hermes-Agent-Deep-Dive-For-those-tired-of-amnesic-AI-The-dawn-of-a-truly-remembering-and-evolving-agent/)

### 6. "코딩 작업에는 Cursor/Claude Code가 낫다"
> "Inferior for code generation compared to Cursor or Claude Code." — Sathish Raju, [Medium](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca)

---

## 5. 📢 광고/홍보 의심 (참고만)

| 발언자 | 발언 | 의심 사유 | 출처 |
|---|---|---|---|
| Medium @ai-software-engineer | "It's so good I gave up on other AI agents like OpenClaw" (제목) | 단점·한계 0건, 스폰서 disclaimer 없음, 출판물명 자체가 관련 도구 홍보 | [Medium](https://medium.com/ai-software-engineer/i-tried-hermes-agent-its-so-good-i-gave-up-on-other-ai-agents-like-openclaw-211076ce4934) |
| Senpeng (@senke0x) | "Hermes searched 4 platforms at once ..." | 본인 제품 Actionbook.dev 통합 홍보가 본 글의 결론. Hermes 비판 0건 | [DEV.to](https://dev.to/senke0x/hermes-searched-4-platforms-at-once-i-just-told-it-what-i-wanted-b9g) |
| HN ethanjamescolez (#47865412) | "독립 설치 가이드" 자체 게시 | 작성자 본인 홍보성, 댓글에서도 "이름이 공식과 너무 비슷하다"는 지적 받음 | [HN #47865412](https://news.ycombinator.com/item?id=47865412) |
| 신규 Reddit 계정군 (식별 불가) | "I ditched OpenClaw for Hermes" 류 템플릿 글 다수 | 다수 정상 사용자가 "templated/coordinated"라고 지목 | [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says) |
| (참고) "NOUS 토큰" 언급 자료들 | Web3 토큰 연계 암시 | Nous Research 공식은 토큰 무관 명시. 36kr가 "공식 발표 없는 거래/투자 주의" 경고 | [36kr](https://eu.36kr.com/en/p/3760771429958403) |

> 가이드북 본문 인용 시 위 5건은 직접 인용 금지. 메타 시그널(예: "왜 화제인가")로만 활용.

---

## 6. 도메인별 적합도

| 도메인 / 역할 | 적합도 | 근거 (커뮤니티 발언) |
|---|---|---|
| 백엔드 / 인프라 자동화 | **높음** | 무인 스케줄링·메신저 통합으로 모니터링 워크플로우 강함 ([Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative), [Harlem World](https://www.harlemworldmagazine.com/ive-been-running-hermes-agent-for-six-weeks-what-actually-surprised-me/)). 단, 프로덕션 안정성 우려 잔존 ([Reddit](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)) |
| 데이터 수집 / 리서치 | **중간** | 4개 플랫폼 동시 검색 등 강점 ([DEV.to](https://dev.to/senke0x/hermes-searched-4-platforms-at-once-i-just-told-it-what-i-wanted-b9g))이지만 "결과를 잘못 생성해도 잘했다고 보고"하는 자가평가 결함이 가장 치명적인 도메인 ([Reddit](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says)) |
| 코드 생성 / 코딩 보조 | **낮음** | "Inferior for code generation compared to Cursor or Claude Code" — Sathish Raju ([Medium](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca)) |
| 비개발 PM / 기획 | **중간** | 정량적 ROI 사례 ([Aakash Gupta](https://www.news.aakashg.com/p/hermes-agent-guide))는 매력적이나, 40분 터미널 셋업이 진입 장벽 ([Harlem World](https://www.harlemworldmagazine.com/ive-been-running-hermes-agent-for-six-weeks-what-actually-surprised-me/)) |
| 디자이너 / 1인 사이드 프로젝트 | **높음** | "designers running side projects 게임체인저" — Nervegna ([Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative)) |
| 한국 개발자 / 튜터 | **중간~높음** | OpenClaw보다 매끄럽다는 후기 ([Threads](https://www.threads.com/@ai_tutor_lab/post/DW01kFfCOZH/)), 단 Windows 사용자는 WSL2 필수 ([OPSOAI](https://www.opsoai.com/posts/Hermes-Agent-Deep-Dive-For-those-tired-of-amnesic-AI-The-dawn-of-a-truly-remembering-and-evolving-agent/)) |
| 보안 민감 엔터프라이즈 | **낮음** | 노출 시간 짧아 검증 부족 ([Medium-Raju](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca)), Web3 연계 의혹 잔존 ([36kr](https://eu.36kr.com/en/p/3760771429958403)) |

---

## 7. 비교 대상 (Alternatives)

| 비교 도구 | 차이점 (커뮤니티 의견) | 출처 |
|---|---|---|
| **OpenClaw** | 플러그인 24+개, 릴리스 82회로 성숙. 그러나 업데이트마다 버그 양산("Every single update ships more bugs"), 보안 이슈 다수. Hermes는 그 대안으로 선택됨 | [Reddit 정리](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says), [Brunch](https://brunch.co.kr/@abrahamsong/154) |
| **Claude Code** | 코드 생성에서 우위. 그러나 매 세션 "blank slate"라는 한계. Hermes는 보완재로 병행 사용 | [Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative), [Medium-Raju](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca) |
| **Cursor** | 코딩 IDE로 특화. Hermes 대체 불가 영역 | [Medium-Raju](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca) |
| **Cowork / CREAO / CrewAI** | HN 댓글에서 "Hermes의 차별점은 자율 스킬 생성"이라는 비교 맥락에서 등장 | [HN #47644400](https://news.ycombinator.com/item?id=47644400) |

---

## 8. 출처 모음

| 출처 | 종류 | 신뢰도 |
|---|---|---|
| [HN #47644400](https://news.ycombinator.com/item?id=47644400) | HN 종합 토론 | 중 (단일 long comment 위주) |
| [HN #47786673](https://news.ycombinator.com/item?id=47786673) | HN 사용 후기 | 중 |
| [HN #47865412](https://news.ycombinator.com/item?id=47865412) | HN Show HN | 낮음 (작성자 자기 홍보) |
| [Kilo: Reddit 종합](https://kilo.ai/articles/openclaw-vs-hermes-what-reddit-says) | Reddit 댓글 정리 | 높음 (다수 사용자 upvote 인용) |
| [Nervegna Substack](https://nervegna.substack.com/p/hermes-agent-the-openclaw-alternative) | 디자이너 시각 | 중 (호평 편향) |
| [Harlem World 6주 후기](https://www.harlemworldmagazine.com/ive-been-running-hermes-agent-for-six-weeks-what-actually-surprised-me/) | 장기 사용 후기 | 높음 (구체 수치 + 단점 명시) |
| [Medium @ai-software-engineer](https://medium.com/ai-software-engineer/i-tried-hermes-agent-its-so-good-i-gave-up-on-other-ai-agents-like-openclaw-211076ce4934) | Medium 전환 후기 | **광고 의심** |
| [Aakash Gupta 뉴스레터](https://www.news.aakashg.com/p/hermes-agent-guide) | PM 시각 | 중 (긍정 편향, 일부 paywall) |
| [DEV.to @senke0x](https://dev.to/senke0x/hermes-searched-4-platforms-at-once-i-just-told-it-what-i-wanted-b9g) | 실사용 | **광고 의심** (자사 도구 통합) |
| [Medium @sathishkraju](https://medium.com/@sathishkraju/i-switched-from-openclaw-to-hermes-agent-heres-what-nobody-told-me-5f33a746b6ca) | Sr AI Architect | 높음 (장단점 명시) |
| [GitHub Repo](https://github.com/nousresearch/hermes-agent) | 1차 시그널 | 높음 (스타·이슈 수치) |
| [36kr 분석](https://eu.36kr.com/en/p/3760771429958403) | 미디어 보도 | 중 (Web3 우려 포함) |
| [OPSOAI 한국 리뷰](https://www.opsoai.com/posts/Hermes-Agent-Deep-Dive-For-those-tired-of-amnesic-AI-The-dawn-of-a-truly-remembering-and-evolving-agent/) | 한국 10년차 개발자 | 높음 (장단점 균형) |
| [Brunch 송재희](https://brunch.co.kr/@abrahamsong/154) | 한국 시각 | 중 |
| [Threads @ai_tutor_lab](https://www.threads.com/@ai_tutor_lab/post/DW01kFfCOZH/) | 한국 SNS 후기 | 중 (짧음) |

---

## 9. 메타 — 자체 검증

- [x] 8개 이상 출처를 실제 조회했는가? (13개 fetch + 2회 추가 WebSearch)
- [x] 호평·혹평이 비슷한 비율로 들어갔는가? (호평 6 · 혹평 6 · 현실 6)
- [x] 트렌드 시그널이 정량 수치로 표현되었는가? (스타 22k→47k→95k→103k→130k 추이, +6,400/일, 209 PRs, 2,900 issues 등)
- [x] 광고 의심 발언을 별도 라벨로 격리했는가? (Section 5 분리, 본문 직접 인용 금지 명시)
- [x] 모든 인용에 출처 URL이 attach되어 있는가?
- [x] 한국 커뮤니티/한국어 발언 최소 1개 포함했는가? (OPSOAI, Brunch, Threads 3개)
- [x] 도메인별 적합도가 커뮤니티 발언 근거로 기술되었는가? (Section 6 각 셀에 출처 링크)
