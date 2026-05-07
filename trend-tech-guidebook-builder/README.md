# trend-tech-guidebook-builder

> 트렌드 기술 키워드 하나만 던지면, 공식문서·실사용 튜토리얼·커뮤니티 후기를 병렬 조사해서 **cc101 스타일의 초보자용 한국어 가이드북(스탠드얼론 HTML 1파일)** 을 자동 생성하는 Claude Code Skill.

새 AI 도구·라이브러리를 빠르게 따라잡아야 하는 비개발자/주니어 개발자/PM을 위한 도구입니다. X/Reddit/뉴스레터에서 처음 보는 도구 이름을 던지면, 5단계 파이프라인이 돌아 슬랙·노션에 그대로 던질 수 있는 1파일 HTML 가이드북을 만들어 줍니다.

---

## ✨ 무엇을 만드는가

- **입력**: 자유 키워드 한 줄 (예: `"hermes agent"`, `"openclaw 세팅"`)
- **출력**: `trend-{slug}-guidebook.html` — 인라인 CSS, Google Fonts CDN만 외부, 나머지 0 의존성 단일 HTML 파일
- **콘텐츠**: cc101 7블록 18섹션 구조 — 진입 결정 → 가치 인지 → 투자 → 첫 경험 → 효율성 → 문제 해결 → 고급화
- **톤**: 한국어 80% 존댓말 + 단점·한계 솔직히 명시 + 비유·이모지 적절
- **출처**: 모든 주장에 인라인 출처 번호 + 푸터에 출처 모음 매핑

---

## 🎬 Demo — Hermes Agent 가이드북

이 스킬로 처음 만든 샘플은 [Hermes Agent](https://hermes-agent.nousresearch.com/) (2026년 화제의 자기학습형 AI 에이전트) 가이드북입니다.

- **위치**: `output/hermes-agent/trend-hermes-agent-guidebook.html` (또는 `examples/`로 이동 권장)
- **분량**: 91KB, 18섹션, 출처 27개
- **소스**: 공식 docs 9개 + 실사용 튜토리얼 11개 + 커뮤니티 후기 13개 (HN·Reddit·Brunch·OPSOAI·Threads 등)
- **포함된 디자인 컴포넌트**: 콜아웃 4종, Q&A 박스, Step-by-step, Before/After, Bad vs Good 비교, 비교표, 코드블록 라이트 톤

브라우저에서 `open output/hermes-agent/trend-hermes-agent-guidebook.html` 또는 IDE 미리보기로 확인 가능.

---

## 📐 디렉토리 구조

```
trend-tech-guidebook-builder/
├── SKILL.md                          # 오케스트레이션 메인
├── README.md                         # 이 파일
├── assets/
│   ├── upstage-design-prompt.md      # Upstage 공식 브랜드 가이드 (디자인 철학 마스터)
│   ├── upstage-design-tokens.md      # 웹 가이드북용 CSS 변수 + 컴포넌트 매핑
│   └── cc101-content-structure.md    # 콘텐츠 구조·톤·패턴 청사진
├── subagents/
│   ├── docs-researcher.md            # Task 2-A 프롬프트 템플릿
│   ├── tutorial-researcher.md        # Task 2-B 프롬프트 템플릿
│   └── community-researcher.md       # Task 2-C 프롬프트 템플릿
├── templates/
│   └── scaffold-guide.md             # Task 4 시맨틱 마크업 가이드
└── output/                            # 런타임 산출물 (.gitignore 권장)
    └── {slug}/
        ├── plan.json
        ├── docs_summary.md
        ├── tutorial_findings.md
        ├── community_sentiment.md
        ├── content.json
        ├── scaffold.html
        └── trend-{slug}-guidebook.html
```

---

## 🔄 동작 흐름 (5 Tasks)

```
사용자 키워드
    ↓
[Task 1: Planner]                 정규화 + 디스앰비기 + 3축 시드 URL 수집
    ↓
[★ Plan Gate]                     URL/쿼리를 사용자에게 보여주고 검토
    ↓
[Task 2-A] [Task 2-B] [Task 2-C]  서브에이전트 3개 동시 spawn (병렬)
   docs    tutorial    community
    └──────────┴──────────┘
              ↓
[Task 3: Synthesizer]             cc101 7블록 구조로 콘텐츠 합성
              ↓
[Task 4: Scaffolder]              시맨틱 HTML (디자인 0%)
              ↓
[Task 5: Brand Stylist]           Upstage 디자인 토큰 인라인 적용
              ↓
   trend-{slug}-guidebook.html
```

**핵심 설계 결정**:
- Task 2의 3축 리서치는 **반드시 서브에이전트 병렬** — 메인 컨텍스트에 raw HTML을 안 들임 (토큰 보호)
- Task 1 끝에 **사용자 검토 게이트** — URL 부족·디스앰비기 모호 시 대화로 보완
- Task 3는 **콘텐츠만**, Task 4는 **마크업만**, Task 5는 **디자인만** — 책임 분리
- 최종 산출물은 **단일 HTML 1파일** — Slack/노션 첨부, 사내망 등 어디서든 깨지지 않음

---

## 🚀 설치 (Claude Code Skill로 등록)

```bash
# 옵션 A: 직접 이동
mv trend-tech-guidebook-builder ~/.claude/skills/

# 옵션 B: 심볼릭 링크 (원본 위치 유지하고 싶을 때)
mkdir -p ~/.claude/skills
ln -s "$(pwd)/trend-tech-guidebook-builder" ~/.claude/skills/

# 옵션 C: 등록 없이 바로 테스트 (이 세션에서 SKILL.md 따라 진행)
# Claude Code에 SKILL.md 내용 inject 후 자연어로 키워드 던지기
```

Claude Code 재시작 후 슬래시 커맨드 또는 자연어로 호출:
- `/trend-tech-guidebook-builder hermes agent`
- "openclaw 가이드북 만들어줘"

---

## 💬 사용법

### 기본 흐름

1. 키워드 던지기 — `/trend-tech-guidebook-builder <키워드>` 또는 자연어
2. **Plan Gate에서 검토** — 시드 URL이 충분한지 확인, 부족하면 보충 요청
3. 서브에이전트 3개 병렬 리서치 (~3-5분)
4. Task 3-5 자동 진행 (~2-3분)
5. `output/{slug}/trend-{slug}-guidebook.html` 생성 → 브라우저에서 확인

### 동음이의 처리

키워드가 모호하면 (예: "Hermes" → Nous Research? 그리스 신? 다른 도구?), Task 1이 **트렌디한 후보를 1순위로 추천 + 다른 후보 2~3개 제시 → 사용자에게 되묻기** → 결정 후 가이드북 상단에 명시.

### 자료 부족 시

특정 도구가 너무 niche해서 자료가 부족하면, Plan Gate에서 알려드립니다. "지금 자료가 N개뿐인데 그래도 만들까요? 한 달 뒤 재시도가 더 좋을 수 있습니다" 식 안내.

---

## 🎨 디자인 시스템

### Upstage Brand Tokens (`assets/upstage-design-tokens.md`)

- **시그니처 컬러**: Ups Purple `#805CFB` (포인트 전용)
- **Primary**: Jacksons Purple 11단계 스케일
- **Typography**: Geist (영문) + Noto Sans KR (한글) — Google Fonts CDN
- **철학**: "Visually Relaxing" — 여백 충분, 한 화면 한 메시지
- **컴포넌트**: 콜아웃 4종(tip/note/warn/danger), Q&A 박스, Step 박스, Before/After 그리드, Bad vs Good 비교 카드, 라이트 코드블록 (Upstage Console 관찰값)

### Content Blueprint (`assets/cc101-content-structure.md`)

[cc101.axwith.com](https://cc101.axwith.com/ko)의 콘텐츠 구조를 추상화한 **7블록 18섹션 청사진** + **재사용 가능한 6가지 콘텐츠 패턴** (비교표 / Step-by-step / Bad vs Good / 설정파일 / Q&A / Before-After).

---

## 🔧 커스터마이즈 — 다른 브랜드로 바꾸기

이 스킬은 Upstage 브랜드를 디폴트로 쓰지만 갈아끼울 수 있습니다.

1. `assets/upstage-design-tokens.md`를 자기 브랜드 버전으로 교체 (CSS 변수 + 컴포넌트 매핑 형식 유지)
2. (선택) `assets/upstage-design-prompt.md`도 자기 브랜드 가이드로 교체
3. SKILL.md의 Task 5 참조 경로만 새 파일명으로 수정

콘텐츠 구조(cc101)도 다른 가이드 사이트로 바꾸려면 `assets/cc101-content-structure.md`를 같은 형식으로 다시 작성.

---

## 🛠️ Tech / 의존성

- **Claude Code** (또는 Claude Agent SDK 기반 환경)
- **도구 사용**: WebSearch, WebFetch, Agent (서브에이전트 병렬 spawn), TodoWrite, Read/Write/Edit/Bash
- **외부 의존성 0** (런타임). 가이드북 출력물은 Google Fonts CDN만 외부 (오프라인 시 시스템 폰트 폴백)
- **Python/Node 등 별도 런타임 없음** — Claude Code 안에서 모두 처리

---

## 📊 성능 / 토큰 비용 (Hermes Agent 샘플 기준)

| Task | 시간 | 토큰 (대략) |
|---|---|---|
| Task 1 (Planner) | 1~2분 | 5K |
| Task 2 (3축 병렬 리서치) | 3~5분 | 서브에이전트당 40-60K, 메인엔 요약만 |
| Task 3 (Synthesizer) | 2분 | 30K |
| Task 4 (Scaffolder) | 1분 | 25K |
| Task 5 (Stylist) | 1분 | 15K |
| **합계** | **약 8~12분** | 메인 컨텍스트 ~80K |

서브에이전트 병렬 spawn으로 메인 컨텍스트가 보호되어, 17섹션 풀 가이드를 만들어도 메인은 여유 있음.

---

## 📜 License

MIT — 자유롭게 fork·수정·재배포 OK.

---

## 🙏 Credits

- **Content blueprint**: [cc101.axwith.com](https://cc101.axwith.com/ko) (Claude Code 101 한국어 가이드)
- **Design system**: Upstage AI 공식 Brand Guide
- **Sample target**: [Hermes Agent](https://hermes-agent.nousresearch.com/) by Nous Research

---

## 🤝 기여 / 피드백

이슈·PR 환영합니다. 특히:
- 다른 브랜드용 design tokens 프리셋 (Linear / Vercel / Notion 스타일 등)
- 영어 출력 모드
- 다크모드 지원
- 신택스 하이라이트 자동 적용 (현재는 색만 정의, 사전 처리 X)

---

_Generated by Claude Code · 2026_
