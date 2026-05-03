# Upstage 디자인 토큰 — 웹 가이드북용

> 출처: `upstage-design-prompt.md` (Upstage 공식 브랜드 가이드, Airtable 기반) + Upstage Console Documentation 라이브 UI 관찰
> 용도: trend-tech-guidebook-builder 의 Task 5 (Brand Stylist)가 가이드북 HTML에 입힐 **CSS 변수 + 컴포넌트 매핑**.
>
> ⚠️ **라이트 모드 전용**. 다크모드 토글 미지원.
> ⚠️ 콘텐츠·DOM 구조는 절대 변경하지 않음. CSS만 입힘.

---

## 1. 디자인 철학 (모든 결정의 상위 규칙)

- **Visually Relaxing**: 여백을 충분히, 한 화면에 한 메시지.
- **Clean / Minimal / Professional / Trustworthy**.
- 그림자·장식·그라데이션·원색은 최소.
- 타이포그래피와 여백으로 위계를 만든다.

이 철학과 충돌하는 컴포넌트는 만들지 않는다.

---

## 2. CSS 변수 (전체 토큰)

```css
:root {
  /* === Brand === */
  --ups-purple: #805CFB;          /* 시그니처. 포인트/엑센트 전용. 배경 전면 사용 금지. */

  /* === Primary: Jacksons Purple === */
  --jp-50:  #ECF0FF;              /* 메인 배경 */
  --jp-100: #DDE3FF;              /* 호버, 연한 배경, 배지 bg */
  --jp-200: #C2CBFF;              /* 강조 배경, 자사 열 하이라이트 */
  --jp-300: #9CA7FF;              /* 아이콘 배경, 태그 */
  --jp-400: #7578FF;              /* 링크, KPI 숫자, 포인트 텍스트 */
  --jp-500: #5B52FF;              /* CTA 버튼 */
  --jp-600: #4F36F5;              /* 어두운 포인트 */
  --jp-700: #442AD8;
  --jp-800: #3725AE;
  --jp-900: #302689;
  --jp-950: #1E1650;

  /* === Secondary: Gray === */
  --gray-100: #F8F7FD;            /* 카드 내부 배경 */
  --gray-200: #E9E8F0;            /* 보더, 행 구분선 */
  --gray-300: #D1D1DB;            /* 비활성 */
  --gray-400: #979CAE;            /* 캡션, 각주, 주석 */
  --gray-500: #434C60;            /* 본문 */
  --gray-600: #1C2537;            /* 제목 */
  --gray-700: #0D1320;            /* 최고 어두운 텍스트 */

  /* === 코드블록 신택스 (Upstage Console 라이브 관찰값) === */
  --code-bg:       #F4F1F9;       /* 코드블록 배경 (JP 50과 Gray 100 사이의 옅은 보라/회색) */
  --code-text:     #1C2537;       /* 일반 코드 (Gray 600) */
  --code-comment:  #979CAE;       /* 주석 (Gray 400) */
  --code-keyword:  #C92A2A;       /* 키워드 (from, import, if, for, True, None) — 빨강 */
  --code-string:   #E8590C;       /* 문자열 — 주황 */
  --code-number:   #1864AB;       /* 숫자 — 진한 파랑 (보조) */
  --code-function: #1C2537;       /* 함수명/변수 — Gray 600 */

  /* === 의미 색 (콜아웃) === */
  /* Upstage 가이드는 원색 금지지만, 콜아웃은 의미 전달 위해 최소 톤다운 버전만 허용 */
  --info-bg:    #DDE3FF;          /* JP 100 */
  --info-fg:    #442AD8;          /* JP 700 */
  --warn-bg:    #FFF4E6;          /* 톤다운 주황 */
  --warn-fg:    #D9480F;
  --danger-bg:  #FFE3E3;
  --danger-fg:  #C92A2A;

  /* === 타이포그래피 === */
  --font-display: 'Geist', 'Noto Sans KR', system-ui, sans-serif;
  --font-body:    'Geist', 'Noto Sans KR', system-ui, sans-serif;
  --font-mono:    'Geist Mono', 'JetBrains Mono', 'D2Coding', Consolas, Monaco, monospace;
  /* ES Peak는 브랜드 타이틀 전용이라 웹 가이드북에서는 사용 안 함.
     Geist + Noto Sans KR Google Fonts CDN 로딩. */

  /* === 폰트 크기 (웹 본문 환산, PPT의 pt 가이드를 px로 매핑) === */
  --fs-h1:    36px;               /* 가이드 메인 타이틀 */
  --fs-h2:    28px;               /* 블록 제목 */
  --fs-h3:    22px;               /* 섹션 제목 */
  --fs-h4:    18px;               /* 소제목 */
  --fs-body:  16px;               /* 본문 */
  --fs-small: 14px;               /* 표 내부, 캡션 */
  --fs-tiny:  12px;               /* 각주, 출처 */
  --fs-code:  14px;               /* 코드블록 */

  /* === Weight === */
  --fw-regular: 400;
  --fw-medium:  500;
  --fw-bold:    700;
  --fw-extrabold: 800;

  /* === Line height === */
  --lh-heading: 1.3;
  --lh-body:    1.7;              /* cc101의 여유로운 호흡감 차용 */
  --lh-code:    1.55;

  /* === Spacing (8px 기반 스케일) === */
  --sp-1:  4px;
  --sp-2:  8px;
  --sp-3:  12px;
  --sp-4:  16px;
  --sp-5:  24px;
  --sp-6:  32px;
  --sp-7:  48px;
  --sp-8:  64px;
  --sp-9:  96px;

  /* === Layout === */
  --container-max: 880px;         /* 본문 max-width */
  --sidebar-width: 280px;         /* TOC 사이드바 (옵션) */
  --header-height: 64px;

  /* === Border / Radius / Shadow === */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --border-thin: 1px solid var(--gray-200);
  --shadow-soft: 0 1px 2px rgba(28, 37, 55, 0.04);  /* "거의 사용 안 함" 원칙 */
}
```

---

## 3. Google Fonts CDN

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@400;500;700;800&family=Geist+Mono:wght@400;500&family=Noto+Sans+KR:wght@400;500;700;800&display=swap" rel="stylesheet">
```

폴백: 시스템 폰트 (system-ui, Apple SD Gothic Neo, Pretendard).

---

## 4. 컴포넌트 매핑 — 가이드북 HTML 요소별 스타일

### 4-1. 페이지 골격

```css
body {
  background: var(--jp-50);          /* 메인 배경 = JP 50 */
  color: var(--gray-500);            /* 본문 = Gray 500 */
  font-family: var(--font-body);
  font-size: var(--fs-body);
  line-height: var(--lh-body);
}

main {
  max-width: var(--container-max);
  margin: 0 auto;
  padding: var(--sp-7) var(--sp-5);
}
```

### 4-2. 헤딩

```css
h1, h2, h3, h4 {
  font-family: var(--font-display);
  color: var(--gray-600);             /* 제목 = Gray 600 */
  line-height: var(--lh-heading);
  letter-spacing: -0.01em;
}

h1 { font-size: var(--fs-h1); font-weight: var(--fw-extrabold); color: var(--gray-700); margin: var(--sp-7) 0 var(--sp-5); }
h2 { font-size: var(--fs-h2); font-weight: var(--fw-bold);     margin: var(--sp-6) 0 var(--sp-4); }
h3 { font-size: var(--fs-h3); font-weight: var(--fw-bold);     margin: var(--sp-5) 0 var(--sp-3); }
h4 { font-size: var(--fs-h4); font-weight: var(--fw-medium);   margin: var(--sp-5) 0 var(--sp-3); }
```

**금지**: 제목 아래 장식적 밑줄/가로선.

### 4-3. 섹션 라벨 (좌상단 작은 라벨)

cc101의 "00 시작하기 전에" 같은 섹션 번호+라벨을 Upstage 스타일로:

```css
.section-label {
  font-size: var(--fs-tiny);
  color: var(--ups-purple);            /* 시그니처 컬러의 거의 유일한 사용처 */
  font-weight: var(--fw-medium);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  margin-bottom: var(--sp-2);
}
```

### 4-4. 본문 단락

```css
p { margin: 0 0 var(--sp-4); }

strong { color: var(--gray-700); font-weight: var(--fw-bold); }
em     { color: var(--gray-600); font-style: italic; }
```

**규칙**: 한 단락 5줄 이내. 굵음 강조는 한 단락에 1군데만.

### 4-5. 링크

```css
a {
  color: var(--jp-400);                 /* 링크 = JP 400 (#7578FF) */
  text-decoration: none;
  border-bottom: 1px dotted var(--jp-200);
  transition: color 0.15s, border-color 0.15s;
}
a:hover {
  color: var(--jp-600);
  border-bottom-color: var(--jp-400);
}
```

### 4-6. 코드블록 (라이트 톤, Upstage Console 관찰값)

```css
pre {
  background: var(--code-bg);          /* #F4F1F9 */
  color: var(--code-text);
  border-radius: var(--radius-md);
  padding: var(--sp-5);
  overflow-x: auto;
  font-family: var(--font-mono);
  font-size: var(--fs-code);
  line-height: var(--lh-code);
  position: relative;
  margin: var(--sp-5) 0;
}

/* 인라인 코드 */
code:not(pre code) {
  background: var(--gray-100);
  color: var(--gray-700);
  padding: 2px 6px;
  border-radius: var(--radius-sm);
  font-family: var(--font-mono);
  font-size: 0.92em;
}

/* 신택스 하이라이트 (highlight.js / prism 호환 클래스 매핑) */
.hljs-keyword,  .token.keyword     { color: var(--code-keyword); }
.hljs-string,   .token.string      { color: var(--code-string); }
.hljs-comment,  .token.comment     { color: var(--code-comment); font-style: italic; }
.hljs-number,   .token.number      { color: var(--code-number); }
.hljs-function, .token.function    { color: var(--code-function); font-weight: var(--fw-medium); }

/* 우상단 복사 버튼 (선택) */
.code-copy {
  position: absolute;
  top: var(--sp-3);
  right: var(--sp-3);
  background: white;
  border: var(--border-thin);
  border-radius: var(--radius-sm);
  padding: var(--sp-1) var(--sp-2);
  font-size: var(--fs-tiny);
  color: var(--gray-500);
  cursor: pointer;
}
```

### 4-7. 콜아웃 박스 (팁/주의/위험)

cc101의 인용문 스타일 + Upstage 카드 스타일을 머지.

```css
.callout {
  border-radius: var(--radius-md);
  padding: var(--sp-4) var(--sp-5);
  margin: var(--sp-5) 0;
  border-left: 4px solid;
  background: var(--gray-100);
  display: flex;
  gap: var(--sp-3);
}
.callout-icon { font-size: 20px; flex-shrink: 0; }
.callout-body { flex: 1; }
.callout-body p:last-child { margin-bottom: 0; }

.callout--tip   { border-left-color: var(--jp-400);  background: var(--info-bg); }
.callout--note  { border-left-color: var(--gray-400); background: var(--gray-100); }
.callout--warn  { border-left-color: var(--warn-fg);  background: var(--warn-bg); }
.callout--danger{ border-left-color: var(--danger-fg);background: var(--danger-bg); }
```

이모지: 💡 tip / ℹ️ note / ⚠️ warn / 🔴 danger.

### 4-8. 표

```css
table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  border-radius: var(--radius-md);
  overflow: hidden;
  margin: var(--sp-5) 0;
  font-size: var(--fs-small);
}

th {
  background: var(--jp-300);            /* 헤더 = JP 300 */
  color: white;
  font-weight: var(--fw-bold);
  text-align: left;
  padding: var(--sp-3) var(--sp-4);
}

td {
  padding: var(--sp-3) var(--sp-4);
  border-top: var(--border-thin);
  background: white;
}

tr:hover td { background: var(--gray-100); }

/* 자사 열 하이라이트 (비교표에서) */
.highlight-col { background: var(--jp-100) !important; }
```

### 4-9. 카드 / 박스

```css
.card {
  background: white;
  border: var(--border-thin);
  border-radius: var(--radius-lg);
  padding: var(--sp-5);
  box-shadow: var(--shadow-soft);
}
.card--muted {
  background: var(--gray-100);
  border: none;
}
```

### 4-10. 배지 / 태그

```css
.badge {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 999px;
  font-size: var(--fs-tiny);
  font-weight: var(--fw-medium);
  background: var(--jp-100);
  color: var(--jp-700);
}
.badge--purple { background: var(--jp-100); color: var(--jp-700); }
.badge--gray   { background: var(--gray-200); color: var(--gray-600); }
```

### 4-11. 목차 (TOC, 사이드바 또는 상단)

```css
.toc {
  background: white;
  border: var(--border-thin);
  border-radius: var(--radius-md);
  padding: var(--sp-4) var(--sp-5);
  margin-bottom: var(--sp-6);
}
.toc-title {
  font-size: var(--fs-tiny);
  text-transform: uppercase;
  letter-spacing: 0.04em;
  color: var(--gray-400);
  margin-bottom: var(--sp-3);
}
.toc a {
  display: block;
  padding: var(--sp-1) 0;
  color: var(--gray-500);
  border: none;
  font-size: var(--fs-small);
}
.toc a:hover { color: var(--ups-purple); }
.toc a.active { color: var(--ups-purple); font-weight: var(--fw-medium); }
```

### 4-12. 헤더 / 네비

```css
header {
  position: sticky;
  top: 0;
  height: var(--header-height);
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-bottom: var(--border-thin);
  z-index: 100;
  display: flex;
  align-items: center;
  padding: 0 var(--sp-5);
}
```

---

## 5. 절대 금지 (Upstage 공식 Anti-pattern 적용)

- ❌ Ups Purple(#805CFB)을 배경 전면에 사용 (포인트만)
- ❌ 빨강·초록·노랑·오렌지 원색 사용 (단, 코드 신택스/콜아웃 의미 색은 예외)
- ❌ 그라데이션 배경 (가이드북에서는 일관성을 위해 단색 JP 50만)
- ❌ 제목 아래 장식적 밑줄/가로선
- ❌ 본문 가운데 정렬 (좌측 정렬 기본)
- ❌ 과도한 그림자 (--shadow-soft만 허용)
- ❌ 이모지 클립아트 일러스트 (콜아웃 아이콘은 텍스트 이모지 OK)
- ❌ 다크모드 (이번 가이드는 라이트 전용)
- ❌ 페이지 전체에 강한 brand color 도포

---

## 6. 우선순위 충돌 시 (cc101 vs Upstage)

색·폰트·여백·시각 디자인 모든 영역에서 **Upstage 토큰이 100% 이김**. cc101은 콘텐츠 구조(섹션 흐름·톤·패턴)만 차용.

예시:
- cc101 = 파란 링크 / Upstage = JP 400(#7578FF) → **JP 400** 사용
- cc101 = 코드블록 회색 bg / Upstage = #F4F1F9 옅은 보라 bg → **#F4F1F9** 사용
- cc101 = 시스템 한글 폰트 / Upstage = Noto Sans KR → **Noto Sans KR** 사용

---

## 7. Task 5 적용 체크리스트

Brand Stylist가 디자인 입힐 때 자체 검증:

- [ ] body bg = JP 50인가?
- [ ] 본문 색 = Gray 500인가?
- [ ] 제목 색 = Gray 600/700인가?
- [ ] 링크 색 = JP 400인가?
- [ ] 코드블록 bg = #F4F1F9인가? 신택스 4색이 정확한가?
- [ ] 콜아웃 4종(tip/note/warn/danger)이 좌측 보더 + 옅은 bg인가?
- [ ] 표 헤더 = JP 300 bg + 흰 텍스트인가?
- [ ] 폰트 = Geist + Noto Sans KR인가? (Google Fonts CDN 로딩)
- [ ] line-height: body 1.7 / heading 1.3 / code 1.55인가?
- [ ] max-width 880px 컨테이너 중앙 정렬인가?
- [ ] 본문 한 단락이 5줄을 넘지 않는가?
- [ ] 이미지에서 본 Upstage Console과 비슷한 인상인가?
- [ ] 단 한 글자도 콘텐츠를 건드리지 않았는가?
