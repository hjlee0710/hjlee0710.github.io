---
title: "[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 수정 -  ① _config.yml 수정"
categories:
- Gitblog
- Jekyll기반 Chirpy 테마 Gitblog 수정
---

## **_config.yml 수정**
---

> **_config.yml에서는 개인 홈페이지의 환경 설정값을 수정할 수 있습니다.**

[앞선 포스트](../gitblog-gen-2/)에서는 `_config.yml`에서 `url`만 수정하여 배포가 가능하도록 했습니다. `_config.yml`에서는 `url`뿐만 아니라 블로그의 다양한 환경 설정값을 수정할 수 있습니다. 설정값을 순서대로 하나씩 살펴보도록 하겠습니다.

> 버전마다 `_config.yml`의 구성이 다를 수 있습니다. 저는 `7.3.0` 버전에서 수정했습니다.
{: .prompt-warning}

> 아래의 내용들은 [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md)의 내용을 기반으로 작성했습니다.
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
	: 부제목입니다. `Sidebar`에 블로그의 제목 바로 밑에 확인할 수 있습니다.
	![1](/assets/img/2025-06-16-gitblog-mod-1/1.png){: .shadow .rounded-10}
	- description
	: `meta` 태그용 긴 설명입니다. 이 설명은 블로그를 검색했을때, 아래의 그림처럼 블로그에 대한 설명으로 나타나게 됩니다.
	- url
	: 배포하는 블로그의 전체 url입니다. 일반적으로 `https://<username>.github.io`로 설정을 합니다.
	- git
