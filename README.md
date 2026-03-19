# 포트폴리오 웹사이트 제작


## 01. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 프로젝트명 | 프론트엔드 개발자 포트폴리오 웹사이트 |
| 제작 기간 | 2026.03.15 — 2026.03.18 |
| 사용 기술 | HTML5, CSS3 |
| 외부 리소스 | Google Fonts, Font Awesome 6 |
| 파일 구성 | `index.html`, `style.css` |

### 기획 의도

> **HTML과 CSS만**으로 제작한 반응형 개인 포트폴리오 웹사이트로,
> 자기소개·경력·프로젝트·활동·수상 정보를 한 페이지에 담았습니다.

---

## 02. 화면 구성 및 레이아웃

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
├── 02. Career (경력)     └── 타임라인 형식
├── 03. Projects (프로젝트) └── 2열 카드 그리드
├── 04. Experience (활동)  └── 기간·분류·설명 리스트
├── 05. Awards (수상)      └── 2열 아이콘 카드
└── Footer
```





## Progress Bar

스크롤 위치에 따라 Navbar 하단 골드 바가 채워지는 효과.  

```css
.progress-bar {
  transform-origin: left;       /* 왼쪽을 기준점으로 */
  transform: scaleX(0);         /* 처음엔 너비 0 */
  animation: progress-grow linear both;
  animation-timeline: scroll(root block); /* 루트 스크롤과 연동 */
}

@keyframes progress-grow {
  from { transform: scaleX(0); } /* 스크롤 0%   → 바 없음 */
  to   { transform: scaleX(1); } /* 스크롤 100% → 바 가득 */
}
```

##  CSS Keyframe 애니메이션

### fadeUp — 요소 진입 효과

Hero 텍스트·버튼·소셜 링크 등에 `delay`를 달리 줘서 순차적으로 등장.
```css
/* 정의 */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0);    }
}

/* 적용 — Hero 요소마다 딜레이를 다르게 */
.hero-greeting { animation: fadeUp 0.8s ease 0.1s both; }
.hero-name     { animation: fadeUp 0.8s ease 0.2s both; }
.hero-title    { animation: fadeUp 0.8s ease 0.3s both; }
.hero-desc     { animation: fadeUp 0.8s ease 0.4s both; }
.hero-cta      { animation: fadeUp 0.8s ease 0.5s both; }
.hero-social   { animation: fadeUp 0.8s ease 0.6s both; }
```

섹션 카드는 뷰포트 진입 시점에 재생되도록 `animation-timeline: view()` 사용.

---

### scrollDot — 스크롤 힌트 점 반복 이동

Hero 하단 마우스 아이콘 안의 점이 아래로 흘러내리는 효과.
```css
.scroll-dot {
  animation: scrollDot 1.8s ease infinite;
}

@keyframes scrollDot {
  0%,100% { opacity: 1; transform: translateY(0);  }
  50%      { opacity: 0.3; transform: translateY(8px); }
}
```



## 03-3. 반응형 웹 — Media Query

화면 너비에 따라 레이아웃이 자동으로 변경됩니다.

### 브레이크포인트 구성

```
데스크톱 (1080px~)   2열 카드, 2×2 About 그리드, 전체 네비게이션
       ↓ 900px
태블릿  (680px~900px) About 컬럼 축소, 타임라인 레이아웃 조정
       ↓ 680px
모바일  (420px~680px) 1열 전환, 햄버거 메뉴 활성화
       ↓ 420px
소형    (~420px)      버튼 전체 너비, 스킬 아이콘 크기 축소
```

---

### ① 태블릿 (max-width: 900px)

```css
@media (max-width: 900px) {

  /* About 좌측 컬럼 폭 축소 */
  .about-grid {
    grid-template-columns: 180px 1fr;
  }

  /* 프로젝트·수상: 2열 → 1열 */
  .projects-grid,
  .awards-grid {
    grid-template-columns: 1fr;
  }

  /* 타임라인: 헤더를 세로로 쌓기 */
  .timeline-header {
    flex-direction: column;
    gap: 8px;
  }
}
```

---

### ② 모바일 (max-width: 680px)

```css
@media (max-width: 680px) {

  /* 네비게이션 바 높이 축소 */
  :root { --nav-h: 60px; }

  /* 데스크톱 메뉴 숨기고, 햄버거·모바일 메뉴 표시 */
  .nav-links   { display: none; }
  .hamburger   { display: flex; }
  .mobile-menu { display: flex; }

  /* About 섹션: 2열 → 1열, 순서대로 쌓기 */
  .about-grid { grid-template-columns: 1fr; }
  .about-photo-wrap { grid-column: 1; grid-row: 1; }
  .about-intro      { grid-column: 1; grid-row: 2; }
  .about-info-card  { grid-column: 1; grid-row: 3; }
  .skills-wrap      { grid-column: 1; grid-row: 4; }

  /* 활동 섹션: 2열 → 1열 */
  .activity-item {
    grid-template-columns: 1fr;
  }
}
```

---

### ③ 소형 모바일 (max-width: 420px)

```css
@media (max-width: 420px) {

  /* 버튼이 화면 전체 너비를 차지하도록 */
  .hero-cta { flex-direction: column; }
  .btn-primary,
  .btn-secondary { width: 100%; justify-content: center; }

  /* 스킬 아이콘 크기 줄이기 */
  .skill-icon-item {
    min-width: 62px;
    padding: 12px 14px 10px;
  }
}
```

