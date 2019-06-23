# Hexo

- 쉽고 빠른, 강력한 블로그 프레임워크
- Markdown 지원
- 작업 환경 CLI(command line interface)방식
- github의 pages 서비스를 활용하여 정적 사이트 생성 가능
- 웹 프로그래밍에 대한 기본적인 지식 필요



### Install node.js

> 다운로드 공식사이트
>
> <https://nodejs.org/en/>



### Setting Hexo

> ```bash
> npm install -g hexo-cil
> mkdir 폴더명
> 
> cd 폴더명
> ```
>
> 
>
> ```bash
> hexo init #웹사이트 초기화
> npm install #필요한 패키지 다운로드
> #node_modules 폴더가 없을 시 생성
> #현재 프로젝트의 package.json에 있는 패키지를 node_modules폴더에 설치
> ```
>
> 
>
> ```yml
> # Site
> title: Jun's Blog
> subtitle: Jun's의 개발일지
> description:
> keywords:
> author: TaeJune Joung
> language: en
> timezone: Asia/Seoul
> 
> # URL
> ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
> url: http://TaeJuneJoung.github.io/
> root: /
> permalink: :year/:month/:day/:title/
> permalink_defaults:
> ```



### Git repositories

> `계정명.github.io` 공개용 저장소 생성



### Change Theme

> ```bash
> cd themes
> #어둡고 깔끔한게 마음에 들어서 해당 theme를 사용함
> #theme마다 기본적으로 제공되는 부분들이 있으니 주의 선택
> git clone https://github.com/probberechts/hexo-theme-cactus.git
> ```
>
> 
>
> ```bash
> #_config_yml line73
> 
> # Extensions
> ## Plugins: https://hexo.io/plugins/
> ## Themes: https://hexo.io/themes/
> theme: hexo-theme-cactus
> ```
>
> 
>
> **필요 패키지 다운로드 및 서버실행**
>
> ```bash
> cd hexo-theme-cactus
> npm install
> 
> cd ../..
> #서버실행
> hexo server
> ```



### github에 배포하기

> **git 배포를 위한 패키지 설치**
>
> ```bash
> npm install hexo-deployer-git --save
> ```
>
> 
>
> **배포(deployment)주소 수정**
>
> ```yml
> # Deployment
> ## Docs: https://hexo.io/docs/deployment.html
> deploy:
>   type: git
>   repo: https://github.com/TaeJuneJoung/TaeJuneJoung.github.io.git
>   branch: master
> ```
>
> 
>
> **deployment**
>
> ```bash
> hexo deploy
> ```



### 이미지 복사

```
source/
	images/
```

`images`폴더에 넣어서 경로 설정으로 잡아주면 됨.

> **example)**
>
> ​	url: /images/파일명



### 페이지 및 파일 생성

> **페이지 생성**
>
> ```bash
> hexo new page 페이지명
> ```
>
> **파일 생성**
>
> ```bash
> hexo new 파일명
> ```