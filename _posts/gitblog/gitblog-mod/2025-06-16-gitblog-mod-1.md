---
title: "[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 수정 - _config.yml 수정"
categories:
- Gitblog
- Jekyll기반 Chirpy 테마 Gitblog 수정
img_path: "/assets/img/posts/gitblog/gitblog-mod/2025-06-16-gitblog-mod-1"
---

## **_config.yml 수정**

> **_config.yml에서는 개인 홈페이지의 환경 설정값을 수정할 수 있습니다.**

[앞선 포스트](../gitblog-gen-2/)에서는 `_config.yml`에서 `url`만 수정하여 배포가 가능하도록 했습니다. `_config.yml`에서는 `url`뿐만 아니라 블로그의 다양한 환경 설정값을 수정할 수 있습니다. 설정값을 순서대로 하나씩 살펴보도록 하겠습니다.

> 버전마다 `_config.yml`의 구성이 다를 수 있습니다. 저는 `7.3.0` 버전에서 수정했습니다.
{: .prompt-warning}

> 아래의 내용들은 [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md), [jekyll-theme-chirpy _config.yml 주석](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/_config.yml)과 [CDN 개요](https://aws.amazon.com/ko/what-is/cdn/)의 내용을 기반으로 작성했습니다.
{: .prompt-info}

- theme
: 지금 적용하고 있는 테마 이름입니다. 우리는 `jekyll-theme-chirpy`을 사용하고 있기 때문에 수정할 필요가 없습니다.
- lang
: 어떤 언어를 타겟으로 작성할지 정할 수 있습니다. 저는 `ko`로 설정하겠습니다.
- timezone
: 시간설정입니다. `Asia/Seoul`로 해둘게요.
- Jekyll SEO tag 설정
: `SEO tag`는 검색 엔진 최적화(SEO, Search Engine Optimization)를 위해 웹페이지에서 사용되는 `HTML` 태그입니다. 말 그대로 검색 엔진에 페이지의 내용을 알려주고 다른 사용자가 웹 페이지를 쉽게 발견할 수 있도록 합니다.
	- title
	: 블로그에서 가장 중요한 제목입니다. `Sidebar`에 블로그의 제목을 확인할 수 있고 블로그 `Home`에 접속했을 때,  `Tap`에서도 블로그 제목을 확인할 수 있습니다.
	- tagline
	: 부제목입니다. `Sidebar`에 블로그의 제목 바로 밑에서 확인할 수 있습니다.
	![1]({{ page.img_path }}/1.png){: .shadow .rounded-10}
	- description
	: `meta` 태그용 긴 설명입니다. 이 설명은 블로그를 검색했을때, 아래의 그림처럼 블로그에 대한 설명으로 나타나게 됩니다.
	![2]({{ page.img_path }}/2.png){: .shadow .rounded-10}
	- url
	: 배포하는 블로그의 전체 url입니다. 일반적으로 `https://<username>.github.io`로 설정을 합니다.
	- github, twitter
	: `GitHub`, `Twitter` 유저명을 입력하면 됩니다. 꼭 입력할 필요는 없습니다.
	- social
	: `Footer`에 저작권 소유자의 이름, 연락 방법 등을 입력할 수 있습니다.
	![3]({{ page.img_path }}/3.png){: .shadow .rounded-10}
		- name
		: 저작권 소유자의 이름입니다.
		- email, links
		: 저작권 소유자에게 연락하는 방법입니다.
	- webmaster_verifications
	: 사이트 소유권 확인용 메타태그입니다. 관련 내용은 [[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 생성 - ③ Google 검색 엔진에 노출시키기](../gitblog-gen-3/)에서 다뤘습니다.
- analytics
: `Web Analytics`설정을 위한 것으로 방문자 활동을 추적할 수 있도록 외부 웹 분석 도구를 연동할 수 있습니다. `Google`, `Goatcounter` 등 다양한 `Web Analytics`를 사용할 수 있습니다.
- pageviews
: 페이지 조회수입니다. 지금은 오로지 `Goatcounter` 에서만 제공한다고 합니다.
- theme_mode
: 비워두면 `Sidebar`의 하단에 있는 `Toggle` 버튼으로 `Light` 버전과 `Dark` 버전을 블로그 이용자가 선택할 수 있습니다. 하지만 비워두지 않고 `light` 또는 `dark`를 입력하면 해당 버전으로만 블로그를 유지할 수 있습니다.
- cdn
: `CDN`은 콘텐츠 전송 네트워크(Content Delivery Network)로 데이터 사용량이 많은 애플리케이션의 웹 페이지 로드 속도를 높이는 상호 연결된 서버 네트워크입니다. 클라이언트와 웹 사이트 서버 간에 중간 서버를 두어 효율성을 높이는 방식이라고 합니다. 이 부분은 개인적으로 유지하고 있는 `Cloud`가 있다면 설정해주세요.
- avatar
: `Sidebar`에 어떤 이미지를 띄울지 정할 수 있습니다.
![4]({{ page.img_path }}/4.png){: .shadow .rounded-10}
- social_preview_image
: 소셜 미리보기 이미지를 지정할 수 있습니다. 이미지 경로를 설정해두면 블로그 사용자가 블로그 링크를 `Facebook`, `Twitter` 등에 공유할 때 미리보기 썸네일로 표시됩니다.
- toc
: 게시글에 대해 전역적으로 목차(Table of Contents)를 표시할지 말지 설정하는 값입니다.
- comments
: 댓글 기능을 활성화하기 위한 항목입니다. 
	- provider
	: 어떤 방식으로 댓글을 받을지 정할 수 있습니다. 아래와 같이 총 3가지 방식이 있습니다.
		- disqus
		: [`DISQUS`](https://help.disqus.com/en/) 계정으로 댓글을 등록할 수 있습니다. 단순하게 [`shortname`](https://help.disqus.com/en/articles/1717111-what-s-a-shortname)을 등록하면 사용할 수 있습니다.
		- utterances
		: `GitHub Issue`  기반의 경량 댓글 시스템입니다. `GitHub` 유저라면 쉽게 댓글을 달 수 있지만 답글을 달 수는 없습니다.
		- giscus
		: `GitHub Discussions` 기반의 댓글 시스템입니다. `GitHub` 유저라면 쉽게 댓글을 달 수 있고 `utterances`과 달리 답글도 바로 달 수 있어서 저는 이 방식을 채택했습니다. [`giscus`를 설정하는 방법](#optional-giscus-설정-방법)은 아래에서 확인해주세요.	
- assets
: 정적 `asset`(CSS, JS, 이미지 등)을 외부 `CDN`이 아니라 직접 호스팅하는 기능을 제어하는 설정입니다. 해당 방식에 대해서 자세히 알기 위해서는 [chirpy-static-assets의 README](https://github.com/cotes2020/chirpy-static-assets)를 확인해주시기 바랍니다.
- pwa
: `PWA`는 `Progressive Web App`로 웹사이트를 앱처럼 설치하고, 오프라인에서도 사용할 수 있게 해주는 기능입니다.
- pageinate
: 블로그의 `Home`에서 게시할 페이지 개수를 지정하는 설정입니다.
- baseurl
: 사이트의 기본 하위 경로를 지정하는 설정값입니다. 우리는 사용자`Repository`에 `Gitblog`를 배포했기 때문에 `baseurl`을 비워둬도 문제가 없었습니다. 하지만, 프로젝트를 개설하고 다양한 프로젝트를 관리하게 되면 이야기가 달라집니다. 예를 들어, 프로젝트에서 문서를 `myblog`라는 이름으로 운영할 경우 사이트 주소는 `https://<username>.github.io/myblog/`가 됩니다. 이때는 `baseurl = "myblog"`라고 설정해야 경로가 올바르게 작동합니다.
- 나머지 설정
: 수정하는 것을 추천하지 않습니다.

## **(Optional) giscus 설정 방법**
`giscus`를 사용하기 위해서는 아래의 방법을 순서대로 진행하시면 됩니다. 
> 아래의 방법은 `Chirpy` 테마에서의 설정 방법입니다. 그리고 앞선 포스트들과 일관된 내용으로 진행하기 때문에 `Repository` 이름을 `<username>.github.io`로 가정하며 작성했습니다.
{: .prompt-info}
1. [`giscus`](https://giscus.app/ko)에 먼저 접근한 후, **`giscus` 설정 > 저장소**의 절차를 따릅니다. 그리고 아래와 같이 `<username>/<username>.github.io`를 빈칸에 입력했을 때, `통과했습니다! 이 저장소는 모든 조건을 만족합니다.`가 출력되어야 합니다.<br>
![5]({{ page.img_path }}/5.png){: .shadow .rounded-10}

2. **`giscus` 설정 > 페이지 ↔️ Discussions 연결**에서는 아래와 같이 설정해도 좋지만 기호에 맞게 설정해주세요.
![6]({{ page.img_path }}/6.png){: .shadow .rounded-10}
3. **`giscus` 설정 > Discussion 카테고리, 기능, 테마**는 아래와 같이 설정합니다. 당연하게도 기호에 따라 다른 설정을 하셔도 됩니다.
![7]({{ page.img_path }}/7.png){: .shadow .rounded-10}
4. **`giscus` 설정 > giscus 사용**에서 `_config.yml`에 입력이 필요한 `repo_id`, `category_id`가 생성되어 있을 것입니다. `mapping`, `strict` 등등 여러 설정 값도 생성 되었으니 확인하시고 입력해주세요.
![8]({{ page.img_path }}/8.png){: .shadow .rounded-10}
5. 이제 아래와 같이 `comments`가 생겼는지 확인하시면 됩니다.
![9]({{ page.img_path }}/9.png){: .shadow .rounded-10}


## **마치며**
자유도가 높은 `Jekyll` 기반 `Chirpy` 테마인 만큼 설정할 것도 많이 있습니다. 하지만 제가 설명한 내용을 살펴보면 그렇게까지 어려운 내용은 없습니다. 이 포스트도 여러분의 블로그 생성에 도움되기를 기대하며 마치도록 하겠습니다.
