# 최종 산출물 렌더 가이드

WS6까지 끝나면 진행 파일(`progress/{팀명}_진행.md`)을 입력으로 두 산출물을 만든다.

## 1) 워크시트 완성본 HTML

1. `assets/worksheet_ui_template.html`을 읽는다.
2. `{{TOKEN}}`을 진행 파일 내용으로 치환한다. 매핑:
   - `{{TEAM}} {{MEMBERS}} {{DATE}}` ← 머리말. `{{WORK_TITLE}}` ← WS1 자동화하고 싶은 업무(한 문장).
   - `{{WS1_*}}` ← WS1 5칸. `{{WS2_TRIGGER}}/{{WS2_OUTPUT}}` ← WS2. `{{WS5_*}} {{PRD1..9}}` ← 해당 칸.
   - `{{WS3_AGENTS}}` ← 로드맵 Agent 목록(쉼표/줄바꿈). `{{WS4_PICK}}/{{WS4_REASON}}` ← 1순위·이유.
3. **흐름(WS2/WS3)은 도형 기반 순서도(`.fc`)로 그린다** — 번호 리스트가 아니라 교육에서 쓰는 순서도 도형으로.
   - 각 단계 = `<div class="node 도형">텍스트 …뱃지</div>`, 단계 사이마다 `<div class="conn"></div>`(화살표).
   - 도형 매핑: **start**(시작/종료·타원형) · **process**(처리·사각형, 기본) · **io**(입출력·평행사변형 — 안에 `<span>텍스트</span>`) · **decision**(판단·마름모 — 안에 `<span>텍스트?</span>`) · **agent**(WS3에서 Agent 박스·보라). 전체 규칙은 `references/flowchart_rules.md`.
   - **분기(Yes/No)**: `node decision` 바로 뒤에 `<div class="branch">`로 두 갈래의 목적지를 적는다. No가 앞 단계로 돌아가면 `↩ 복귀`로. 한쪽만 그리지 않는다.
     ```html
     <div class="node decision"><span>요건 충족?</span></div>
     <div class="branch">
       <div class="branch-leg"><span class="yn yes">Yes</span>다음 단계로</div>
       <div class="branch-leg"><span class="yn no">No</span>앞 단계로 복귀 ↩</div>
     </div>
     ```
   - **병렬**은 `.parallel` 컨테이너로 묶고 안에 `.parallel-row`로 노드를 나란히. (예: As-Is의 동시 진행 단계)
   - 태그 뱃지는 *해당될 때만* node 안에 부착: `badge p`(P) · `badge h`(H) · `badge o`(◎).
   - WS3는 As-Is에서 ◎였던 자리를 `node agent`로, 사람 검토/승인은 `node decision`("◇ …")으로.
   - 예)
     ```html
     <div class="node start">요청 접수</div>
     <div class="conn"></div>
     <div class="parallel"><div class="parallel-label">병렬</div><div class="parallel-row">
       <div class="node process">자료원 A 조회 <span class="badge p">P</span><span class="badge o">◎</span></div>
       <div class="node process">자료원 B 조회 <span class="badge p">P</span><span class="badge o">◎</span></div>
     </div></div>
     <div class="conn"></div>
     <div class="node start">결과 정리·저장</div>
     ```
4. **WS4 표**는 후보별 `<tr>`을 복제한다.
5. 빈 칸은 팀이 적은 그대로. 정말 비었으면 `(추후 확인)`. 임의로 지어내지 않는다.
6. 남은 `{{...}}`/예시 `<li>`/주석은 모두 제거한다.
7. `outputs/{팀명}_워크시트_완성본.html`로 저장하고 경로를 안내한다.

> 품질: 실제 팀이 말한 표현을 살린다(딱딱한 재작성 X). 흐름은 단계 리스트로 충분 — 다이어그램 엔진 불필요.

## 2) PRD 초안 .docx

1. PRD 9칸을 JSON으로 만든다(예: `outputs/_prd_data.json`). 형식은 `scripts/build_prd_docx.py` 상단 주석 참고.
   - 핵심 3칸(대상 사용자·기능 요구사항·구현 방향)이 비면, 생성 전에 한 번 더 짧게 물어 채운다.
2. 생성:
   ```bash
   cd <skill 디렉터리>
   python3 scripts/build_prd_docx.py outputs/_prd_data.json "outputs/{팀명}_PRD초안.docx"
   ```
   - `python-docx` 미설치면: `pip install python-docx` 후 재실행.
3. 두 파일(HTML + docx) 경로를 함께 안내하고 마무리한다.
