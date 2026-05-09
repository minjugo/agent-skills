# playbook-from-materials

> 다양한 자료(MD·PDF·PPTX·HTML·URL 등)를 받아서 **자기학습용 HTML Playbook 한 파일**로 정리하는 [Claude Code Skill](https://docs.claude.com/en/docs/claude-code/skills).

좌측 sticky 네비게이션 + 4-section 구조(개념·실전·주의사항·한 줄 평) + 채워서 쓰는 활동 템플릿 + Upstage 디자인 토큰. 사내 교육·신입 온보딩·세미나 후속 자료·자기학습 가이드를 만들 때 사용.

---

## 무엇이 해결되나

조직에서 학습 자료를 만들 때 흔한 문제:

- **자료가 흩어져 있음** — PDF, PPTX, MD, 슬랙 링크가 각자 따로
- **포맷이 매번 다름** — 누가 쓰느냐에 따라 깊이·톤·시각이 다름
- **재사용 안 됨** — 한 번 쓰고 끝, 다른 주제로 옮길 때 처음부터 다시

이 Skill은 다음을 해줌:

- 자료 여러 개를 한 번에 읽고 → **2~4개 pillar로 자동 분류**
- 각 pillar에 **4~8 sub-section**, 매 sub-section에 일관된 4-section 구조 적용
- 비공식·커뮤니티 개념은 자동 hedging
- 이미지 자리는 **placeholder만** (디자이너가 나중에 채움)
- 활동 섹션의 모든 코드 블록에 **클립보드 복사 버튼**
- Upstage 디자인 토큰 100% 적용 — 새 색·폰트 도입 안 함

---

## 미리보기

> 실제 generated playbook 화면 캡처 (좌측 네비 + 흰 배경 + 4-section 카드)

생성된 HTML은 다음을 갖춤:

- 좌측 sticky 사이드바 (3 pillar × 4–8 sub-section)
- IntersectionObserver 기반 활성 섹션 자동 하이라이트
- 1024px 이하에서 햄버거 토글
- 코드 블록 우상단 "복사" 버튼
- 단일 HTML 파일 — 외부 의존은 Google Fonts CDN 하나

---

## 설치

이 Skill은 두 가지 위치에 둘 수 있음 — Claude Code 공식 스코프 우선순위에 따라:

### A. Personal — 내 머신 어디서든 호출

```bash
git clone https://github.com/<your-org>/playbook-from-materials.git \
  ~/.claude/skills/playbook-from-materials
```

### B. Project — 이 리포의 팀 멤버 모두 호출

```bash
git clone https://github.com/<your-org>/playbook-from-materials.git \
  ./.claude/skills/playbook-from-materials
git add .claude/skills/playbook-from-materials
git commit -m "Add playbook-from-materials skill"
```

> **추천 동선**: Personal에서 4주 정도 검증 → 안정되면 Project로 승격.

설치 확인:

```bash
ls ~/.claude/skills/playbook-from-materials/
# SKILL.md  README.md  assets/  references/
```

---

## 사용

새 Claude Code 세션에서 자연어 또는 `/명령` 형태로 호출:

### 자연어 트리거

> "Context Engineering Playbook을 만들어줘.
> 자료: ~/Downloads/papers/ 안의 PDF 3개 + ~/Downloads/decks/m02-s01.md.
> 1순위는 m02-s01.md.
> 결과는 ~/Desktop/context_eng_playbook.html에 저장."

Claude가 `description` 매칭으로 이 Skill을 발동하고, SKILL.md의 12단계 절차를 따라감.

### `/명령` 형태 — 더 명시적

```
/playbook-from-materials

주제: RAG 시스템 설계
자료:
- ~/Downloads/papers/long-context.pdf
- ~/Downloads/decks/rag-overview.pptx
- https://www.anthropic.com/news/contextual-retrieval
출력: ~/Desktop/rag_playbook.html
1순위: long-context.pdf
```

### 입력 형식별 처리

| 형식 | 처리 방법 |
|---|---|
| `.md`, `.txt`, `.html` | Read로 직접 |
| `.pdf` | Read tool로 페이지 단위 (텍스트 PDF 자동, 이미지 PDF는 시각 분석) |
| `.pptx` | `python -m markitdown` 으로 텍스트 추출 |
| URL | WebFetch |
| 이미지 | Read (시각적 분석) |

---

## 파일 구조

```
playbook-from-materials/
├── README.md                         # 이 파일
├── SKILL.md                          # 진입점 — frontmatter + 12단계 절차
├── references/                       # Progressive Disclosure — 필요할 때만 로드
│   ├── content-strategy.md           # Pillar 도출, 4-focus 구조, hedging 규칙
│   ├── design-tokens.md              # Upstage 디자인 토큰 매핑
│   └── html-patterns.md              # 재사용 가능한 14개 HTML 패턴
└── assets/
    └── skeleton.html                 # 베이스 HTML — CSS·JS 풀 포함
```

`SKILL.md`는 메뉴판 — Claude가 먼저 읽음. 상세는 `references/`와 `assets/`에서 필요할 때만 로드 (Progressive Disclosure 원리).

---

## 어떤 결과가 나오나

### 자동으로 적용되는 것

- 본문 영역 흰색 배경 (보라 배경 금지 — Upstage Anti-pattern)
- 좌측 280px sticky 네비, 본문 max-width 820px
- Geist + Noto Sans KR 폰트 (Google Fonts preconnect)
- 4-section 구조: **핵심 원리 / 실전 활용 / 전략적 인사이트 / Key Summary**
- 비공식 개념 자동 hedging callout
- 이미지 placeholder (SCREENSHOT / ILLUSTRATION) 각 pillar 최소 2개

### 사용자가 직접 채우는 것

- 이미지 placeholder 자리 → 실제 캡처·일러스트
- 활동 템플릿 → 자기 업무 분석

---

## 디자인 시스템

[Upstage Design Tokens](https://www.upstage.ai/) 기반 — 라이트 모드 전용. 자세한 매핑은 `references/design-tokens.md`.

| 영역 | 컬러 |
|---|---|
| 본문 배경 | `#ffffff` (흰색 + 옅은 도트 그리드) |
| 본문 텍스트 | `--gray-500 (#434C60)` |
| 제목 | `--gray-700 (#0D1320)` |
| 액센트 | `--ups-purple (#805CFB)` — **섹션 라벨·강조선만** (배경 사용 금지) |
| 코드 블록 | `--gray-100 (#F8F7FD)` 옅은 회색 |

---

## 호환성

- **Claude Code** ≥ Skills 기능 지원 버전 (2025.10 이후)
- **Node** — 불필요 (HTML 생성에만 사용, 런타임 의존 없음)
- **Python** — `markitdown` 만 필요 (PPTX 입력 시):

  ```bash
  pip install "markitdown[pptx]"
  ```

생성된 HTML은 모든 모던 브라우저(Chrome, Safari, Firefox, Edge) 호환.

---

## 라이선스 / 크레딧

- 디자인 토큰: Upstage AI 공식 Brand Guide 기반
- 콘텐츠 형식: Anthropic Claude Mastery Playbook의 4-section 구조 차용
- Skill 사양: [Claude Code Skills 공식 문서](https://docs.claude.com/en/docs/claude-code/skills)

내부 사용 / 사내 배포 자유. 외부 공개 시 Upstage 브랜드 가이드 준수 권장.

---

## 기여

- 새 HTML 패턴이 필요하면 `references/html-patterns.md`에 P14, P15… 형식으로 추가
- 새 콘텐츠 전략이 필요하면 `references/content-strategy.md`에 추가
- **`assets/skeleton.html`의 CSS는 함부로 변경 금지** — 다른 사용자의 기존 Playbook과 디자인 일관성이 깨짐. 변경 시 버전 번호 올림.

---

## FAQ

**Q. 자료가 한국어와 영어 섞여 있어도 되나?**
A. 네. 출력 Playbook은 한국어 기준이지만, 인용·전문 용어는 원어 그대로 보존됨.

**Q. 이미지를 자동 생성할 수는 없나?**
A. 의도적으로 안 함. AI 생성 이미지는 디자인 일관성·정확성을 보장 못 해서 **placeholder만 두고 사용자가 직접 채우는** 방식. placeholder 안에 "들어갈 이미지·의도·권장 비율"이 명시됨.

**Q. PPTX 추출이 안 돼요.**
A. `pip install "markitdown[pptx]"` 확인. 그래도 안 되면 PPTX → PDF 변환 후 PDF로 입력.

**Q. Pillar 수를 5개 이상으로 하고 싶어요.**
A. Skill의 가이드는 2~4개. 5개 이상은 학습 단위가 너무 많아져 가독성 손해. 정말 필요하면 SKILL.md의 §3 Pillar 도출 부분을 fork 해서 수정.

**Q. 결과 HTML이 보라 배경이에요. 흰 배경이라고 했는데?**
A. `assets/skeleton.html`의 CSS가 변경됐을 가능성. `body { background: #ffffff }` 확인. (예전 버전에는 보라 JP-50 배경이었던 적 있음 — v1.0부터 흰색 고정.)

---
