---
name: playbook-from-materials
description: 다양한 자료(MD·PDF·PPTX·HTML·URL 등)를 입력받아 좌측 sticky 네비게이션과 4-section 구조(핵심 원리·실전 활용·전략적 인사이트·마스터 한 줄 평)를 가진 자기학습용 HTML Playbook 한 파일로 정리. Upstage 디자인 토큰 + 흰 배경 + 채워서 쓰는 활동 템플릿 + 이미지 플레이스홀더 포함. 사내 교육·신입 온보딩·세미나 후속 자료·자기학습 가이드 만들 때 사용.
allowed-tools: Read, Write, Glob, Grep, Bash, WebFetch
---

# Playbook 생성 절차

너의 임무: 사용자가 준 자료를 읽고, 좌측 sticky 네비게이션을 가진 단일 HTML Playbook 파일로 정리한다. 결과물은 자체완결형(self-contained) — 외부 의존은 Google Fonts CDN 하나만.

이 Skill은 클로드코드 공식 Skill 사양(frontmatter + Markdown + Progressive Disclosure)을 따른다. 본 SKILL.md는 절차의 메뉴판이고, 상세는 `references/`에서 필요할 때만 펼쳐 본다.

---

## 1. 사용자 입력 확인

다음 정보가 모두 있는지 확인:

- **주제** (예: "Harness Engineering", "RAG 시스템 설계")
- **자료 목록** (파일 경로 / URL — 1개 이상)
- **출력 위치** (생략 시 현재 작업 디렉토리에 `[주제]_playbook.html`)
- **1순위 자료 여부** (있으면 그게 가장 권위 있는 콘텐츠로 다뤄짐)

빠진 정보가 있으면 **AskUserQuestion으로 먼저 확인**한다. 절대 추측하지 말 것.

---

## 2. 자료 수집

| 형식 | 처리 방법 |
|---|---|
| `.md` / `.txt` / `.html` | Read로 직접 |
| `.pdf` | Read tool은 페이지 범위 지정 가능 (`pages: "1-20"`). 텍스트 PDF면 일반적으로 OK. 이미지 PDF면 Read가 시각적으로 처리 |
| `.pptx` | `python -m markitdown <파일>` 로 텍스트 추출. 시각 자료는 별도로 thumbnail.py 사용 가능 |
| URL | WebFetch |
| 이미지 | Read (시각적 분석) |

각 자료에서 **무엇을 추출했는지 메모**한다 — 인용 출처에 쓰임. 읽기 도중 권위 수준(공식 / 벤더 / 커뮤니티 / 추정)도 함께 기록.

---

## 3. 콘텐츠 분석 → Pillar 도출

자료를 종합해 **2~4개 메인 pillar**로 나눈다. 각 pillar는 다음 중 하나의 자연스러운 카테고리:

- 개념 / 핵심 원리
- 구성 요소 (구현 단위, 도구, primitive)
- 사례 / 구체 구현체
- 운영 / 거버넌스
- 안티패턴 / 주의사항

판단이 모호하면 **AskUserQuestion으로 확인**한다 — 예: "Pillar를 [A / B / C] 3개로 잡았는데 괜찮나요?"

각 pillar 안에 **4~8개 sub-section**을 둔다. 각 sub-section은 다음 4가지 초점 중 하나에 강하게 집중:

1. **개념 이해** (정의, 패러다임, why now)
2. **구성 요소** (무엇으로 이루어져 있는가)
3. **어떻게 작동** (구성 요소가 어떻게 맞물리는가)
4. **주의사항** (안티패턴, 함정)

각 pillar 마지막 sub-section은 **[활동] 섹션**으로 — 사용자가 직접 채우는 템플릿.

---

## 4. 톤·hedging 판단

각 콘텐츠가 어떤 권위 수준인지 구분해서 어조를 다르게 쓴다.

| 권위 수준 | 어조 | 예시 표현 |
|---|---|---|
| 공식 표준 (Anthropic Claude Code, OpenAI SDK 공식 문서, ICLR 논문) | 단정형 | "Claude Code의 Skill은 ... 입니다" |
| 벤더 가이드 (LangChain 블로그, 회사 내부 공식 가이드) | 인용 | "LangChain은 ...라고 설명한다" |
| 커뮤니티 / 비공식 (개인 블로그, GitHub README, 커뮤니티 프로젝트) | hedging | "**공식 표준이 아닙니다** — 이 프로젝트가 자체 문서화한 값으로 버전에 따라 변할 수 있음" |
| 자체 정리 / 추정 | 명시 | "강사 자체 추정", "본 Playbook의 자체 정리" |

**구체 수치(7개 모듈, 5층 구조 등)는 출처와 함께 표기**한다. 출처가 약하면 hedging callout 추가.

---

## 5. 이미지 플레이스홀더 자리잡기

각 sub-section마다 시각적 요소가 도움될 자리에 **이미지 플레이스홀더**를 둔다. 자동으로 이미지를 만들지 않고, 빈 공간 + 안내문만 넣는다.

두 종류:
- `figure--screenshot` — 📷 SCREENSHOT (실제 툴/플랫폼 화면 캡처가 들어갈 자리)
- `figure--illustration` — 🎨 ILLUSTRATION (개념 다이어그램이 들어갈 자리)

각 placeholder에 다음 3가지를 명시:
- **들어갈 이미지** (구체 묘사)
- **의도** (왜 여기에 이 이미지가 필요한가)
- **권장 비율** (16:9 / 4:3 / 정사각형)

마크업 패턴은 `references/html-patterns.md` 참고.

---

## 6. HTML 생성

`assets/skeleton.html`을 베이스로 사용한다. 모든 CSS와 JS는 그 파일에 이미 들어 있다. 너의 작업은 **콘텐츠 부분만 채우기**:

- `<!-- BRAND -->` 영역 → 주제명
- `<!-- HERO -->` 영역 → 큰 타이틀 + lede
- `<!-- SIDEBAR_NAV -->` 영역 → 좌측 네비
- `<!-- CONTENT_SECTIONS -->` 영역 → Intro + Pillar 1..N + 맺음말

각 sub-section의 HTML은 `references/html-patterns.md`의 패턴을 그대로 따른다 — **새 디자인을 만들지 않는다**. 새 색상·폰트 도입 금지.

상세 디자인 규칙은 `references/design-tokens.md` 참고. 콘텐츠 흐름 가이드는 `references/content-strategy.md` 참고.

---

## 7. 출력

사용자가 지정한 경로 또는 기본 경로(`./[주제]_playbook.html`)에 Write로 저장.
`assets/upstage-education.png`을 출력 HTML과 같은 디렉터리의 `assets/`로 함께 복사. `<img>` `src`는 `assets/upstage-education.png`.

저장 후 다음을 보고:
- 파일 경로 (절대경로)
- 생성된 pillar 수와 sub-section 총 개수
- 이미지 플레이스홀더 개수 (사용자가 채울 자리)
- 인용된 자료 목록과 권위 수준 표시
- 검증 안내: `open <파일>` 로 브라우저에서 열어보고 (1) 좌측 네비 sticky 동작 (2) 활성 섹션 자동 하이라이트 (3) 코드 블록 "복사" 버튼 동작 (4) 1024px 이하 반응형 토글 — 4가지 확인

---

## 8. 품질 체크리스트 (출력 전 자체 검증)

- [ ] 주제와 pillar 구성이 자료의 무게중심을 반영하는가
- [ ] 비공식·커뮤니티 개념이 hedging callout으로 표시됐는가
- [ ] 각 개념 sub-section에 4-section 구조 (핵심 원리 / 실전 활용 / 전략적 인사이트 / 마스터 한 줄 평) 가 들어갔는가 — 활동 섹션 제외
- [ ] [활동] 섹션의 모든 코드 블록 우상단에 `복사` 버튼이 있는가 (`<button class="code-copy">복사</button>`)
- [ ] Upstage 디자인 토큰을 그대로 사용했는가 (CSS 변수 변경 금지)
- [ ] 본문 영역 흰색 배경 (보라/원색 배경 금지)
- [ ] HTML이 self-contained인가 (외부 이미지·JS 의존 없음. Google Fonts만 OK)
- [ ] 좌측 sticky 네비가 모든 sub-section에 매핑되는가 (앵커 ID 일치)
- [ ] 각 pillar에 [활동] 섹션이 있는가
- [ ] 이미지 플레이스홀더가 각 pillar 최소 2개 이상 들어갔는가

---

## 9. 실패 복구

| 실패 | 처리 |
|---|---|
| 자료가 너무 적어 주제를 충분히 커버 못 함 | "현재 자료로는 X 부분이 빈약합니다. 추가 자료 받을 수 있을까요?" |
| 자료끼리 서로 충돌 | 충돌을 callout--note로 명시 + 사용자에게 1순위 자료 확인 |
| PDF 텍스트 추출 실패 (이미지 PDF) | Read tool로 페이지를 시각적으로 분석 |
| Pillar가 자연스럽게 안 나옴 | AskUserQuestion으로 분류 의견 묻기 |
| 출력 HTML이 self-contained 아님 (외부 의존 발견) | 의존을 inline 또는 base64로 임베드, 불가능하면 사용자에게 보고 |
| 한 pillar에 sub-section이 8개 초과 | 분할하거나 압축 — 가독성 우선 |

---

## 10. 참고 파일

| 파일 | 역할 |
|---|---|
| `references/content-strategy.md` | pillar 도출, 4-focus 구조, tone-down/hedging 규칙 |
| `references/design-tokens.md` | Upstage 디자인 토큰 — 색·폰트·간격·금지 사항 |
| `references/html-patterns.md` | 모든 재사용 가능한 HTML 패턴 (4-section card, callout, activity, figure 등) |
| `assets/skeleton.html` | 베이스 HTML — CSS·JS 포함, 콘텐츠 자리만 비어 있음 |
| `assets/upstage-education.png` | 좌상단 로고 — 출력 디렉터리의 `assets/`로 함께 복사 |

---

## 11. 호출 예시

```
/playbook-from-materials

주제: Context Engineering
자료:
- ~/Downloads/papers/long-context.pdf
- ~/Downloads/decks/m02-s01-script.md
- https://www.anthropic.com/news/context-engineering
출력: ~/Desktop/context_engineering_playbook.html
1순위: m02-s01-script.md
```

또는 자연어로:

> "Context Engineering Playbook을 만들고 싶어. /Users/gominju/Downloads/refs/ 안의 PDF와 MD 파일들을 다 보고, m02-s01이 1순위야. 결과는 ~/Desktop에 저장해줘."

---

## 12. 안티패턴 (절대 하지 말 것)

- 새 디자인 컴포넌트를 임의로 추가 (제공된 패턴만 사용)
- 보라(`--ups-purple`)를 배경 전면에 사용 — 액센트 전용
- 본문 가운데 정렬 (좌측 정렬 기본, hero·activity 헤더만 예외)
- 그림자·그라데이션·장식적 밑줄
- 한 단락 5줄 초과 (분할하거나 카드/표로 변환)
- 비공식 정의를 단정형으로 서술
- 이미지 자동 생성 (placeholder만 둠 — 실제 이미지는 사용자가 채움)
- 한 sub-section이 600 단어 초과 (압축하거나 둘로 나눔)
