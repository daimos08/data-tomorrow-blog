# 데이터로 미리 보는 내일 — Quarto 블로그

공공데이터와 예측모델로 한국 사회의 지금과 앞으로를 풀어 쓰는 블로그입니다. 글, 데이터, 분석 코드가 한 저장소에 함께 있습니다.

## 폴더 구조

```
data-tomorrow-blog/
├── _quarto.yml              사이트 설정 (제목, 메뉴, 테마)
├── index.qmd                첫 화면 = 글 목록
├── about.qmd                소개
├── data.qmd                 데이터 목록 페이지
├── posts/
│   ├── _metadata.yml        모든 글의 공통 설정 (저자 등)
│   ├── welcome/index.qmd    첫 글: 블로그를 시작하며
│   └── missing-persons/     실종 데이터 분석 글 (Python + Plotly)
├── data/missing/            글에 쓴 CSV와 출처 목록
├── theme.scss, theme-dark.scss, styles.css, _includes/head.html   디자인
├── requirements.txt         Python 패키지
└── .github/workflows/publish.yml   push하면 자동 배포
```

## 1. 내 컴퓨터에서 미리 보기

1. Quarto 설치: <https://quarto.org/docs/get-started/>
2. Python 패키지 설치
   ```bash
   python -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. 미리 보기 (저장하면 브라우저가 자동으로 새로고침됩니다)
   ```bash
   quarto preview
   ```

## 2. GitHub Pages로 공개하기 (처음 한 번)

1. GitHub에 `data-tomorrow-blog` 저장소를 만들고 이 폴더를 올립니다.
   ```bash
   git init && git add . && git commit -m "첫 커밋"
   git branch -M main
   git remote add origin https://github.com/내아이디/data-tomorrow-blog.git
   git push -u origin main
   ```
2. 배포용 `gh-pages` 브랜치를 한 번 만들어 줍니다.
   ```bash
   quarto publish gh-pages
   ```
3. GitHub 저장소 → Settings → Pages에서 Source를 `gh-pages` 브랜치로 지정합니다.
4. `_quarto.yml`, `about.qmd`, `data.qmd`의 `YOUR-GITHUB-ID`를 실제 아이디로 바꿉니다.

이후에는 `main`에 push할 때마다 GitHub Actions가 사이트를 다시 만들어 배포합니다.

## 3. 새 글 쓰기

1. `posts/` 아래에 새 폴더를 만들고 `index.qmd`를 둡니다. 예: `posts/births-2026/index.qmd`
2. 맨 위에 다음 머리말을 넣습니다.
   ```yaml
   ---
   title: "아기 울음소리는 다시 커질까"
   date: 2026-10-20
   categories: [인구, 출생]
   description: "목록에 보일 한 줄 요약"
   jupyter: python3        # 코드가 없는 글이면 이 줄을 지웁니다
   ---
   ```
3. 데이터는 `data/<주제>/` 폴더에 CSV로 두고, 출처는 같은 폴더의 `sources.csv`에 적습니다.
4. `quarto preview`로 확인한 뒤 commit, push 하면 끝입니다.

Jupyter 노트북(`.ipynb`)으로 쓰고 싶다면 `index.ipynb`로 두어도 됩니다. 첫 셀을 Raw 셀로 만들고 위의 머리말을 넣으면 됩니다.

`_quarto.yml`의 `freeze: auto` 덕분에, 한 번 렌더링한 글은 내용이 바뀌지 않는 한 다시 계산하지 않습니다. 렌더링 결과가 담긴 `_freeze/` 폴더도 함께 commit하세요.

## 4. 선택 기능

- **방문 통계:** [GoatCounter](https://www.goatcounter.com/)에서 무료 계정을 만든 뒤 `_includes/head.html`의 주석을 풀고 코드를 넣습니다. 어떤 글을 많이 읽는지 확인할 수 있습니다.
- **댓글:** GitHub Discussions 기반의 giscus를 붙일 수 있습니다. `posts/_metadata.yml`의 `comments:`를 아래처럼 바꿉니다 (값은 <https://giscus.app>에서 생성).
  ```yaml
  comments:
    giscus:
      repo: 내아이디/data-tomorrow-blog
  ```
- **브라우저 안에서 계산하는 체험 페이지:** Quarto의 Observable JS(`{ojs}` 코드 블록)를 쓰면 입력값을 서버로 보내지 않는 계산기를 글 안에 넣을 수 있습니다. <https://quarto.org/docs/interactive/ojs/>
- **개인 도메인:** 저장소 Settings → Pages → Custom domain에서 연결합니다.

## 라이선스

글은 CC BY 4.0, 코드는 MIT로 공개하는 것을 기본으로 했습니다. 바꾸려면 `_quarto.yml`의 `page-footer`를 수정하세요.
