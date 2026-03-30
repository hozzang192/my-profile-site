# 호짱쌤 프로필 웹사이트 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 경남 소상공인·중소기업 대상 정책자금 컨설턴트 호짱쌤의 랜딩페이지를 단일 HTML 파일로 구축한다.

**Architecture:** 단일 `index.html` 파일에 인라인 CSS 커스텀 변수와 Tailwind CDN을 혼합 사용. 7개 섹션을 순서대로 구성하고 Vanilla JS로 네비게이션 토글 및 스크롤 애니메이션 처리. 서버 불필요, 정적 파일로 배포 가능.

**Tech Stack:** HTML5, Tailwind CSS CDN (v3), Google Fonts (Noto Sans KR), Vanilla JavaScript (IntersectionObserver)

---

## 파일 구조

```
my-profile-site/
├── index.html          # 전체 페이지 (HTML + 인라인 스타일 + JS)
└── docs/
    └── superpowers/
        ├── specs/2026-03-31-profile-site-design.md
        └── plans/2026-03-31-profile-site.md
```

---

### Task 1: HTML 뼈대 및 헤드 설정

**Files:**
- Create: `index.html`

- [ ] **Step 1: index.html 기본 뼈대 생성**

`index.html` 파일을 아래 내용으로 생성한다:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>호짱쌤 | 금융정책자금 컨설턴트</title>
  <meta name="description" content="27년 경력 정책자금 전문가. 경남 소상공인·중소기업 정책자금 매칭, 재무분석, 기업인증 지원 컨설팅." />
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700;900&display=swap" rel="stylesheet" />
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            navy: '#1B2A4A',
            'navy-dark': '#111d33',
            gold: '#C9A84C',
            'gold-light': '#e0bf6e',
          },
          fontFamily: {
            sans: ['Noto Sans KR', 'sans-serif'],
          },
        },
      },
    }
  </script>
  <style>
    html { scroll-behavior: smooth; }
    .fade-in { opacity: 0; transform: translateY(24px); transition: opacity 0.6s ease, transform 0.6s ease; }
    .fade-in.visible { opacity: 1; transform: translateY(0); }
    .hero-pattern {
      background-image: radial-gradient(circle at 80% 20%, rgba(201,168,76,0.15) 0%, transparent 50%),
                        radial-gradient(circle at 10% 80%, rgba(201,168,76,0.1) 0%, transparent 40%);
    }
    .service-card:hover { border-top-color: #C9A84C; box-shadow: 0 8px 30px rgba(27,42,74,0.12); }
    .nav-scrolled { background: rgba(27,42,74,0.95); backdrop-filter: blur(8px); }
  </style>
</head>
<body class="font-sans text-gray-800 bg-white">

  <!-- 섹션들이 여기에 추가됨 -->

  <script>
    // JavaScript는 Task 7에서 추가
  </script>
</body>
</html>
```

- [ ] **Step 2: 브라우저에서 열어 빈 페이지 확인**

`index.html`을 브라우저로 열어 흰 빈 페이지가 에러 없이 로드되는지 확인. 개발자 도구 콘솔에 에러 없어야 함.

- [ ] **Step 3: Commit**

```bash
git init
git add index.html
git commit -m "feat: HTML 뼈대 및 Tailwind/폰트 설정"
```

---

### Task 2: 네비게이션 섹션

**Files:**
- Modify: `index.html` — `<!-- 섹션들이 여기에 추가됨 -->` 위치에 추가

- [ ] **Step 1: 네비게이션 HTML 추가**

`<!-- 섹션들이 여기에 추가됨 -->` 주석을 아래 코드로 교체:

```html
  <!-- 네비게이션 -->
  <nav id="navbar" class="fixed top-0 left-0 right-0 z-50 transition-all duration-300" style="background: #1B2A4A;">
    <div class="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
      <a href="#hero" class="text-2xl font-black" style="color: #C9A84C;">호짱쌤</a>
      <!-- 데스크탑 메뉴 -->
      <div class="hidden md:flex items-center gap-8">
        <a href="#about" class="text-white hover:text-yellow-300 transition-colors text-sm font-medium">소개</a>
        <a href="#services" class="text-white hover:text-yellow-300 transition-colors text-sm font-medium">서비스</a>
        <a href="#contact" class="text-white hover:text-yellow-300 transition-colors text-sm font-medium">연락처</a>
        <a href="#kakao" class="px-5 py-2 rounded-full font-bold text-sm transition-colors" style="background:#C9A84C; color:#1B2A4A;">무료 상담</a>
      </div>
      <!-- 모바일 햄버거 버튼 -->
      <button id="menu-btn" class="md:hidden flex flex-col gap-1.5 p-2" aria-label="메뉴 열기">
        <span class="block w-6 h-0.5 bg-white transition-all"></span>
        <span class="block w-6 h-0.5 bg-white transition-all"></span>
        <span class="block w-6 h-0.5 bg-white transition-all"></span>
      </button>
    </div>
    <!-- 모바일 드롭다운 메뉴 -->
    <div id="mobile-menu" class="hidden md:hidden px-4 pb-4" style="background:#1B2A4A;">
      <a href="#about" class="block py-3 text-white border-b border-white/10 text-sm font-medium">소개</a>
      <a href="#services" class="block py-3 text-white border-b border-white/10 text-sm font-medium">서비스</a>
      <a href="#contact" class="block py-3 text-white text-sm font-medium">연락처</a>
      <a href="#kakao" class="block mt-3 text-center px-5 py-3 rounded-full font-bold text-sm" style="background:#C9A84C; color:#1B2A4A;">무료 상담받기</a>
    </div>
  </nav>

  <!-- 섹션들이 여기에 추가됨 (Task 3~6) -->
```

- [ ] **Step 2: 브라우저에서 확인**

페이지 상단에 네이비 네비게이션 바가 보이고, 모바일 뷰(개발자도구 375px)에서 햄버거 아이콘 3줄이 보이는지 확인. (JS 없으므로 클릭은 아직 동작 안 함)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: 고정 네비게이션 바 추가"
```

---

### Task 3: Hero 섹션 + 신뢰지표 섹션

**Files:**
- Modify: `index.html` — `<!-- 섹션들이 여기에 추가됨 (Task 3~6) -->` 위치에 추가

- [ ] **Step 1: Hero + 신뢰지표 HTML 추가**

`<!-- 섹션들이 여기에 추가됨 (Task 3~6) -->` 주석을 아래 코드로 교체:

```html
  <!-- Hero 섹션 -->
  <section id="hero" class="hero-pattern pt-24 pb-20 px-4 min-h-screen flex items-center" style="background-color:#1B2A4A;">
    <div class="max-w-3xl mx-auto text-center fade-in">
      <p class="text-sm font-medium mb-4 tracking-widest uppercase" style="color:#C9A84C;">경남 소상공인·중소기업 전문</p>
      <h1 class="text-4xl md:text-6xl font-black text-white leading-tight mb-4">
        27년 경력<br/>정책자금 전문가
      </h1>
      <h2 class="text-xl md:text-2xl font-medium mb-4" style="color:#C9A84C;">
        경남 소상공인·중소기업의 든든한 파트너
      </h2>
      <p class="text-white/70 text-base md:text-lg mb-10 max-w-xl mx-auto">
        1999년부터 여신심사역으로 시작한 실전 컨설턴트
      </p>
      <a href="#kakao"
         class="inline-block w-full md:w-auto px-10 py-4 rounded-full text-lg font-black transition-all hover:scale-105"
         style="background:#C9A84C; color:#1B2A4A;">
        카카오톡으로 무료 상담받기 →
      </a>
    </div>
  </section>

  <!-- 신뢰지표 섹션 -->
  <section class="py-16 px-4 bg-white">
    <div class="max-w-4xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-8 text-center">
      <div class="fade-in p-8 rounded-2xl" style="background:#F9FAFB;">
        <p class="text-5xl font-black mb-2" style="color:#1B2A4A;">27년</p>
        <p class="text-gray-500 font-medium">전문 컨설팅 경력</p>
      </div>
      <div class="fade-in p-8 rounded-2xl" style="background:#F9FAFB;">
        <p class="text-5xl font-black mb-2" style="color:#1B2A4A;">1999년</p>
        <p class="text-gray-500 font-medium">금융기관 여신심사역으로 출발</p>
      </div>
      <div class="fade-in p-8 rounded-2xl" style="background:#F9FAFB;">
        <p class="text-5xl font-black mb-2" style="color:#C9A84C;">경남 전문</p>
        <p class="text-gray-500 font-medium">소상공인·중소기업 특화</p>
      </div>
    </div>
  </section>

  <!-- 섹션들이 여기에 추가됨 (Task 4~6) -->
```

- [ ] **Step 2: 브라우저에서 확인**

- 네이비 배경에 흰 제목 "27년 경력 정책자금 전문가" 표시
- 골드 CTA 버튼 표시
- 그 아래 3개 신뢰지표 카드 표시
- 모바일(375px)에서 카드가 1열로 쌓이는지 확인

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: Hero 섹션 및 신뢰지표 섹션 추가"
```

---

### Task 4: 소개 섹션

**Files:**
- Modify: `index.html` — `<!-- 섹션들이 여기에 추가됨 (Task 4~6) -->` 위치에 추가

- [ ] **Step 1: 소개 섹션 HTML 추가**

`<!-- 섹션들이 여기에 추가됨 (Task 4~6) -->` 주석을 아래 코드로 교체:

```html
  <!-- 소개 섹션 -->
  <section id="about" class="py-20 px-4" style="background:#F9FAFB;">
    <div class="max-w-5xl mx-auto">
      <h2 class="text-3xl font-black text-center mb-14" style="color:#1B2A4A;">컨설턴트 소개</h2>
      <div class="flex flex-col md:flex-row items-center md:items-start gap-12">
        <!-- 프로필 이미지 (placeholder) -->
        <div class="flex-shrink-0 fade-in">
          <div class="w-48 h-48 rounded-full flex items-center justify-center text-6xl font-black text-white"
               style="background:#1B2A4A;">
            호
          </div>
        </div>
        <!-- 소개 텍스트 -->
        <div class="fade-in flex-1">
          <h3 class="text-2xl font-black mb-2" style="color:#1B2A4A;">호짱쌤</h3>
          <p class="font-medium mb-6" style="color:#C9A84C;">금융정책자금 컨설턴트</p>
          <p class="text-gray-600 leading-relaxed mb-8 text-base">
            1999년부터 금융기관 여신심사역으로 시작해 현재까지 중소기업·소상공인의 정책자금 매칭과 재무분석 컨설팅을 전문으로 합니다. 실전 사업자로서의 경험을 살려 최적의 솔루션을 제공합니다.
          </p>
          <!-- 경력 타임라인 -->
          <div class="space-y-4">
            <div class="flex gap-4 items-start">
              <div class="w-2 h-2 rounded-full mt-2 flex-shrink-0" style="background:#C9A84C;"></div>
              <div>
                <p class="font-bold text-sm" style="color:#1B2A4A;">1999년</p>
                <p class="text-gray-500 text-sm">금융기관 여신심사역 시작</p>
              </div>
            </div>
            <div class="flex gap-4 items-start">
              <div class="w-2 h-2 rounded-full mt-2 flex-shrink-0" style="background:#C9A84C;"></div>
              <div>
                <p class="font-bold text-sm" style="color:#1B2A4A;">2000년대</p>
                <p class="text-gray-500 text-sm">중소기업·소상공인 정책자금 전문 컨설팅 시작</p>
              </div>
            </div>
            <div class="flex gap-4 items-start">
              <div class="w-2 h-2 rounded-full mt-2 flex-shrink-0" style="background:#C9A84C;"></div>
              <div>
                <p class="font-bold text-sm" style="color:#1B2A4A;">현재</p>
                <p class="text-gray-500 text-sm">경남 소상공인·중소기업 재무분석 및 정책자금 매칭 전문</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- 섹션들이 여기에 추가됨 (Task 5~6) -->
```

- [ ] **Step 2: 브라우저에서 확인**

- 라이트그레이 배경에 "호" 이니셜 원형 아바타 표시
- 소개 텍스트 및 3단계 타임라인 표시
- 모바일에서 이미지가 텍스트 위에 쌓이는지 확인

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: 소개 섹션 및 경력 타임라인 추가"
```

---

### Task 5: 서비스 섹션

**Files:**
- Modify: `index.html` — `<!-- 섹션들이 여기에 추가됨 (Task 5~6) -->` 위치에 추가

- [ ] **Step 1: 서비스 섹션 HTML 추가**

`<!-- 섹션들이 여기에 추가됨 (Task 5~6) -->` 주석을 아래 코드로 교체:

```html
  <!-- 서비스 섹션 -->
  <section id="services" class="py-20 px-4 bg-white">
    <div class="max-w-5xl mx-auto">
      <h2 class="text-3xl font-black text-center mb-4" style="color:#1B2A4A;">주요 서비스</h2>
      <p class="text-center text-gray-500 mb-14">27년 경험을 바탕으로 기업 상황에 맞는 최적의 솔루션을 제공합니다</p>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
        <!-- 카드 1 -->
        <div class="service-card fade-in p-6 rounded-2xl border-t-4 transition-all cursor-default"
             style="border-top-color:#1B2A4A; background:#F9FAFB;">
          <div class="text-4xl mb-4">💰</div>
          <h3 class="font-black text-base mb-2" style="color:#1B2A4A;">정책자금 매칭</h3>
          <p class="text-gray-500 text-xs leading-relaxed">기업 상황에 맞는 최적 정책자금 연결</p>
        </div>
        <!-- 카드 2 -->
        <div class="service-card fade-in p-6 rounded-2xl border-t-4 transition-all cursor-default"
             style="border-top-color:#1B2A4A; background:#F9FAFB;">
          <div class="text-4xl mb-4">📊</div>
          <h3 class="font-black text-base mb-2" style="color:#1B2A4A;">기업재무분석</h3>
          <p class="text-gray-500 text-xs leading-relaxed">재무구조 진단 및 개선 방향 제시</p>
        </div>
        <!-- 카드 3 -->
        <div class="service-card fade-in p-6 rounded-2xl border-t-4 transition-all cursor-default"
             style="border-top-color:#1B2A4A; background:#F9FAFB;">
          <div class="text-4xl mb-4">🏆</div>
          <h3 class="font-black text-base mb-2" style="color:#1B2A4A;">기업인증 지원</h3>
          <p class="text-gray-500 text-xs leading-relaxed">벤처·이노비즈 등 인증 컨설팅</p>
        </div>
        <!-- 카드 4 -->
        <div class="service-card fade-in p-6 rounded-2xl border-t-4 transition-all cursor-default"
             style="border-top-color:#1B2A4A; background:#F9FAFB;">
          <div class="text-4xl mb-4">📋</div>
          <h3 class="font-black text-base mb-2" style="color:#1B2A4A;">정부지원금 컨설팅</h3>
          <p class="text-gray-500 text-xs leading-relaxed">지원금 신청 전략 수립 및 대행</p>
        </div>
      </div>
    </div>
  </section>

  <!-- 섹션들이 여기에 추가됨 (Task 6) -->
```

- [ ] **Step 2: 브라우저에서 확인**

- 4개 서비스 카드가 모바일(375px)에서 2열, 데스크탑에서 4열로 표시
- 각 카드 상단에 네이비 테두리 표시
- 카드에 마우스 올리면 골드 테두리로 변환

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: 서비스 4개 카드 섹션 추가"
```

---

### Task 6: CTA 배너 + 푸터 섹션

**Files:**
- Modify: `index.html` — `<!-- 섹션들이 여기에 추가됨 (Task 6) -->` 위치에 추가

- [ ] **Step 1: CTA 배너 + 푸터 HTML 추가**

`<!-- 섹션들이 여기에 추가됨 (Task 6) -->` 주석을 아래 코드로 교체:

```html
  <!-- CTA 배너 섹션 -->
  <section class="py-20 px-4 text-center" style="background:#1B2A4A;">
    <div class="max-w-2xl mx-auto fade-in">
      <h2 class="text-3xl md:text-4xl font-black text-white mb-4">
        정책자금, 혼자 고민하지 마세요
      </h2>
      <p class="text-white/70 text-lg mb-10">
        27년 경험의 전문가가 무료로 검토해 드립니다
      </p>
      <a href="#kakao"
         class="inline-block w-full md:w-auto px-12 py-4 rounded-full text-lg font-black transition-all hover:scale-105"
         style="background:#C9A84C; color:#1B2A4A;">
        지금 카카오톡 상담받기
      </a>
    </div>
  </section>

  <!-- 푸터 -->
  <footer id="contact" class="py-12 px-4" style="background:#111d33;">
    <div class="max-w-5xl mx-auto">
      <div class="flex flex-col md:flex-row justify-between items-center gap-6">
        <div>
          <p class="text-2xl font-black mb-1" style="color:#C9A84C;">호짱쌤</p>
          <p class="text-white/50 text-sm">금융정책자금 컨설턴트 | 경남 소상공인·중소기업 전문</p>
        </div>
        <div class="flex flex-col items-center md:items-end gap-3">
          <a href="mailto:ho070333@gmail.com"
             class="flex items-center gap-2 text-white/70 hover:text-white transition-colors text-sm">
            <span>✉️</span> ho070333@gmail.com
          </a>
          <a href="#kakao"
             class="flex items-center gap-2 font-bold text-sm transition-colors"
             style="color:#C9A84C;">
            <span>💬</span> 카카오톡 무료 상담
          </a>
        </div>
      </div>
      <div class="mt-8 pt-6 border-t border-white/10 text-center text-white/30 text-xs">
        © 2026 호짱쌤. All rights reserved.
      </div>
    </div>
  </footer>
```

- [ ] **Step 2: 브라우저에서 확인**

- 네이비 CTA 배너 섹션 표시 (골드 버튼)
- 다크 네이비 푸터에 이메일, 카카오 링크, 카피라이트 표시
- 전체 페이지 스크롤하여 7개 섹션 모두 연결되어 보이는지 확인

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: CTA 배너 및 푸터 섹션 추가"
```

---

### Task 7: JavaScript — 네비게이션 토글 + 스크롤 애니메이션

**Files:**
- Modify: `index.html` — `// JavaScript는 Task 7에서 추가` 주석을 교체

- [ ] **Step 1: JavaScript 추가**

`// JavaScript는 Task 7에서 추가` 주석을 아래 코드로 교체:

```javascript
    // 햄버거 메뉴 토글
    const menuBtn = document.getElementById('menu-btn');
    const mobileMenu = document.getElementById('mobile-menu');
    menuBtn.addEventListener('click', () => {
      mobileMenu.classList.toggle('hidden');
    });
    // 모바일 메뉴 링크 클릭 시 메뉴 닫기
    mobileMenu.querySelectorAll('a').forEach(link => {
      link.addEventListener('click', () => {
        mobileMenu.classList.add('hidden');
      });
    });

    // 스크롤 시 네비게이션 배경 효과
    const navbar = document.getElementById('navbar');
    window.addEventListener('scroll', () => {
      if (window.scrollY > 50) {
        navbar.classList.add('nav-scrolled');
      } else {
        navbar.classList.remove('nav-scrolled');
      }
    });

    // 스크롤 페이드인 (IntersectionObserver)
    const fadeEls = document.querySelectorAll('.fade-in');
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.15 });
    fadeEls.forEach(el => observer.observe(el));
```

- [ ] **Step 2: 브라우저에서 전체 기능 확인**

다음을 순서대로 확인:

1. 모바일(375px)에서 햄버거 버튼 클릭 → 드롭다운 메뉴 열림/닫힘
2. 드롭다운 메뉴 링크 클릭 → 해당 섹션으로 스크롤 + 메뉴 닫힘
3. 페이지 스크롤 → 네비게이션 배경 블러 처리
4. 각 섹션에 진입할 때 fade-in 애니메이션 (아래서 위로 올라오며 나타남)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: 네비게이션 토글 및 스크롤 페이드인 애니메이션 추가"
```

---

### Task 8: 카카오톡 URL 교체 및 최종 점검

**Files:**
- Modify: `index.html` — `#kakao` placeholder를 실제 URL로 교체

- [ ] **Step 1: 카카오톡 채널 URL 교체**

카카오톡 채널 URL을 확보한 경우, 아래 명령으로 모든 `#kakao`를 실제 URL로 일괄 교체:

```bash
# 예시: 카카오 채널 URL이 https://pf.kakao.com/_xABCDE 인 경우
sed -i 's|href="#kakao"|href="https://pf.kakao.com/_xABCDE" target="_blank" rel="noopener"|g' index.html
```

URL이 없으면 이 단계를 건너뛰고 `#kakao`를 그대로 유지.

- [ ] **Step 2: meta 태그 및 SEO 최종 점검**

`index.html` `<head>` 에서 아래 항목 확인:
- `<title>`: "호짱쌤 | 금융정책자금 컨설턴트" ✓
- `<meta name="description">`: 키워드 포함 설명 ✓
- `lang="ko"` ✓
- `viewport` meta ✓

- [ ] **Step 3: 모바일/데스크탑 최종 UI 점검 체크리스트**

브라우저 개발자도구에서 다음 확인:

| 항목 | 모바일(375px) | 데스크탑(1280px) |
|------|-------------|----------------|
| 네비게이션 | 햄버거 버튼 | 링크 + 상담 버튼 |
| Hero 버튼 | 풀너비 | 인라인 |
| 신뢰지표 | 1열 | 3열 |
| 서비스 카드 | 2열 | 4열 |
| 소개 섹션 | 세로 배치 | 가로 배치 |
| 푸터 | 중앙 정렬 | 좌우 분할 |

- [ ] **Step 4: 최종 Commit**

```bash
git add index.html
git commit -m "feat: 카카오톡 URL 교체 및 최종 점검 완료"
```

---

## Self-Review

**스펙 커버리지 점검:**
- [x] 네비게이션 (고정, 햄버거 메뉴, 골드 CTA) — Task 2
- [x] Hero 섹션 (네이비 배경, H1/H2, 골드 CTA) — Task 3
- [x] 신뢰지표 3개 (27년, 1999년, 경남 전문) — Task 3
- [x] 소개 섹션 (이니셜 아바타, 소개 텍스트, 타임라인) — Task 4
- [x] 서비스 4개 카드 (hover 효과, 2열→4열) — Task 5
- [x] CTA 배너 (네이비 배경, 골드 버튼) — Task 6
- [x] 푸터 (이메일, 카카오, 카피라이트) — Task 6
- [x] 스크롤 페이드인 애니메이션 — Task 7
- [x] 햄버거 메뉴 토글 — Task 7
- [x] 네비게이션 스크롤 블러 — Task 7
- [x] 카카오톡 URL placeholder 교체 — Task 8
- [x] 모바일 우선 반응형 — 모든 Task

**Placeholder 없음:** 모든 코드 블록 완전히 작성됨.

**타입/명칭 일관성:**
- `#kakao` — Task 2, 3, 6, 8 모두 동일
- `#navbar` — Task 2 정의, Task 7 참조
- `#mobile-menu` — Task 2 정의, Task 7 참조
- `.fade-in` / `.visible` — Task 1 CSS 정의, Task 3~6 사용, Task 7 JS 처리
- `.service-card` — Task 1 CSS 정의, Task 5 사용
- `.nav-scrolled` — Task 1 CSS 정의, Task 7 JS 적용

이상 없음.
