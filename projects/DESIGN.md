# 경력기술서 상세 페이지 디자인 가이드

메인 이력서(`index.html`)의 프로젝트 제목을 누르면 들어가는 **프로젝트별 경력기술서 상세 페이지**의 기준 문서입니다.
기준 구현은 `projects/woori-misselling.html` + `projects/project.css`입니다. 새 프로젝트 페이지는 이 문서와 기준 구현을 복사해 시작합니다.

---

## 1. 파일 구조

```
index.html                  메인 이력서 (PDF로 제출)
main.css                    메인 이력서 스타일
projects/
  DESIGN.md                 이 문서
  project.css               모든 상세 페이지 공통 스타일 (페이지별 CSS 만들지 않음)
  woori-misselling.html     우리은행 AI 기반 불완전판매방지 솔루션 구축
  <고객사>-<주제>.html        새 상세 페이지
```

- 파일명: 영문 소문자 + 하이픈. `고객사-핵심주제` 형식 (예: `kb-realtime-voice.html`, `nh-counsel.html`).
- 스타일은 `project.css` 하나만 사용합니다. 페이지 안 `<style>`이나 인라인 스타일은 시퀀스 다이어그램의 `grid-column`/`--n` 외에는 쓰지 않습니다.

### 메인 이력서 연결

`index.html`의 해당 프로젝트 제목을 링크로 감쌉니다. 제목 스타일은 `main.css`의 `.project-head h4 a`가 처리합니다.

```html
<h4><a href="./projects/woori-misselling.html">우리은행 AI 기반 불완전판매방지 솔루션 구축</a></h4>
```

---

## 2. 페이지 골격

화면은 **왼쪽 고정 목차(`.toc`) + 오른쪽 본문(`.page`)** 2단입니다. 메인 이력서의 왼쪽 사이드바 구도를 따릅니다.

```html
<body>
  <div class="layout">
    <aside class="toc">
      <a class="back" href="../index.html">← 이력서로 돌아가기</a>
      <p class="toc-kicker">경력기술서</p>
      <p class="toc-title">프로젝트명<br />두 줄로</p>
      <p class="toc-meta">YYYY.MM – YYYY.MM · 연계사 / 소속사</p>
      <nav>
        <p class="toc-label">목차</p>
        <a href="#arch"><span>00</span>아키텍처</a>
        <!-- 섹션 수만큼 -->
      </nav>
    </aside>

    <main class="page">
      <!-- 헤더 → 섹션 00 ~ 06 -->
    </main>
  </div>
  <script>/* 목차 스크롤 강조 스크립트 (기준 구현에서 그대로 복사) */</script>
</body>
```

### 목차 규칙

| 항목 | 규칙 |
|---|---|
| 번호 | `00`부터 두 자리. 본문 섹션의 `.num`과 반드시 같은 번호 |
| 링크 | `href="#섹션id"`. 섹션 id와 1:1 |
| 문구 | 목차는 짧게 (예: `실시간 흐름`), 본문 제목은 구체적으로 (예: `녹취 음성에서 상담 화면 STT까지`) |
| 강조 | 스크롤 위치에 따라 현재 섹션이 파란 배경(`.active`)으로 표시. 클릭 시 누른 항목이 즉시 강조되고 부드럽게 이동 |
| 하단 여백 | `.page`의 하단 패딩 `45vh`는 마지막 섹션도 화면 상단까지 올라오게 하기 위한 값이라 줄이지 않음 |
| 모바일 | 960px 이하에서 상단 가로 목차로 전환 (자동) |

---

## 3. 섹션 표준 순서 (경력기술서 상세 기준)

면접관이 **구조 → 내 역할 → 내가 만든 흐름 → 운영 → 문제 해결 → 성과** 순으로 읽도록 배치합니다.

| 번호 | id | 목차 문구 | 내용 | 사용 컴포넌트 | 필수 |
|---|---|---|---|---|---|
| 00 | `arch` | 아키텍처 | 전체 시스템 구조. 계층 × 흐름 표 | `.arch` + `.tiers` | 필수 |
| 01 | `role` | 규모와 역할 | 서비스 범위 · 처리량 · 포지션 · 기간 카드 4개 | `.stats` | 필수 |
| 02 | `realtime` 등 | 핵심 흐름 | 내가 개발한 핵심 데이터 흐름 1개를 시퀀스로 | `.seq` + `.seq-notes` | 필수 |
| 03 | `batch` 등 | 연동 · 배치 | 외부 시스템 연동, 배치 처리 흐름 | `.timeline` | 선택 |
| 04 | `deploy` | 배포 | 배포 · 운영 절차와 내 담당 단계 | `.timeline` (`.mine`) | 선택 |
| 05 | `trouble` | 트러블슈팅 | 1~2건. 문제 · 원인/어려움 · 해결 · 결과 | `.case` + `.ps` | 필수 |
| 06 | `results` | 핵심성과 | 성과 카드 3개 | `.results` | 필수 |

- 선택 섹션이 빠지면 번호를 당겨서 다시 매깁니다 (목차와 본문 동시에).
- 섹션 사이에 구분선(`<hr>`)은 넣지 않습니다. 섹션 간 여백(`section { margin-top }`)으로 구분합니다.

---

## 4. 헤더

```html
<p class="kicker">경력기술서 · 2021.10 – 2022.10 · KT / 퓨렌스</p>
<h1>프로젝트 정식 명칭</h1>
<p class="meta">주요 역할 · 환경 한 줄</p>
<p class="lead">
  무엇을 하는 시스템인지 1문장.
  <strong>내가 개발한 범위</strong>를 1문장.
</p>
<div class="chips"><span>기술</span>...</div>
```

- `.lead`는 2문장 이내. 굵게는 **내 담당 범위 한 곳**만.
- `.chips`는 실제로 이 프로젝트에서 쓴 기술만. 10개 안팎.

---

## 5. 컴포넌트

### 5-1. 아키텍처 표 `.arch` > `.tiers`

행 = 계층, 열 = 흐름. 서버 구조가 한눈에 보이는 것이 목적입니다.

```
            │ 흐름 A      │ 흐름 B      │ 흐름 C
CLIENT      │ node        │ node        │ node
L4 VIP      │ wire + vip  │ wire + vip  │ wire + vip
SERVER      │ pair        │ pair        │ pair
DATA        │ pair / node │ node        │ node / .cell.empty
```

- 그리드 순서: `.corner` + `.flow-head` 3개 → 행마다 `.tier` + `.cell` 3개. 열이 3개가 아니면 `project.css`의 `.tiers` 컬럼 수와 `4n + k` 색 선택자를 함께 수정합니다.
- 열 색은 자동: 1열 파랑 `#2f6fd6`, 2열 초록 `#178a4c`, 3열 보라 `#7c5cff` (`--flow`, `--tint`).
- `.node` — 서버/클라이언트 박스. `<b>이름</b><span>설명</span>`.
- `.node.vip` — 점선 박스 + 자동 `L4` 배지.
- `.node.zk` — 코디네이터(ZooKeeper 등). 자동 `ZK` 배지.
- `.pair[data-link="이중화"]` — 1·2번 서버 쌍. 가운데 배지 문구는 `data-link` (`이중화`, `복제` 등).
- `.wire` — 층 사이 연결선 + 화살표. 프로토콜은 `<span>HTTP</span>`. 양방향은 `.wire.both`.
- `.cell.empty` — 해당 계층에 요소가 없을 때 빗금.
- `.arch-bar` 아래가 아니라 표 아래에 `.live`(강조 경로 한 줄)를 둘 수 있습니다.

### 5-1b. 망 구간 다이어그램 `.arch` > `.zones`

외부망 · 중계망 · 내부망처럼 **네트워크 구간을 건너는 구조**일 때 `.tiers` 대신 사용합니다. 열 = 망 구간, 행 = 트래픽 종류.

```
            │ 외부망  →  │ 중계망       →  │ 내부운영망
녹취 TCP    │ node   hop │ vip + pair  hop │ vip + pair
화면 HTTP   │ node   hop │ node        hop │ vip + pair
```

- 그리드 순서: `.corner` + (`.zone-head z1`, `.zone-gap`, `.zone-head z2`, `.zone-gap`, `.zone-head z3`) → 행마다 `.tier` + `.zcell z1` + `.hop` + `.zcell z2` + `.hop` + `.zcell z3`.
- 구간 색: `z1` 파랑(외부), `z2` 주황 `#b7791f`(중계), `z3` 초록(내부).
- `.hop` — 구간 사이 가로 화살표. 프로토콜은 `<span>TCP</span>`.
- `.node.mine` — 내가 직접 구현한 서버 (옅은 파랑). 아래 `.cell-note`로 "직접 구현" 표기.
- 다른 상세 페이지의 환경을 재사용하면 `.arch-foot`에 해당 페이지 링크를 둡니다.

### 5-2. 시퀀스 다이어그램 `.seq`

내가 만든 핵심 흐름을 **하나의 시퀀스로 통합**해서 그립니다 (수집 → 전달 → 표시 → 저장 → 조회).

```html
<div class="seq">
  <div class="seq-head"><div>참여자1</div>...7개</div>
  <div class="seq-body">
    <div class="lifelines"><i></i>...7개</div>
    <div class="phase">단계 이름</div>
    <div class="msg" style="grid-column: 1 / 3; --n: 2"><span>① 메시지</span></div>
    <div class="msg rev" style="grid-column: 3 / 7; --n: 4"><span>⑧ 역방향</span></div>
  </div>
</div>
```

- 참여자는 7개 기준 (`repeat(7, …)`). 바꾸면 `.seq-head`, `.seq-body`, `.lifelines` 컬럼 수를 같이 수정.
- `grid-column: 시작열 / 끝열+1`, `--n`은 걸치는 열 개수. 화살표가 첫 열 중앙에서 마지막 열 중앙까지 그려집니다.
- `.msg` → 오른쪽 방향 (파랑), `.msg.rev` → 왼쪽 방향 (점선), `.msg.ws` → 화면으로 push (청록).
- 메시지 번호는 `① ② ③` 원문자. 아래 `.seq-notes`의 `.why` 설명에서 같은 번호로 참조합니다.

### 5-3. 설명 박스 `.why`

다이어그램 아래에서 **설계 이유**를 설명합니다. 왼쪽 라벨 + 오른쪽 본문 2단.

```html
<div class="why"><b>④ WAS 이중화</b><p>왜 이렇게 설계했는지 2~3문장</p></div>
```

### 5-4. 세로 타임라인 `.timeline`

배치, 배포 절차처럼 **순서가 있는 과정**에 사용합니다. 가로 흐름도는 쓰지 않습니다.

```html
<div class="timeline">
  <div class="tl-item mine">
    <div class="tl-dot blue"></div>
    <div class="tl-phase blue">Step 01 / 단계 · 담당</div>
    <div class="tl-title">단계 제목</div>
    <div class="tl-desc">설명 한 문장</div>
  </div>
  <div class="tl-item fail">...실패 분기...</div>
</div>
```

| 클래스 | 의미 |
|---|---|
| `.tl-dot` (색 없음) | 내가 하지 않은 절차 단계 (회색) |
| `.tl-dot.blue` / `.tl-phase.blue` | 진행 단계 강조 |
| `.tl-item.mine` | **내가 담당한 단계** — 점이 채워짐. `tl-phase`에 `· 담당` 표기 |
| `.tl-dot.violet` | 처리 · 변환 단계 |
| `.tl-dot.green` | 완료 · 반영 단계 |
| `.tl-item.fail` + `.red` | 실패 · 장애 분기. 위에 점선 구분 |

### 5-5. 규모 카드 `.stats`

```html
<div class="stats">
  <div class="stat"><b>우리은행 전 지점</b><span>서비스 범위</span></div>
  ...4개
</div>
```

- 4개 고정: **범위 · 처리량 · 포지션 · 기간**. 카드 외 설명 문단은 넣지 않습니다.

### 5-6. 트러블슈팅 `.case` > `.ps`

```html
<article class="case">
  <h3><span>01</span>사례 제목</h3>
  <div class="ps">
    <div class="ps-row"><b>문제</b><p>...</p></div>
    <div class="ps-row"><b>원인</b><p>...</p></div>   <!-- 또는 어려움 -->
    <div class="ps-row"><b>해결</b><p>...</p></div>
    <div class="ps-row result"><b>결과</b><p>...</p></div>
  </div>
</article>
```

- 1~2건. 행 순서는 **문제 → 원인(또는 어려움) → 해결 → 결과** 고정.
- 결과 행(`.result`)은 파란 강조. 문장 하나로.
- 굵게는 행마다 최대 한 곳 (핵심 기술 · 핵심 원인).

### 5-6b. 기술 용어 인라인 표기 `<code>`

본문 문장 안의 **기술 용어 · 설정 키 · 도구 이름**은 `<code>`로 감싸 회색 칩으로 표시합니다. 예시 코드 블록은 넣지 않습니다.

```html
<p>직접 구현한 TCP Proxy에서 <code>idle timeout</code> 등으로 연결이 끊겼습니다.</p>
```

- 대상: 설정 · 헤더 (`Cache-Control`, `Keep-Alive`), 알고리즘 · 방식 (`Hash`, `SEED`), 도구 · 기법 (`AOP`, `grep`, `PCMS`), 에러 · 상태 (`OOM`, `timeout`), 메시지 필드 (`채널 ID`).
- 문장당 2~3개 이내. 이미 `.chips`에 있는 일반 기술 이름(Spring, Kafka 등)은 감싸지 않습니다.
- 제목(`h2`, `h3`), 다이어그램 노드, 카드 제목에는 쓰지 않고 **본문 문장에만** 씁니다.
- `<strong>` 안에 넣어도 됩니다 (`<strong><code>AOP</code>로 …</strong>`).

### 5-7. 핵심성과 `.results`

```html
<article class="result-card">
  <em>안정성</em>
  <b>성과 한 줄 (숫자가 있으면 숫자로)</b>
  <p>어떻게 달성했는지 1~2문장</p>
</article>
```

- 3개. 분류(`em`) 예: 안정성 · 운영 · 서비스 · 성능 · 자동화.
- 트러블슈팅과 내용이 겹쳐도 됩니다. 성과는 **결과 중심 한 줄**, 트러블슈팅은 **과정**.

---

## 6. 디자인 토큰

### 색

| 토큰 | 값 | 용도 |
|---|---|---|
| `--text` | `#1a1f2b` | 제목 · 본문 강조 |
| `--sub` | `#3f4756` | 본문 |
| `--muted` | `#7a8394` | 보조 설명 · 라벨 |
| `--line` | `#e6e9ef` | 테두리 · 구분선 |
| `--bg` | `#f4f6f9` | 페이지 배경 |
| `--paper` | `#ffffff` | 카드 · 박스 배경 |
| `--side` | `#f7f9fc` | 표 헤더 · 옅은 면 |
| `--accent` | `#2f6fd6` | 강조 · 링크 · 번호 · 내 담당 |

| 의미 색 | 값 | 용도 |
|---|---|---|
| 초록 | `#178a4c` | 2번째 흐름 · 완료 · 반영 |
| 보라 | `#7c5cff` | 3번째 흐름 · 처리 · 메시징 |
| 청록 | `#0e7490` | WebSocket · 화면 push |
| 빨강 | `#d64545` | 실패 · 장애 분기 |

- 한 컴포넌트 안에서 의미 색은 **최대 3개**. 장식용 색은 쓰지 않습니다.
- 그라데이션, 그림자 강조, hover 효과는 쓰지 않습니다.

### 타이포그래피

| 요소 | 크기 | 굵기 |
|---|---|---|
| 기본 | 15px, 줄간격 1.6 | 400 |
| `h1` | 1.85rem | 700 |
| `h2` (섹션 제목) | 1.35rem | 700 |
| `h3` (사례 제목) | 1.02rem | 700 |
| `.num` (섹션 번호) | 0.78rem, 파랑 | 800 |
| 라벨 (`tl-phase`, `tier`, `toc-label`) | 0.7~0.72rem | 800 |

- 폰트: Pretendard (CDN). `word-break: keep-all`.

### 형태

- 모서리: 카드 `10px`, 큰 다이어그램 `14px`, 노드 `8px`, 배지 `999px`.
- 테두리: `1px solid var(--line)`. 의미를 가진 박스만 윗선 2~3px 색.

---

## 7. 작성 원칙

1. **사실만 쓴다.** 본인에게 확인한 내용만 넣고, 모르는 값(개수 · 주기 · 알고리즘 · 수치)은 비워 둔다. 추정으로 채우지 않는다.
2. **내가 한 일과 주어진 환경을 구분한다.** 이미 구성된 인프라는 "구성되어 있었다", 내가 한 건 "개발 · 설계 · 조언"으로 쓴다. 타임라인에서는 `.mine`으로 표시.
3. **설계 이유를 쓴다.** 다이어그램마다 `.why`로 "왜 이렇게 했는지"를 붙인다 (예: 녹취 Hash, 채널 ID 필터, 청크 규격).
4. **면접 질문을 미리 막는다.** 설명이 기술적으로 성립하는지 확인한다 (예: Kafka 컨슈머 그룹 동작과 설명이 맞는지).
5. **문장은 짧게.** 카드 설명 1~2문장, 타임라인 설명 1문장, `.lead` 2문장.
6. **굵게는 아껴 쓴다.** 문단당 한 곳, 핵심 기술 또는 내 담당 범위.
7. **숫자는 근거가 있을 때만.** 없으면 정성 결과로 쓴다 (예: "grep으로 바로 조회").
8. **메인 이력서와 표현을 맞춘다.** 상세 페이지 내용이 바뀌면 `index.html`의 해당 프로젝트 bullet도 같이 확인한다.

---

## 8. 새 상세 페이지 체크리스트

- [ ] `woori-misselling.html` 복사 → `<고객사>-<주제>.html`
- [ ] `<title>`, `.toc-title`, `.toc-meta`, `.kicker`, `h1`, `.meta`, `.lead`, `.chips` 교체
- [ ] 아키텍처 확인 질문: 계층 · 흐름 · 이중화 · 분산 방식 · 저장소
- [ ] 역할 확인 질문: 포지션 · 직접 코드 작성 범위 · 인프라 관여 방식 · 배포 방식
- [ ] 규모 확인 질문: 서비스 범위 · 처리량 · 기간
- [ ] 트러블슈팅 · 성과 확인 질문: 현상 · 원인 · 해결 방법 · 결과(수치 여부)
- [ ] 섹션 번호 ↔ 목차 번호 ↔ 섹션 id 일치
- [ ] 선택 섹션을 뺐다면 번호 재정렬
- [ ] `index.html` 프로젝트 제목에 링크 추가
- [ ] 1440px(데스크톱) · 960px 이하(모바일 목차) 화면 확인
