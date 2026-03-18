# 포트폴리오 웹사이트 제작


## 01. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 프로젝트명 | 프론트엔드 개발자 포트폴리오 웹사이트 |
| 제작 기간 | 2024.00.00 — 2024.00.00 |
| 사용 기술 | HTML5, CSS3 |
| 외부 리소스 | Google Fonts, Devicon, Font Awesome 6 |
| 파일 구성 | `index.html`, `style.css` |

### 기획 의도

> 순수 **HTML과 CSS만**으로 제작한 반응형 개인 포트폴리오 웹사이트로,
> 자기소개·경력·프로젝트·활동·수상 정보를 한 페이지에 담았습니다.



## 02. 화면 구성 및 레이아웃

### 전체 페이지 구조

```
index.html
│
├── 네비게이션 바 (고정, 스크롤 진행 바 포함)
│
├── Hero (랜딩) 섹션
│   └── 이름, 직무, 소개 한 줄, CTA 버튼, SNS 링크
│
├── 01. About Me (자기소개)
│   ├── [상단-좌] 프로필 사진
│   ├── [상단-우] 짧은 자기소개
│   ├── [하단-좌] 개인 정보 카드
│   └── [하단-우] 기술 스택 아이콘
│
├── 02. Career (경력)
│   └── 타임라인 형식
│
├── 03. Projects (프로젝트)
│   └── 2열 카드 그리드
│
├── 04. Experience (활동/경험)
│   └── 기간·분류·설명 리스트
│
├── 05. Awards (수상)
│   └── 2열 아이콘 카드
│
└── Footer
```




## 03. 핵심 구현 기술

### 3-1. CSS Scroll-Driven Animation — 스크롤 진행 바

JavaScript 없이 순수 CSS `animation-timeline` 속성을 사용해
상단 네비게이션 바에 스크롤 진행률을 시각화했습니다.

```css
.progress-bar {
  transform-origin: left;
  transform: scaleX(0);
  animation: progress-grow linear both;
  animation-timeline: scroll(root block); /* 페이지 스크롤에 연동 */
}

@keyframes progress-grow {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
```



### 3-2. CSS Scroll-Driven Animation — 네비게이션 배경

스크롤 120px 지점까지 배경이 투명 → 반투명 블러로 자연스럽게 전환됩니다.

```css
.navbar {
  animation: navbar-bg linear both;
  animation-timeline: scroll(root block);
  animation-range: 0px 120px; /* 0~120px 구간 동안 전환 */
}

@keyframes navbar-bg {
  from { background: transparent; }
  to   { background: rgba(11, 12, 16, 0.92);
         backdrop-filter: blur(20px); }
}
```



### 3-3. CSS Scroll-Driven Animation — 요소 등장 효과

각 섹션의 카드·타임라인 등이 화면에 진입할 때 아래에서 위로 페이드인됩니다.

```css
.timeline-item,
.project-card,
.award-card {
  animation: fadeUp 0.6s ease both;
  animation-timeline: view();              /* 뷰포트 진입 기준 */
  animation-range: entry 0% entry 25%;    /* 등장 시작~25% 지점 */
}
```



### 3-4. Checkbox Hack — 모바일 햄버거 메뉴

JavaScript 없이 `<input type="checkbox">`와 CSS `:checked` 선택자를 활용해
모바일 네비게이션 토글을 구현했습니다.

```html
<!-- HTML 구조 -->
<input type="checkbox" id="menu-toggle" class="menu-toggle-input" />
<header class="navbar">
  <label for="menu-toggle" class="hamburger"> ... </label>
</header>
<div class="mobile-menu"> ... </div>
```

```css
/* 체크 시 메뉴 표시 */
.menu-toggle-input:checked ~ .mobile-menu {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}

/* 체크 시 햄버거 → X 아이콘 변형 */
.menu-toggle-input:checked ~ header .hamburger span:nth-child(1) {
  transform: translateY(7px) rotate(45deg);
}
```



### 3-5. CSS Grid — About Me 2×2 레이아웃

`grid-column` / `grid-row`로 각 요소의 위치를 명시적으로 지정해
2×2 배치를 구현했습니다.

```css
.about-grid {
  display: grid;
  grid-template-columns: minmax(200px, 260px) 1fr;
  grid-template-rows: auto auto;
  gap: 20px;
}

.about-photo-wrap { grid-column: 1; grid-row: 1; } /* 상단-좌 */
.about-intro      { grid-column: 2; grid-row: 1; } /* 상단-우 */
.about-info-card  { grid-column: 1; grid-row: 2; } /* 하단-좌 */
.skills-wrap      { grid-column: 2; grid-row: 2; } /* 하단-우 */
```



### 3-6. 반응형 웹 (미디어 쿼리)

세 가지 브레이크포인트로 모든 기기 대응:

| 브레이크포인트 | 대상 기기 | 주요 변화 |
|---|---|---|
| `max-width: 900px` | 태블릿 | 2컬럼 → 컬럼 축소, 타임라인 레이아웃 조정 |
| `max-width: 680px` | 모바일 | 1열 전환, 햄버거 메뉴 활성화 |
| `max-width: 420px` | 소형 모바일 | 버튼 전체 너비, 스킬 아이콘 크기 축소 |



## 04. 구현 결과


### 사용한 외부 리소스 (CDN)

| 리소스 | 용도 |
|--------|------|
| Google Fonts — Sora, Noto Sans KR | 본문·헤딩 폰트 |
| Devicon | 기술 스택 아이콘 (HTML, React 등) |
| Font Awesome 6 | UI 아이콘 (GitHub, 수상, 링크 등) |








