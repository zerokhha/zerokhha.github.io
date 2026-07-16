# ZeroKhha Portfolio (Jekyll)

개인 포트폴리오 사이트 저장소입니다.
This repository contains ZeroKhha's personal portfolio website.

## Intro

- KR: 안녕하세요, 저는 Data Engineer ZeroKhha입니다.
- EN: Hello, I'm ZeroKhha, a Data Engineer.

이 프로젝트는 Jekyll 기반으로 구성되어 있으며, 데이터 엔지니어링 프로젝트 경험을 포트폴리오 형태로 정리합니다.
This site is built with Jekyll and presents data engineering project experience in portfolio form.

## Live Site

- https://zerokhha.github.io

## Tech Stack

- Jekyll
- Liquid Template
- Sass
- Bootstrap
- Ruby (Bundler)

## Project Structure

- `_config.yml`: 사이트 기본 설정(제목, 설명, 메뉴, 페이지네이션)
- `_layouts/`: 공통 레이아웃 템플릿
- `_includes/`: 재사용 UI 조각(헤더, 푸터, 히어로 영역 등)
- `_posts/`: 블로그 포스트
- `_work_list/`: 프로젝트 목록 카드 데이터
- `_work_content/`: 프로젝트 상세 페이지
- `css/`, `_sass/`, `js/`: 스타일 및 스크립트
- `_site/`: Jekyll 빌드 결과물(자동 생성)

## Run Locally

1. 의존성 설치 / Install dependencies

```bash
bundle install
```

2. 로컬 서버 실행 / Start local server

```bash
bundle exec jekyll serve
```

3. 브라우저 확인 / Open in browser

```text
http://127.0.0.1:4000
```

## Content Editing Guide

- KR: 히어로 소개 문구는 `_config.yml`의 `description`에서 변경할 수 있습니다.
- EN: Update the hero subtitle from `description` in `_config.yml`.

- KR: 프로젝트 목록은 `_work_list/`, 상세 내용은 `_work_content/`에서 관리합니다.
- EN: Manage project cards in `_work_list/` and detailed project pages in `_work_content/`.

- KR: About 페이지 내용은 `about.md`에서 수정합니다.
- EN: Update the About page in `about.md`.

## Deployment

- KR: GitHub Pages 기반으로 배포됩니다.
- EN: The site is deployed via GitHub Pages.

## Credits

- Phantom theme base by jami-gibbs/phantom
- Bootstrap
- Animate.css
- WOW.js
