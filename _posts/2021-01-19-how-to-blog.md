---
title: "Jekyll 블로그 만드는 간단한 방법"
date: 2021-01-19T15:27:00+09:00
categories:
  - 블로그
tags:
  - Jekyll
  - tutorial
paginate_path: '/index/:num/pagination-plugin/'
---

이 포스트에서는 github와 Jekyll을 이용하여 블로그를 만들고 커스터마이징하는 법을 다룹니다.

github와 Jekyll의 조합은 웹이나 프론트/백 엔드에 대한 지식 없이도 직관적으로 블로그를 만들 수 있다는 점에서 참 편리합니다. 프로젝트 원격 저장 및 협업 도구로 유명한 github에서는 서버 및 도메인을, Jekyll은  HTML, CSS, Markdown을 기반으로 정적인 웹사이트를 만들어줍니다. 거두절미하고 다음과 같은 과정으로 Minimal Mistake 테마의 블로그를 만들 수 있습니다.

1. Github 아이디를 만들고 접속한다.
2. Minimal Mistake 테마의 저장소를 fork하고 설정의 웹 옵션을 활성화시킨다: [이곳][jekyll-start]에서 "Use this template"을 클릭하면 한번에 해결됩니다.
3. README.md나 [Jekyll Minimal Mistake Guide][jekyll-guide]를 참고하며 내 저장소의 파일을 수정한다. 

참 쉽죠? 참고로 저는 3의 단계에서 블로그의 skin을 air로 커스터마이징하였습니다. 너무 하얀 배경은 좋아하지 않아서요. 또 이전 포스팅에서 언급하였듯이 저는 한국어-수식-코드 포스팅에 맞게 폰트 옵션을 수정하였습니다. 이 부분이 좀 까다로울 수 있는데, [Lee Sangheon][jekyll-font]님의 포스팅에 힘입어 시행착오를 정말 많이 줄일 수 있었습니다. 이 포스팅을 빌려 다시 한번 감사드리고 싶네요. 참고로 저는 "Noto Sans KR"과 "Source Code Pro" 폰트를 사용하였습니다. 

여담이지만, 각 포스팅 파일 상단의 date 항목에도 관심이 생겨 조금 알아봤습니다. "2019-04-18T15:34:30-04:00"와 같은 형식으로 지정이 되어있던데, [이곳][jekyll-date]을 살펴보니 "-" 이후 부분은 UTC 시간대를 나타냅니다. 이 또한 한국에 맞는 "+09:00"시간대와 제 포스팅 시간에 맞추어 수정하였습니다.

[jekyll-start]: https://github.com/mmistakes/mm-github-pages-starter
[jekyll-guide]: https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
[jekyll-font]: https://evenharder.github.io/blog/jekyll-change-fonts/
[jekyll-date]: https://anupumpant.github.io/blog/ISO8601/
