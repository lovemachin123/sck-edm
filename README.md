# CAD 설계 자가진단 — Design Diagnosis Tool

> AutoCAD 사용자를 위한 5분 자가진단 웹앱. 21개 질문으로 5개 영역(자동화·전문성·협업·확장성·이동성) 점수를 산출하고, 10가지 유형 중 하나로 진단하여 가장 적합한 AutoCAD 솔루션을 추천합니다.

[![License: MIT](https://img.shields.io/badge/license-MIT-1a1a1a.svg?style=flat-square)](LICENSE)
[![Pages](https://img.shields.io/badge/GitHub-Pages-ff2e63.svg?style=flat-square&logo=github)](#-github-pages-배포)
[![Zero deps](https://img.shields.io/badge/dependencies-0-success.svg?style=flat-square)](#-기술-스택)

---

## 🔗 Live Demo

배포 후 본 섹션을 다음 형태로 갱신하세요.

```
https://<your-github-username>.github.io/<repo-name>/
```

---

## ✨ 주요 기능

| 영역 | 내용 |
|---|---|
| **진단 설문** | 21개 질문 · 단일선택 / 복수선택 / 자유서술 지원 |
| **5축 레이더 차트** | 외부 라이브러리 없이 SVG로 직접 렌더링 |
| **10가지 진단 유형** | 상위 2개 차원 조합 (`exp-mob` 등) 으로 자동 판별 |
| **맞춤 CAD 추천** | AutoCAD Plus / Web / LT / Standard 점수 기반 추천 |
| **다크 모드** | `prefers-color-scheme` 자동 감지 |
| **인쇄/PDF** | 결과 페이지 인쇄 최적화 (`@media print`) |
| **마이크로 인터랙션** | IntersectionObserver reveal · 점수 카운트업 · 호버 lift |
| **접근성** | 키보드(←/→/Enter) 지원, 시맨틱 마크업 |

---

## 🧰 기술 스택

- **Vanilla HTML / CSS / JavaScript** — 단일 파일, 빌드 도구 없음
- **Pretendard / Inter / JetBrains Mono** — 웹폰트만 외부 로드
- **SVG** — 레이더 차트, 아이콘, 일러스트 모두 인라인
- **Zero Dependencies** — npm·번들러·프레임워크 없음

---

## 🚀 GitHub Pages 배포

### 옵션 A. 브랜치 직접 배포 (가장 간단)

```bash
git init
git add .
git commit -m "feat: initial release"
git branch -M main
git remote add origin https://github.com/<USERNAME>/<REPO>.git
git push -u origin main
```

GitHub 리포지토리 페이지에서:
1. **Settings** → **Pages**
2. **Source**: `Deploy from a branch`
3. **Branch**: `main` / `/ (root)`
4. **Save** → 1~2분 후 `https://<USERNAME>.github.io/<REPO>/` 에 배포 완료

### 옵션 B. GitHub Actions 자동 배포 (권장)

이 리포지토리에는 `.github/workflows/deploy.yml` 워크플로가 포함되어 있어, `main` 브랜치에 push 할 때마다 자동으로 GitHub Pages에 배포됩니다.

1. **Settings** → **Pages** → **Source**: `GitHub Actions` 선택
2. `main` 에 push → Actions 탭에서 빌드 진행 확인
3. 완료 후 같은 URL로 접속 가능

### 배포 후 필수 작업

`index.html` 헤드의 다음 placeholder를 실제 도메인으로 치환하세요.

```diff
- <link rel="canonical" href="https://USERNAME.github.io/cad-diagnosis/" />
+ <link rel="canonical" href="https://yourname.github.io/your-repo/" />
```

해당 위치는 `canonical` · `og:url` · `og:image` · `twitter:image` 4곳입니다. (각각 1줄씩, 일괄 치환하시면 됩니다.)

---

## 💻 로컬 실행

빌드 도구가 필요 없습니다. 아래 셋 중 한 가지를 선택하세요.

```bash
# 1. 가장 빠른 방법 — 그냥 브라우저로 더블클릭
open index.html

# 2. Python 내장 서버
python3 -m http.server 8080
# → http://localhost:8080

# 3. Node 사용 시
npx serve .
```

---

## 📁 프로젝트 구조

```
.
├── index.html                  # 웹앱 본체 (단일 파일)
├── og-image.svg                # 소셜 미리보기 이미지 (1200×630)
├── README.md                   # 본 문서
├── LICENSE                     # MIT
├── .gitignore
└── .github/
    └── workflows/
        └── deploy.yml          # GitHub Pages 자동 배포
```

---

## 🛠️ 커스터마이징 가이드

### 질문 변경
`index.html` 의 `QUESTIONS` 배열을 수정합니다. 각 질문은 다음 필드를 가집니다.

```js
{
  id: 'unique-id',         // 응답 저장 키
  cat: '카테고리 표시명',
  catKey: 'auto',          // CAT_META 키 (auto/expert/collab/ext/mob/info/fb)
  type: 'single',          // single | multi | text
  dim: 'auto',             // 점수 차원 (없으면 점수 계산 제외)
  q: '질문 본문',
  options: [...],
  weight: [100, 75, 50, 0] // 옵션별 가중치
}
```

### 진단 유형 추가/수정
`TYPE_MAP` 에 새 키를 추가하세요. 키 형식은 **두 차원의 알파벳 정렬 조합**입니다.

```js
'auto-exp':  { ... },  // 자동화 + 전문성
'exp-mob':   { ... },  // 전문성 + 이동성
'col-ext':   { ... },  // 협업 + 확장성
```

차원 키: `auto`, `exp`, `col`, `ext`, `mob`

### 색상 테마
HTML 상단 `:root` 의 CSS 변수를 수정하면 전체 톤이 바뀝니다.

```css
--brand:        #ff2e63;   /* 포인트 컬러 */
--brand-soft:   #ffe8ee;
--ink:          #0a0a0a;   /* 헤드라인·본문 핵심 */
--bg:           #f4f1ec;   /* 페이지 배경 */
```

### 카테고리별 색상
질문 화면 카테고리 색은 `:root --cat-*` 토큰 + `CAT_META` 객체에서 관리됩니다.

```css
--cat-auto:   #2563eb;   /* 자동화 */
--cat-expert: #7c3aed;   /* 전문성 */
--cat-collab: #0891b2;   /* 협업 */
--cat-ext:    #c2410c;   /* 확장성 */
--cat-mob:    #059669;   /* 이동성 */
```

---

## 🌐 브라우저 호환성

| 브라우저 | 지원 |
|---|---|
| Chrome / Edge | 최신 2개 버전 |
| Safari | 14+ |
| Firefox | 최신 2개 버전 |
| iOS / Android | 최신 |

CSS 기능: `aspect-ratio`, `prefers-color-scheme`, `backdrop-filter`, SVG `r` CSS 프로퍼티 사용.

---

## 📜 라이선스

[MIT](LICENSE) © 2026

---

## 🙋 FAQ

**Q. 응답 데이터는 어디로 전송되나요?**
A. 어디로도 전송되지 않습니다. 모든 진단은 브라우저 메모리 내에서 처리되며 새로고침 시 사라집니다.

**Q. PDF로 결과 저장 가능한가요?**
A. 결과 페이지 하단의 **"결과 인쇄"** 버튼 → 인쇄 대화상자 → "PDF로 저장"을 선택하세요.

**Q. 모바일 앱으로도 쓸 수 있나요?**
A. 모바일 브라우저로 접속 후 "홈 화면에 추가"하면 PWA처럼 사용 가능합니다.
