# Upstage 디자인 토큰 — Playbook 전용 요약

`assets/skeleton.html`에 이미 모든 CSS가 박혀 있으므로, **너는 디자인 토큰을 새로 추가할 필요가 없다**. 이 문서는 콘텐츠를 채울 때 어떤 컬러·폰트·간격이 어디 쓰이는지 이해하기 위한 reference다.

원본 출처: Upstage 공식 Brand Guide (Airtable) 기반 토큰. 라이트 모드 전용.

---

## 1. 컬러 — 어디에 무엇을 쓰는가

| CSS 변수 | HEX | 용도 |
|---|---|---|
| `--ups-purple` | `#805CFB` | **시그니처. 액센트 전용 — 섹션 라벨, 활성 네비, 강조 라인.** 배경 전면 사용 절대 금지 |
| `--jp-50` | `#ECF0FF` | 옅은 액센트 배경 (예: callout--tip) |
| `--jp-100` | `#DDE3FF` | 강조 카드 배경 (master-line, activity 강조) |
| `--jp-200` | `#C2CBFF` | 표의 자사 열 / 배지 |
| `--jp-300` | `#9CA7FF` | 도식 노드 |
| `--jp-400` | `#7578FF` | KPI 숫자, 링크, 포인트 텍스트 |
| `--jp-700` | `#442AD8` | 정보 callout 텍스트 |
| `--gray-100` | `#F8F7FD` | 코드 블록 배경, 카드 보조 배경 |
| `--gray-150` | `#F1F1F4` | 활성 네비 알약 배경 |
| `--gray-200` | `#E9E8F0` | 보더, 행 구분선 |
| `--gray-400` | `#979CAE` | 캡션, 각주, 페이지 번호 |
| `--gray-500` | `#434C60` | 본문 텍스트 |
| `--gray-600` | `#1C2537` | 일반 제목 |
| `--gray-700` | `#0D1320` | 큰 제목 (h1, hero) |

### 의미 색 (callout 전용 — 톤다운)

| 변수 | HEX | 용도 |
|---|---|---|
| `--info-bg` / `--info-fg` | `#EEF1FF` / `#442AD8` | callout--tip |
| `--warn-bg` / `--warn-fg` | `#FFF8EE` / `#B85C0F` | callout--warn |
| `--danger-bg` / `--danger-fg` | `#FFF1F1` / `#B72A2A` | callout--danger |

원색(빨강·초록·노랑·오렌지) 직접 사용 금지. 의미 색은 위 callout 변수만.

---

## 2. 타이포그래피

```
font-display:  'Geist', 'Noto Sans KR', system-ui, sans-serif
font-body:     'Geist', 'Noto Sans KR', system-ui, sans-serif
font-mono:     'Geist Mono', 'JetBrains Mono', 'D2Coding', Consolas, Monaco, monospace
```

Google Fonts CDN preconnect는 skeleton.html에 이미 들어 있다.

### 사이즈 (skeleton.html에 박혀 있음)

| 요소 | px |
|---|---|
| Hero h1 | 44 |
| Section h2 | 28 |
| Sub-heading h3 | 18 |
| Sub-sub h4 | 15 |
| Body | 15 |
| Small (caption, badge) | 13.5 / 13 |
| Tiny (page num, eyebrow, footer) | 11 / 10.5 |
| KPI number | 22–32 (variable) |

### Line height

- Body: 1.7 (여유로운 호흡감)
- Heading: 1.3
- Code: 1.6

---

## 3. 레이아웃

| 항목 | 값 |
|---|---|
| 컨테이너 max-width | 820px |
| 사이드바 너비 | 280px |
| 상단 topbar 높이 | 56px |
| 본문 좌우 padding | 32px |
| 마진 표준 | 24 / 32 / 48 / 64 / 88 / 96px |

본문 단락은 5줄 이내로. 굵은 강조는 한 단락에 1군데만.

---

## 4. 컴포넌트 별 디자인 결정

### 사이드바 활성 상태

```css
.sidebar a.active {
  background: var(--gray-150);  /* 옅은 회색 알약 — Upstage Console 스타일 */
  color: var(--gray-700);
  font-weight: 600;
}
```

**보라(`--ups-purple`) 강조 사용 안 함** (Upstage Console 사이드바 스타일 차용).

### 섹션 라벨

```css
.section-label {
  background: var(--info-bg);
  color: var(--ups-purple);
  font-size: 10.5px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
```

uppercase + letter-spacing 0.1em + 작은 알약 형태.

### 4-section 카드 (sub-section 본문)

흰 배경 + 옅은 보더 + 8px radius + 미세 그림자 (`--shadow-soft`만 허용).

### Callout

좌측 4px 컬러 보더 + 옅은 컬러 배경 + 아이콘 1개. 4종 (tip / note / warn / danger).

### 표

| 영역 | 스타일 |
|---|---|
| Border | 1px gray-200, 8px radius, overflow hidden |
| Header | gray-150 배경, gray-700 텍스트, weight 600 |
| Body | white 배경, gray-200 보더 행 구분 |

**보라 헤더 사용 안 함** (Upstage Console 톤). 자사 열 강조가 필요하면 `--jp-100` 옅은 배경.

### 코드 블록

옅은 회색 배경(`--gray-100`) + 보더 + 우상단 "복사" 버튼. 활동 섹션 코드 블록에 필수.

### 도트 그리드 배경

전체 화면 뒤에 매우 옅은 도트 그리드(opacity 0.35)를 깔아 Upstage Console 질감을 흉내. skeleton.html의 `.grid-bg`로 제어.

---

## 5. 절대 금지 (Anti-patterns)

| 금지 | 왜 |
|---|---|
| 보라(`--ups-purple`)를 본문 배경에 사용 | Upstage 가이드 §5 위반 |
| 다른 컬러 시스템 도입 | 일관성 깨짐 |
| 그라데이션 배경 | 일반 콘텐츠 슬라이드 금지 |
| 제목 아래 장식적 밑줄/가로선 | AI 생성 슬라이드 hallmark — 반드시 회피 |
| 본문 가운데 정렬 | hero·activity 헤더만 예외, 본문은 좌측 |
| 다크 모드 | 라이트 전용 |
| 8자리 hex 색상 | pptxgenjs / browser 호환성 — opacity 별도 |
| 과도한 그림자 | `--shadow-soft`만 허용 |
| 이모지 본문 사용 | callout 4종(💡 ℹ️ ⚠️ 🔴)에만 한정 |

---

## 6. 충돌 시 우선순위

콘텐츠 자료가 다른 디자인 시스템(예: Material, Bootstrap)을 추천하더라도 — **Playbook은 Upstage 토큰 100% 적용**한다. 톤·구조·문구는 자료에서, 시각은 Upstage에서.
