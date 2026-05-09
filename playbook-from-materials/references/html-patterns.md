# HTML 패턴 — Playbook 콘텐츠를 채울 때 그대로 복사해서 쓰는 블록

이 문서는 `assets/skeleton.html`의 `<!-- CONTENT_SECTIONS -->` 영역에 들어갈 모든 재사용 패턴을 모아둔다. **새 디자인 만들지 말고, 여기 있는 패턴만 조합**한다.

---

## P0. 사이드바 네비

`<!-- SIDEBAR_NAV -->` 영역에 다음 패턴을 채운다.

```html
<div class="nav-section">
  <div class="nav-pillar-title">Overview</div>
  <a href="#intro">시작하기 전에</a>
</div>

<div class="nav-section">
  <div class="nav-pillar-title">1. [Pillar 1 이름]</div>
  <a href="#p1-1">1.1 [sub-section 제목]</a>
  <a href="#p1-2">1.2 ...</a>
  <a href="#p1-3">1.3 ...</a>
  <a href="#p1-4">1.4 ...</a>
  <a href="#p1-5">1.5 [활동] ...</a>
</div>

<!-- pillar 2, 3 동일 패턴 -->
```

각 anchor ID(`#p1-1`)는 본문의 `<section id="p1-1">`과 정확히 일치해야 한다.

---

## P1. Pillar Divider — pillar 시작 부분

```html
<div class="pillar-divider">
  <div class="pillar-num">PILLAR 01</div>
  <h2>[Pillar 이름]</h2>
  <p>[Pillar의 한 줄 정의 — 자료의 핵심 인용 가능]</p>
</div>
```

---

## P2. 개념 sub-section — 4-section 풀 구조

```html
<section id="p1-1">
  <span class="section-label">1.1 / Concept</span>
  <h2>[sub-section 제목]</h2>
  <p class="headline">[한 줄 헤드라인 — 본문 톤 미리보기]</p>

  <!-- 이미지 플레이스홀더 (선택) -->
  <figure class="figure figure--illustration" aria-label="placeholder">
    <div class="figure-placeholder">
      <span class="figure-tag">ILLUSTRATION</span>
      <p class="figure-spec"><strong>들어갈 이미지:</strong> [구체 묘사]</p>
      <p class="figure-spec"><strong>의도:</strong> [왜 필요한가]</p>
      <p class="figure-spec"><strong>권장 비율:</strong> [16:9 / 4:3 / 정사각형]</p>
    </div>
  </figure>

  <!-- A. 핵심 원리 및 개념 -->
  <h3>[소제목]</h3>
  <p>[정의 본문]</p>
  <ul>
    <li><strong>[키워드]</strong> — [설명]</li>
    <li><strong>[키워드]</strong> — [설명]</li>
  </ul>

  <!-- B. 실전 활용 가이드 (필요 시) -->
  <h3>[실전 적용 소제목]</h3>
  [표 / 카드 / 리스트]

  <!-- C. 전략적 인사이트 (callout) -->
  <div class="callout callout--warn">
    <div class="callout-body">
      <p><strong>흔한 함정</strong> — [안티패턴 설명]</p>
    </div>
  </div>

  <!-- D. Key Summary -->
  <div class="master-line">
    <span class="label">Key Summary</span>
    "[강한 단언 한 문장 — <strong>핵심 키워드</strong> 굵게]"
  </div>
</section>
```

`section-label`의 `.label` 텍스트는 sub-section 번호 + Focus(Concept / Components / How / Caution) 한 단어. 예: `1.1 / Concept`, `2.3 / How it works`.

---

## P3. Section Label 종류

| Focus | 라벨 |
|---|---|
| 개념 이해 | `[번호] / Concept` |
| 구성 요소 | `[번호] / Components` |
| 어떻게 작동 | `[번호] / How it works` |
| 주의사항 | `[번호] / Caution` |
| 활동 | `[번호] · Activity` |

---

## P4. Callout — 4종

```html
<!-- TIP (긍정·권장) -->
<div class="callout callout--tip">
  <div class="callout-body">
    <p><strong>강조</strong> — [팁 본문]</p>
  </div>
</div>

<!-- NOTE (정보·hedging) -->
<div class="callout callout--note">
  <div class="callout-body">
    <p><strong>중요한 hedging</strong> — [출처 / 권위 / 변동성 단서]</p>
  </div>
</div>

<!-- WARN (주의) -->
<div class="callout callout--warn">
  <div class="callout-body">
    <p>현상 — [상황]. 결과 — [부작용].</p>
    <p><strong>해결책</strong> — [조치]</p>
  </div>
</div>

<!-- DANGER (위험) -->
<div class="callout callout--danger">
  <div class="callout-body">
    <p><strong>중대 경고</strong> — [심각한 위험 본문]</p>
  </div>
</div>
```

---

## P5. 표 — 비교·구성요소 정리

```html
<table>
  <thead>
    <tr>
      <th>[컬럼 1]</th>
      <th>[컬럼 2]</th>
      <th>[컬럼 3]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>[행 라벨]</strong></td>
      <td>[셀]</td>
      <td>[셀]</td>
    </tr>
    <tr>
      <td><strong>[행 라벨]</strong></td>
      <td>[셀]</td>
      <td>[셀]</td>
    </tr>
  </tbody>
</table>
```

7행 초과 금지. 더 많으면 핵심만 남기고 나머지는 본문 bullet으로.

---

## P6. Numbered Step Card — "Start building" 풍

순차적 흐름·작동 단계를 보여줄 때.

```html
<div class="steps">
  <div class="step">
    <div class="step-num">1</div>
    <div>
      <h4>[step 제목]</h4>
      <p>[설명 — <code>키워드</code> 강조 가능]</p>
    </div>
  </div>
  <div class="step">
    <div class="step-num">2</div>
    <div>
      <h4>[step 제목]</h4>
      <p>[설명]</p>
    </div>
  </div>
  <!-- 더 많은 step 추가 -->
</div>
```

3~5개 step이 적당. 6개 이상이면 표로 변환.

---

## P7. 2-col / 3-col 카드 그리드

```html
<!-- 2-column -->
<div class="col-2">
  <div class="col-card">
    <span class="num">CONCEPT</span>
    <h4>[카드 제목]</h4>
    <p>[1줄 설명]</p>
  </div>
  <div class="col-card">
    <span class="num">COMPONENTS</span>
    <h4>[카드 제목]</h4>
    <p>[1줄 설명]</p>
  </div>
</div>

<!-- 3-column -->
<div class="col-3">
  <div class="col-card">
    <span class="num">2023~</span>
    <h4>[제목]</h4>
    <p>[설명]</p>
  </div>
  <!-- ... -->
</div>
```

`<span class="num">`는 우측 상단 작은 라벨 (시기 / 카테고리 / 번호 표시).

---

## P8. Master Line (sub-section 마지막)

```html
<div class="master-line">
  <span class="label">Key Summary</span>
  "[강한 take-away — <strong>핵심 단어</strong>는 bold]"
</div>
```

매 sub-section의 closing line. **활동 섹션은 master line 대신 callout--tip으로 권장 워크플로 안내**.

---

## P9. Figure Placeholder — 이미지 자리

```html
<!-- 스크린샷 자리 -->
<figure class="figure figure--screenshot" aria-label="placeholder">
  <div class="figure-placeholder">
    <span class="figure-tag">SCREENSHOT</span>
    <p class="figure-spec"><strong>들어갈 이미지:</strong> [실제 화면 / 캡처 묘사]</p>
    <p class="figure-spec"><strong>의도:</strong> [왜 필요한가]</p>
    <p class="figure-spec"><strong>권장 비율:</strong> 16:9</p>
  </div>
</figure>

<!-- 일러스트 / 다이어그램 자리 -->
<figure class="figure figure--illustration" aria-label="placeholder">
  <div class="figure-placeholder">
    <span class="figure-tag">ILLUSTRATION</span>
    <p class="figure-spec"><strong>들어갈 이미지:</strong> [개념 도식 묘사]</p>
    <p class="figure-spec"><strong>의도:</strong> [왜 필요한가]</p>
    <p class="figure-spec"><strong>권장 비율:</strong> 4:3 또는 정사각형</p>
  </div>
</figure>
```

각 pillar 최소 2개 이상 placeholder를 둔다. 너무 많아도 산만해지므로 sub-section당 1개 정도가 적당.

---

## P10. Activity Section — 채워서 쓰는 템플릿

```html
<section id="p1-5">
  <span class="section-label">1.5 · Activity</span>
  <h2>[활동] [활동 제목]</h2>
  <p class="headline">[활동 의도 한 줄]</p>

  <!-- 이미지 placeholder (선택) -->
  <figure class="figure figure--screenshot" aria-label="placeholder">
    <div class="figure-placeholder">
      <span class="figure-tag">SCREENSHOT</span>
      <p class="figure-spec"><strong>들어갈 이미지:</strong> [잘 채워진 예시]</p>
      <p class="figure-spec"><strong>의도:</strong> 빈 템플릿 앞에서 막히는 학습자에게 reference</p>
      <p class="figure-spec"><strong>권장 비율:</strong> 16:9</p>
    </div>
  </figure>

  <!-- Step 카드 1 -->
  <div class="activity-card">
    <h3><span class="step-tag">STEP 1</span>[step 제목]</h3>
    <p>[안내문]</p>
    <pre><button class="code-copy">복사</button><code># [템플릿 헤더]

## [필드 1]
(설명)

## [필드 2]
(설명)
</code></pre>
  </div>

  <!-- Step 카드 2, 3 동일 패턴 -->
  <div class="activity-card">
    <h3><span class="step-tag">STEP 2</span>[step 제목]</h3>
    <p>[안내문]</p>
    <pre><button class="code-copy">복사</button><code># [템플릿]
...
</code></pre>
  </div>

  <!-- 마무리 callout (master-line 대신) -->
  <div class="callout callout--tip">
    <div class="callout-body">
      <p>[권장 워크플로 한 마디]</p>
    </div>
  </div>
</section>
```

**중요**: `<pre>` 안에 반드시 `<button class="code-copy">복사</button>`를 첫 자식으로 둔다. skeleton.html의 JS가 이 버튼을 자동으로 click handler에 연결한다.

---

## P11. Hero (Welcome) — Intro 영역

```html
<div class="hero">
  <div class="hero-eyebrow">[Pillar 1] · [Pillar 2] · [Pillar 3]</div>
  <h1>Welcome to<br>[주제] Playbook</h1>
  <p class="hero-lede">
    [학습자가 누구인지 + 이 Playbook이 무엇을 다루는지 한 단락. <strong>주요 단어</strong>는 굵게.]
  </p>
</div>
```

---

## P12. Intro 본문 (Welcome 다음)

```html
<section id="intro">
  <span class="section-label">Overview</span>
  <h2>시작하기 전에</h2>
  <p class="headline">[Playbook 한 줄 가치]</p>

  <h3>이 Playbook이 다루는 것</h3>
  <div class="steps">
    <div class="step">
      <div class="step-num">1</div>
      <div>
        <h4>[Pillar 1 이름]</h4>
        <p>[Pillar 1 설명 한 단락]</p>
      </div>
    </div>
    <!-- pillar 2, 3 step 추가 -->
  </div>

  <h3>이 Playbook을 읽는 법</h3>
  <p>[읽는 방법 안내 — 좌측 네비, sub-section 4-focus 구조 소개]</p>

  <div class="col-2">
    <div class="col-card">
      <span class="num">CONCEPT</span>
      <h4>개념 이해</h4>
      <p>그게 무엇이고, 왜 지금 필요한가.</p>
    </div>
    <div class="col-card">
      <span class="num">COMPONENTS</span>
      <h4>구성 요소</h4>
      <p>무엇으로 이루어져 있는가.</p>
    </div>
    <div class="col-card">
      <span class="num">HOW</span>
      <h4>어떻게 작동하나</h4>
      <p>구성 요소가 어떻게 맞물려 돌아가는가.</p>
    </div>
    <div class="col-card">
      <span class="num">CAUTION</span>
      <h4>주의사항</h4>
      <p>흔한 안티패턴과 빠지기 쉬운 함정.</p>
    </div>
  </div>

  <h3>활동 섹션</h3>
  <p>각 pillar 끝의 <strong>[활동]</strong> 섹션에는 채워서 쓸 수 있는 템플릿이 들어 있습니다. 우상단 <code>복사</code> 버튼으로 마크다운을 자기 에디터에 붙여 넣어 채워보세요.</p>

  <div class="callout callout--tip">
    <div class="callout-body">
      <p><strong>읽는 시간 가이드</strong> · [pillar별 예상 시간 안내]</p>
    </div>
  </div>
</section>
```

---

## 13. 톤 / 어조 일관 유지

본문 작성 시 다음 톤 규칙:

- 문장 어미는 일관 — `~합니다` (격식체) 또는 `~이다` (단정형) 중 하나로 통일
- 한 단락 5줄 이내
- 굵은 강조는 한 단락에 1군데만
- 구체 수치·인용은 출처와 함께
- 영어 약어는 처음 등장 시 풀어쓰기 (예: "MCP (Model Context Protocol)")

---

## 14. 안티패턴 (절대 하지 말 것)

| 안티패턴 | 왜 |
|---|---|
| 새 CSS 클래스 추가 | skeleton.html에 모든 게 있음. 추가 클래스는 유지보수 비용 |
| 인라인 `style="..."` 남발 | 디자인 일관성 깨짐. 위 closing 같은 예외만 |
| 컬러를 hex로 직접 박기 | CSS 변수만 사용 (var(--xxx)) |
| `<br>` 남발 | 단락 분리는 `<p>`로 |
| `<div>`로 모든 걸 감싸기 | semantic HTML(section/figure/article) 우선 |
| 이모지 일체 사용 | 본문·라벨·callout 모두 텍스트만. callout 색상·테두리로 종류 구분 |
