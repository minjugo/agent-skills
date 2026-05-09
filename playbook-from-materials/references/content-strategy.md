# 콘텐츠 전략 — Pillar 도출, 4-focus 구조, hedging

이 문서는 `playbook-from-materials` 스킬이 자료를 읽고 콘텐츠를 정리할 때 따르는 사고 프레임이다.

---

## 1. Pillar 도출 — 무엇을 기준으로 묶는가

좋은 Playbook의 pillar는 **"서로 다른 학습 단위"**여야 한다. 같은 층위의 다른 측면을 묶지 말 것.

### 좋은 Pillar 묶음 (예시)

- "Harness Engineering / Skill / Hermes Agent" — 추상도가 다름 (원리 → 도구 → 사례)
- "기초 / 핵심 도구 / 운영" — 학습 깊이가 다름
- "What / How / Pitfalls" — 사고의 layer가 다름

### 나쁜 Pillar 묶음

- "프롬프트 / 시스템 프롬프트 / 컨텍스트" — 같은 층위에서 비슷한 측면 (한 pillar로 합쳐야 함)
- "도구 A / 도구 B / 도구 C" — 같은 층위 동일 측면 (한 pillar의 sub-sections으로 다뤄야 함)

### 판단 휴리스틱

자료를 다 읽고 나서 다음 질문을 해보고 답을 찾는다:
- 각 pillar를 한 사람이 따로 가르칠 수 있는가? (Yes면 좋은 분리)
- 한 pillar 끝에서 다음 pillar로 넘어갈 때 사고가 한 단계 진화하는가? (Yes면 좋은 분리)
- 두 pillar의 내용이 30% 이상 겹치는가? (Yes면 합쳐야 함)

자연스럽게 안 나오면 사용자에게 분류 의견을 묻는다.

---

## 2. Sub-section 구조 — 4가지 초점

각 pillar 안의 sub-section은 다음 4가지 초점 중 **하나에 강하게 집중**한다. 한 섹션에서 모든 걸 다루려 하면 깊이가 얕아진다.

### Focus 1 · 개념 이해 (Concept)

- 그것이 무엇인가
- 왜 지금 필요한가 (패러다임 변화 / 시장 신호)
- 한 줄 정의

### Focus 2 · 구성 요소 (Components)

- 무엇으로 이루어져 있는가
- 각 구성 요소의 역할
- 표 / 카드 그리드 / 다이어그램으로 시각화

### Focus 3 · 어떻게 작동 (How it works)

- 구성 요소가 어떻게 맞물려 돌아가는가
- step-by-step 흐름
- 4-단계 루프 / 호출 순서 / 데이터 흐름

### Focus 4 · 주의사항 (Caution)

- 흔한 안티패턴
- 빠지기 쉬운 함정
- 트레이드오프
- callout--warn / callout--danger 활용

### 한 pillar의 추천 흐름

```
[1] 개념 → [2] 구성 → [3] 작동 → [4] 주의 → [5] 활동
```

이 다섯 sub-section이 한 pillar의 균형 잡힌 학습 단위가 된다. 자료가 많으면 [2]나 [3]을 둘로 쪼개도 된다 (최대 8 sub-section).

---

## 3. 각 sub-section 내부 — 4-section 구조 (Anthropic Mastery Playbook 차용)

활동 섹션을 제외한 모든 개념 sub-section은 다음 4부 구조를 따른다.

### A. 핵심 원리 및 개념 (Core Principles & Concepts)

- 정의·핵심 개념을 bullet이나 표로
- 추상적인 일반론보다 자료에서 추출한 구체 정의 우선

### B. 실전 활용 가이드 (Practical Implementation Guide)

- 어떻게 자기 업무에 적용할지의 step·패턴·예시
- 코드 블록·표·step 카드 활용

### C. 전략적 인사이트 및 주의사항 (Strategic Insights & Considerations)

- 흔한 안티패턴 / 빠지기 쉬운 함정
- callout(--tip / --warn / --danger)으로 강조

### D. Key Summary

- 그 sub-section의 take-away를 한 문장으로
- 굵은 글씨 + 인용 마크 한 자리 + 강한 단언

---

## 4. Hedging — 권위 수준에 따라 어조 바꾸기

| 권위 수준 | 어조 | 예시 |
|---|---|---|
| **공식 표준** (Anthropic Claude Code docs, OpenAI Agents SDK, ICLR 논문) | 단정형 | "Claude Code의 Skill은 frontmatter와 Markdown 본문으로 구성됩니다." |
| **벤더 가이드** (LangChain 블로그, Anthropic Cookbook 외) | 인용 | "LangChain은 Agent = Model + Harness로 정의한다." |
| **개인 커뮤니티** (GitHub README, 개인 블로그, 발표자 한 명의 의견) | hedging | "이 프로젝트가 자체 문서화한 값으로 — **공식 표준이 아닙니다.** 버전에 따라 변할 수 있으므로 도입 시 최신 공식 문서를 확인하세요." |
| **자체 정리 / 추정** | 명시 | "강사 자체 추정", "본 Playbook의 자체 정리" |

### Hedging callout 템플릿

비공식 개념·구체 수치를 인용할 때 sub-section 안에 다음 callout을 둔다:

```html
<div class="callout callout--note">
  <div class="callout-body">
    <p><strong>중요한 hedging</strong> — [구체 수치/모듈 수/구조 등]은 [출처]가 자체 문서화한 값이지, 업계 표준이나 공인 벤치마크가 아닙니다. 프로젝트 버전에 따라 변할 수 있으므로 도입 검토 시 <strong>최신 공식 문서를 직접 확인</strong>하세요.</p>
  </div>
</div>
```

### 출처 표기

- 자료에서 가져온 그림·표·인용은 sub-section 끝이나 callout 안에 출처를 명시
- 형식: `(출처: [자료명] [페이지 / 섹션])` — 예: `(출처: 클로드코드 사용 가이드 p47–48)`

---

## 5. 톤 다운 — 깊이 vs 폭

처음 만든 Playbook이 너무 깊으면 학습자가 압도된다. 톤 다운 가이드:

| 신호 | 처리 |
|---|---|
| 한 sub-section이 600 단어 초과 | 둘로 나누거나 표로 압축 |
| 한 표가 7행 초과 | 핵심 행만 남기고 나머지는 본문으로 |
| 한 페이지에 metric / 지표 / 항목이 10개 이상 | 카테고리로 묶기 (예: "5층 Eval Set" → "smoke / golden / production replay 3층") |
| 깊이 있는 frame이 표면 학습자에게 과한가? | 별도 "심화편" 부록으로 옮기거나 callout 안에 압축 |

**원칙**: 한 학습자가 30분 안에 한 pillar를 완독할 수 있어야 한다. 못하면 톤 다운.

---

## 6. 활동 섹션 — 채워서 쓰는 템플릿

각 pillar 끝의 [활동] 섹션은 다음 원칙을 따른다.

- **사용자가 직접 채울 markdown 템플릿** 제공 — 추상 개념을 자기 업무에 적용해보게 만듦
- 각 코드 블록은 **우상단 "복사" 버튼**이 있어야 함
- 한 활동에 2~4개의 step (Step 1: 분석, Step 2: 작성, Step 3: 검증)
- 마지막에 **callout--tip**으로 권장 워크플로 안내

활동 템플릿 예시:

```markdown
# [활동 제목]

## Step 1 — 현재 상태 분석
- 항목 1:
- 항목 2:
- 항목 3:

## Step 2 — 갭 식별
- 가장 약한 부분:
- 우선 보강할 것:

## Step 3 — 다음 1주일 행동
- 첫 1–3시간 안에 할 일:
- 어떻게 좋아졌다는 걸 확인할 것인가:
```

---

## 7. 이미지 플레이스홀더 가이드

각 sub-section마다 다음 자리에 placeholder를 둔다:

### 어디에 둘까

- 개념 sub-section: `figure--illustration` (다이어그램 / 진화 타임라인 / 4-quadrant 도식)
- 구성 요소 sub-section: `figure--illustration` (구성요소 그리드 / 매핑 다이어그램)
- 작동 sub-section: `figure--illustration` (4단계 루프 / step-by-step 흐름)
- 도구 사례 / 사례 사례: `figure--screenshot` (실제 화면 캡처가 들어갈 자리)
- 활동 섹션: `figure--screenshot` (잘 채워진 예시가 들어갈 자리)

### 무엇을 적을까

각 placeholder의 `<p class="figure-spec">` 안에 3가지를 명시:

1. **들어갈 이미지** — 매우 구체적으로 묘사
   - ✅ "6대 구성요소 카드 그리드 (2×3). 각 카드에 번호 + 이름 + 한 줄 정의. 클로드코드 가이드 p40 기반"
   - ❌ "다이어그램"
2. **의도** — 왜 여기에 이 이미지가 필요한가
3. **권장 비율** — 16:9 / 4:3 / 정사각형

### 자동 이미지 생성 안 함

이 Skill은 placeholder만 둔다. 실제 이미지는 사용자가 디자이너에게 의뢰하거나 직접 캡처해서 채운다.

---

## 8. 자료 부족할 때

자료가 한 pillar를 충분히 커버 못 할 때:

- 자료에 있는 만큼만 sub-section 만들고 나머지는 비워둠
- callout--note로 "이 부분은 자료가 부족합니다. 추가 자료 후 보강 권장"
- 사용자에게 보고: "X pillar는 자료가 빈약해 sub-section 2개로 줄였습니다. 추가 자료 있으면 보강 가능합니다."

---

## 9. 자료 충돌할 때

여러 자료가 같은 개념에 다른 정의/수치를 줄 때:

- callout--note로 충돌을 명시
- 1순위 자료의 정의를 본문, 다른 자료의 정의를 callout으로 보조
- 사용자에게 보고: "X 개념에서 [자료A]와 [자료B]가 충돌합니다. [자료A]를 1순위로 채택했는데 다른 선택 원하시면 알려주세요."
