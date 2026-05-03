---
name: trend-tech-guidebook-builder
description: 트렌드 기술 키워드 하나만 받으면 공식문서·실사용 튜토리얼·커뮤니티 후기를 병렬 조사해 cc101 스타일의 초보자용 한국어 가이드북(스탠드얼론 HTML)을 생성합니다. 키워드 또는 자연어 요청("openclaw 가이드 만들어줘", "/trend-tech-guidebook-builder hermes agent")으로 트리거하세요. 새 AI 도구·라이브러리·프레임워크를 빠르게 따라잡아야 하는 비개발자/주니어/PM에게 적합합니다.
---

# Trend Tech Guidebook Builder

> 트렌드 기술 1개 키워드 → 공식문서·튜토리얼·커뮤니티 3축 병렬 리서치 → cc101 톤의 한국어 가이드북(HTML 1파일) 자동 생성.

---

## 호출 트리거

다음 중 하나면 이 스킬 실행:

- 슬래시 형태: `/trend-tech-guidebook-builder <키워드>` (예: `/trend-tech-guidebook-builder openclaw`)
- 자연어 형태: "X 가이드 만들어줘", "X 트렌드 가이드북 만들어줘", "X 따라잡고 싶어 가이드 줘"
- 한 메시지에 **스킬명 + 트렌드명**이 같이 등장하면 트리거

키워드만 와도 OK. 키워드가 없으면 사용자에게 "어떤 트렌드 기술의 가이드북을 만들까요?" 한 번 묻고 시작.

---

## 출력 정책

- **언어**: 한국어 본문 (디폴트). 기술용어·도구명·명령어·옵션은 영어 그대로 유지 (예: "MCP 서버를 띄우면" / "`claude code init` 명령어").
- **최종 산출물**: `output/{canonical_name}/trend-{canonical_name}-guidebook.html` 1파일. 폰트만 Google Fonts CDN, 나머지(CSS/SVG 아이콘/이미지)는 **인라인 또는 data URI**.
- **다크모드**: 미지원, 라이트 전용.
- **모든 주장에 출처**: 가이드북 안의 핵심 주장·수치·인용은 **반드시 출처 링크**가 같이 붙어있어야 함.

---

## 5단계 오케스트레이션

```
[Task 1: Planner]
   ↓ canonical_name + 검색 쿼리 + URL 후보
   ↓
[★ Plan Gate: 사용자 검토 ★]   ← URL/검색쿼리 부족 시 사용자가 보충
   ↓
[Task 2-A] [Task 2-B] [Task 2-C]   ← 서브에이전트 3개 병렬 spawn
   ↓          ↓          ↓
docs       tutorial   community
_summary   _findings  _sentiment
   └──────────┴──────────┘
              ↓
        [Task 3: Synthesizer]
        cc101 구조·톤 차용, 콘텐츠만
              ↓
         content.json
              ↓
        [Task 4: Scaffolder]
        시맨틱 HTML, 디자인 0%
              ↓
         scaffold.html
              ↓
        [Task 5: Stylist]
        Upstage 토큰 100% 적용
              ↓
   trend-{canonical_name}-guidebook.html
```

---

## Task 1: Planner (키워드 정규화 & 검색 전략)

### 입력
사용자가 던진 자유 키워드 (예: `openclaw 세팅`, `hermes agent 써보고 싶어`)

### 처리

1. **정확한 공식 표기 확인**
   - 키워드를 WebSearch + WebFetch로 1회 조회하여 공식 도메인·표기·로고를 확인.
   - 예: `openclaw` → 정식 표기 `OpenClaw`, 공식 URL `https://open-claw.org` (가상)
   - 공식 URL을 못 찾으면 **사용자에게 후보 2~3개 제시 → 되묻기**.

2. **동음이의 디스앰비기 처리**
   - 검색 결과에 같은 이름의 다른 도구·인물·제품이 섞여 있으면:
     - 가장 트렌디한 후보를 1번으로 추천 (최근 6개월 언급량 기준)
     - 후보 2~3개를 함께 제시
     - 사용자에게 "이 X를 다루면 될까요? 다른 X를 찾으셨나요?" 명시적으로 되묻기
     - 결정되면 가이드북 상단에 "이 가이드는 [공식 표기 + 한 줄 정의]을 다룹니다"를 명시

3. **3축 검색 쿼리 생성** (한국어 + 영어 둘 다)
   - **docs**: `<공식명> documentation`, `<공식명> getting started`, `<공식명> github`, `<공식명> 공식 가이드`
   - **tutorials**: `<공식명> 사용법`, `<공식명> 튜토리얼`, `<공식명> 설치 막힘`, `<공식명> tutorial setup`, `<공식명> review velog`, `<공식명> qiita`
   - **community**: `<공식명> 후기`, `<공식명> 써봤다`, `<공식명> reddit`, `<공식명> hacker news`, `<공식명> X(트위터)`, `<공식명> geeknews`

4. **각 축별 시드 URL 후보 수집** (각 5~8개)
   - 빠른 WebSearch 한 번씩 돌려 후보 URL 목록 작성. 각 URL에는 1줄짜리 "이 페이지는 무엇" 설명 attach.

### 출력 (`output/{canonical_name}/plan.json`)

```json
{
  "canonical_name": "OpenClaw",
  "official_url": "https://open-claw.org",
  "one_liner": "원격 환경에서 안전하게 코드를 실행하는 샌드박스 에이전트 도구",
  "trend_signal": "최근 6개월간 X에서 언급량 +312%, GitHub 스타 1.8k → 8.4k",
  "disambiguation": {
    "candidates": ["OpenClaw (open-claw.org)", "..."],
    "user_confirmed": "OpenClaw (open-claw.org)"
  },
  "search_queries": {
    "docs":       ["...", "..."],
    "tutorials":  ["...", "..."],
    "community":  ["...", "..."]
  },
  "seed_urls": {
    "docs":       [{"url": "...", "what": "..."}, ...],
    "tutorials":  [{"url": "...", "what": "..."}, ...],
    "community":  [{"url": "...", "what": "..."}, ...]
  }
}
```

### ★ Plan Gate (사용자 검토)

**plan.json을 사용자에게 제시**하고 다음을 검토 요청:

```markdown
## 🔍 Task 1 결과 검토

**대상 도구**: OpenClaw (https://open-claw.org)
**한 줄 정의**: ...
**왜 지금 트렌드?**: ...

### 검색 시드 URL (각 축별)

📘 **공식 문서** (5개 후보):
1. https://... — Getting Started 페이지
2. ...

🛠️ **실사용 튜토리얼** (6개 후보):
1. https://velog.io/... — 한국어 설치기, 막힌 포인트 다수
2. ...

💬 **커뮤니티 후기** (5개 후보):
1. https://reddit.com/r/... — "써봤는데 X 부분 별로"
2. ...

이대로 Task 2 리서치 들어갈까요?
부족하거나 추가하고 싶은 URL/관점이 있으면 알려주세요.
```

사용자가 OK하면 Task 2로. 보충 요청 있으면 plan.json 업데이트 후 다시 검토 요청.

---

## Task 2-A/B/C: 3축 병렬 리서치 (서브에이전트)

세 리서처는 **반드시 동시에 spawn** (Agent tool 3개를 한 메시지에서 병렬 호출). 결과 수신 전에는 다음 태스크 진행 금지.

### 호출 패턴

```
Agent(subagent_type="Explore", description="Docs 리서치", prompt=<<subagents/docs-researcher.md 내용 + plan.json 컨텍스트>>)
Agent(subagent_type="Explore", description="Tutorial 리서치", prompt=<<subagents/tutorial-researcher.md 내용 + plan.json 컨텍스트>>)
Agent(subagent_type="Explore", description="Community 리서치", prompt=<<subagents/community-researcher.md 내용 + plan.json 컨텍스트>>)
```

각 prompt 안에는:
- `canonical_name`, `official_url`, 해당 축의 `search_queries`, 해당 축의 `seed_urls`
- 결과 저장 위치: `output/{canonical_name}/{docs_summary|tutorial_findings|community_sentiment}.md`
- 출력 포맷 명세 (각 subagent 파일에 정의)

각 서브에이전트는 **요약본 .md만 메인 컨텍스트로 반환**. 원시 페이지 내용·HTML은 메인으로 가져오지 않음 (컨텍스트 보호).

### 결과 검증

3개 .md 모두 받으면 빠르게 sanity check:
- 각 파일이 비어있지 않은가?
- 출처 링크가 모든 주장에 attach되어 있는가?
- 광고성/홍보 글이 명확히 라벨링되었는가?

문제 있으면 해당 서브에이전트 1개만 재실행. 정상이면 Task 3으로.

---

## Task 3: Content Synthesizer (콘텐츠 합성)

### 입력
- `docs_summary.md` + `tutorial_findings.md` + `community_sentiment.md`
- `assets/cc101-content-structure.md` (필수 참조)

### 처리

1. **cc101의 7블록 구조** 따라 각 블록을 채움:
   - 블록 1 (진입+정의) ← Docs + Community ("왜 지금")
   - 블록 2 (가치 인지) ← Docs + Community
   - 블록 3 (투자) ← Docs (가격·라이센스)
   - 블록 4 (첫 경험) ← Tutorial 위주 + Docs
   - 블록 5 (효율성) ← Docs + Tutorial
   - 블록 6 (문제 해결) ← Tutorial (막힌 곳) + Community (혹평·우회법)
   - 블록 7 (고급화) ← Docs + Community (실전 활용)

2. **필수 7섹션** 0/1/2/5/6/11/17 무조건 포함. 나머지는 자료가 충분한 것만.

3. **콘텐츠 패턴 6종** (A~F) 중 최소 4가지 활용. 특히:
   - 패턴 A 비교표는 블록 1 또는 3에
   - 패턴 B Step-by-step은 블록 4에 필수
   - 패턴 E Q&A는 블록 6에 필수

4. **톤 적용**:
   - 80% 존댓말 + 20% 반말
   - 섹션마다 비유 1~2개
   - 단점·한계 솔직히 명시 (community_sentiment의 혹평 활용)
   - 한 단락 5줄 이내
   - 굵음 강조는 한 단락에 1군데만

5. **출처 attachment**: 모든 주장·수치·인용에 `_sources` 필드로 URL을 메타데이터에 attach. Task 4가 마크업할 때 인용 링크로 변환.

6. **합성 직후 자체 검증** (cc101-content-structure.md §9 체크리스트 9개 모두 PASS).

### 출력 (`output/{canonical_name}/content.json`)

```json
{
  "meta": {
    "canonical_name": "OpenClaw",
    "one_liner": "...",
    "lang": "ko",
    "generated_at": "..."
  },
  "blocks": [
    {
      "block_id": 1,
      "block_title": "진입 결정",
      "sections": [
        {
          "section_id": "00-before-you-start",
          "title": "시작하기 전에",
          "intro_promise": "...",
          "body": [
            { "type": "paragraph", "text": "...", "_sources": ["url1", "url2"] },
            { "type": "callout", "variant": "tip", "text": "..." },
            { "type": "table", "headers": [...], "rows": [...] },
            { "type": "code", "lang": "bash", "code": "..." }
          ],
          "outro_summary": "한 줄 요약: ..."
        }
      ]
    }
  ]
}
```

**중요**: HTML/CSS/디자인은 한 글자도 결정하지 않음. 순수 콘텐츠 구조만.

---

## Task 4: HTML Scaffolder (시맨틱 마크업)

### 입력
- `content.json`
- `templates/scaffold-guide.md` (필수 참조)

### 처리
- 시맨틱 HTML5 마크업 생성. `<header>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>` 적극 활용.
- 각 콘텐츠 타입을 `templates/scaffold-guide.md`에 정의된 클래스명·구조로 변환.
- **CSS 한 줄도 적용 안 함**. `<style>` 태그도 비워둠 (Task 5가 채움).
- **콘텐츠 한 글자도 변형 안 함** (content.json의 텍스트 그대로).
- 출처 링크는 각 주장 끝에 `<a class="source-ref">` 또는 footnote로 표시.

### 출력 (`output/{canonical_name}/scaffold.html`)

디자인 0%, 시맨틱 구조 100% HTML 파일.

---

## Task 5: Brand Stylist (디자인 적용)

### 입력
- `scaffold.html`
- `assets/upstage-design-tokens.md` (필수 참조, **100% 권한**)
- `assets/upstage-design-prompt.md` (보조 참조, 디자인 철학)

### 처리

1. **`<head>`에 Google Fonts CDN 링크** 삽입 (Geist + Geist Mono + Noto Sans KR).
2. **`<style>` 태그 안에 인라인 CSS** 삽입:
   - upstage-design-tokens.md §2의 CSS 변수 전체
   - §4의 컴포넌트 매핑 전부
3. **신택스 하이라이트**: highlight.js 또는 Prism의 CSS만 인라인 (JS 없이 사전 빌드된 HTML로). 색은 upstage-design-tokens.md §2의 `--code-*` 변수 사용.
4. **DOM·콘텐츠 절대 변경 금지**. 클래스 추가는 OK (예: `<table>` → `<table class="...">`).
5. **이미지/아이콘**: 가이드북 안에서 외부 URL 의존 최소화. 아이콘은 SVG inline 또는 텍스트 이모지(💡 ⚠️ 등). 외부 이미지는 base64 data URI로 인라인.

### 출력 (`output/{canonical_name}/trend-{canonical_name}-guidebook.html`)

스탠드얼론 .html 1파일. 인터넷 끊겨도 폰트만 시스템 폴백되고 나머지는 정상 렌더링.

### 자체 검증 (Task 5 직후)

upstage-design-tokens.md §7 체크리스트 13개 모두 PASS.

---

## 디렉토리 구조 & 파일 위치

```
~/skills/trend-tech-guidebook-builder/
├── SKILL.md                          ← 이 파일
├── assets/
│   ├── upstage-design-prompt.md      ← Upstage 공식 브랜드 가이드 (PPT 원본, 디자인 철학 마스터)
│   ├── upstage-design-tokens.md      ← 웹 가이드북용 CSS 변수 + 컴포넌트 매핑
│   └── cc101-content-structure.md    ← 콘텐츠 구조·톤·패턴 청사진
├── subagents/
│   ├── docs-researcher.md            ← Task 2-A 프롬프트 템플릿
│   ├── tutorial-researcher.md        ← Task 2-B 프롬프트 템플릿
│   └── community-researcher.md       ← Task 2-C 프롬프트 템플릿
├── templates/
│   └── scaffold-guide.md             ← Task 4 시맨틱 마크업 가이드
└── output/                            ← 런타임 산출물
    └── {canonical_name}/
        ├── plan.json
        ├── docs_summary.md
        ├── tutorial_findings.md
        ├── community_sentiment.md
        ├── content.json
        ├── scaffold.html
        └── trend-{canonical_name}-guidebook.html
```

---

## 실행 흐름 요약 (Claude의 구체 행동 순서)

1. 사용자 키워드 받음 → `output/{tmp}/` 디렉토리 만들 준비.
2. **Task 1**: WebSearch/WebFetch로 공식명·트렌드 시그널·시드 URL 수집 → `plan.json` 작성.
3. **★ Plan Gate**: plan.json 사용자 검토 받기. OK 떨어질 때까지 대기.
4. canonical_name 확정 후 `output/{canonical_name}/`로 디렉토리 rename.
5. **Task 2-A/B/C**: 한 메시지에서 Agent tool 3개 동시 spawn. 각 서브에이전트는 자기 .md만 작성하고 요약 반환.
6. 3개 결과 검증. 문제 있으면 해당 서브에이전트만 재실행.
7. **Task 3**: cc101 구조 따라 합성. content.json 작성. 자체 검증 9개 PASS.
8. **Task 4**: scaffold.html 생성. 디자인 0%.
9. **Task 5**: Upstage 토큰 100% 적용. 최종 .html 생성. 자체 검증 13개 PASS.
10. 사용자에게 최종 파일 경로 안내. 미리보기 권유 (`open <path>` 또는 preview_start).

---

## 실패·예외 처리

- **공식 URL을 못 찾음**: Task 1에서 사용자에게 후보 제시 → 되묻기. 끝까지 모호하면 "더 구체적인 키워드를 주세요" 요청.
- **검색 결과 부족 (해당 도구가 너무 niche)**: 영문 자료라도 충분하면 진행. 둘 다 부족하면 사용자에게 "지금 자료가 X개뿐인데 그래도 가이드 만들까요? 한 달 뒤 재시도가 더 좋을 수 있습니다" 안내.
- **서브에이전트 1개 실패**: 해당 1개만 재실행. 그래도 실패하면 해당 축은 "자료 미수집" 라벨로 표시하고 합성에서 그 블록은 축약.
- **사용자가 Plan Gate에서 큰 수정 요청**: plan.json 갱신 후 재검토. 수정이 깊으면 Task 1을 처음부터 다시.

---

## 톤 (Claude가 사용자에게 보내는 메시지 스타일)

- 한국어, 존댓말 80%.
- 진행 상황은 짧게 (한 문장씩). 예: "Task 2-A/B/C 병렬 시작합니다", "3개 리서치 완료, 합성 들어갑니다".
- Plan Gate에서는 검토 요청을 명확히 (위 §Task 1 출력 예시 형식).
- 최종 산출물 안내: 파일 경로 + "지금 열어볼까요?" 한 줄.
