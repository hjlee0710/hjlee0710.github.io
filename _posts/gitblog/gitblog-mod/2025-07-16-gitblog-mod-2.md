---
title: "[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 수정 - Title과 Tagline 글자 및 레이아웃 속성 변경"
categories:
- Gitblog
- Jekyll기반 Chirpy 테마 Gitblog 수정
img_path: "/assets/img/posts/gitblog/gitblog-mod/2025-07-16-gitblog-mod-2"
---

## **Title과 Tagline 수정하기**
> **글자 속성과 레이아웃 속성을 수정해보도록 하겠습니다.**

우리는 [`[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 수정 - _config.yml 수정`](../gitblog-mod-1)에서 `Title`과 `Tagline`을 입력하여 `Sidebar`에 표시되는 내용을 수정했습니다. 하지만 `_config.yml`에서는 글자 및 레이아웃 속성을 수정할 수 없습니다. `/_sass/layout/_sidebar.scss`에 접근하여 글자 및 레이아웃 속성을 수정해보겠습니다.

> **`SASS`는 `Syntactically Awesome StyleSheet`로 CSS의 확장 언어입니다. 중첩 규칙, 변수, 믹스인, 선택자 상속 등 다양한 기능을 추가합니다. `SASS`는 커맨드라인 도구나 빌드 시스템용 플러그인을 통해 잘 정리된 표준 CSS로 변환됩니다.**<br>
> _([SASS GitHub README](https://github.com/sass/sass))_
{: .prompt-info}
#### **/_sass/layout/_sidebar.scss 수정**
> 이 포스트는 Chirpy 테마(ver. 7.3.0) 기준으로 작성했습니다.
{: .prompt-warning}

> **디렉토리 `/_sass/layout/`에서는 `Sidebar`, `Footer`, `Panel` 등의 속성을 바꿀 수 있습니다.**

우리는 `Title`과 `Tagline`을 수정할 수 있는 `_sidebar.scss`만 살펴보도록 하겠습니다.
``` scss
#sidebar {
  @include mx.pl-pr(0);

  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  overflow-y: auto;
  width: v.$sidebar-width;
  background: var(--sidebar-bg);
  border-right: 1px solid var(--sidebar-border-color);

  /* Hide scrollbar for IE, Edge and Firefox */
  -ms-overflow-style: none;
  /* IE and Edge */
  scrollbar-width: none;
  /* Firefox */
	
	/* Hide scrollbar for Chrome, Safari and Opera */
	&::-webkit-scrollbar {...}
	@include bp.lt(bp.get(lg)) {...}
	@include bp.xxxl {...}
	 %sidebar-link-hover {...}
	a {...}
	#avatar {...}
	.profile-wrapper {...}
	.site-title {...}
	.site-subtitle {...}
	ul {...}
	.sidebar-bottom {...}
  /* .sidebar-bottom */
}
```
위의 코드는  `_sidebar.scss` 소스 코드에서 주요 요소의 내용을 `{...}`으로 생략했습니다. 여러 요소들을 확인할 수 있지만 우리가 눈여겨 봐야하는 것은 `.site-title`, `.site-subtitle`입니다. 그럼 `.site-title`, `.site-subtitle` 코드를 확인해보겠습니다.
``` scss
  .site-title {
    @extend %clickable-transition;
    @extend %sidebar-link-hover;

    font-family: inherit;
    font-weight: 900;
    font-size: 1.5rem;//1.75rem;
    line-height: 1.2;
    letter-spacing: 0.25px;
    margin-top: 1.25rem;
    margin-bottom: 0.5rem;
    width: fit-content;
    color: var(--site-title-color);
  }

  .site-subtitle {
    font-size: 80%;//95%;
    color: var(--site-subtitle-color);
    margin-top: 0.25rem;
    word-spacing: 1px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
```
`.site-title`, `.site-subtitle` 코드에서 어떤 글자 및 레이아웃 속성들을 바꿀 수 있는 지 보겠습니다. 먼저, 글자 속성들을 살펴보겠습니다.
- font-family
: 글꼴. 기본 값은 `inherit`, 즉 상속 받은 값입니다.
- font-weight
: 글자의 굵기. 기본 값은 `900`로 매우 굵은 글씨입니다.
- font-size
: 글자의 크기. 기본 값은 `1.75rem`이지만 저는 `Title`이 길어서 `1.5rem`으로 조정했습니다. 또한, `rem`이 아니라 `%`으로도 조정이 가능합니다.
- line-height
: 줄 간격. 기본 값은 `1.2`입니다.
- letter-spacing
: 글자 사이의 간격. 기본 값은 `0.25px`입니다.
- color
: 텍스트의 색상. 기본 값은 `var(--site-title-color)`으로 지정되어 있는데, 이는 `/_sass/themes/_dark.scss`와 `/_sass/themes/_light.scss`에서 선언 및 정의한 값입니다.
- word-spacing
: 단어 사이의 간격. 기본 값은 `1px`입니다.
	- user-select
	: 블로그 이용자가 텍스트를 선택 가능하도록 할 것인지 설정하는 값입니다. 기본 값은 `none`으로 지정되어 있는데, 이는 블로그 이용자가 드래그를 하지 못하도록 합니다.
	- webkit-user-select
	: `Safari`, `Chrome`와 같은 `Webkit` 기반 브라우저에서 `user-select`를 지정할 때 사용.
	- moz-user-select
	: `Mozilla Firefox` 브라우저에서 `user-select`를 지정할 때 사용.
	- ms-user-select
	: `Microsoft` 브라우저에서 `user-select`를 지정할 때 사용.

그 다음 레이아웃 설정을 살펴보도록 하겠습니다.
- margin-top
: `Title`이나 `Tagline`의 위쪽 바깥 여백을 설정.
- margin-bottom
: `Title`이나 `Tagline`의 아래쪽 바깥 여백을 설정.
- width
: `Title`이나 `Tagline`의 너비를 설정.

## **마치며**
이번 포스트에서는 `Title`과 `Tagline`의 글자 및 레이아웃 설정을 수정했습니다. 저는 프론트엔드 전문가가 아님에도 `SASS`가 그렇게 어렵지 않아서 꽤 수정하기 편했습니다. 여러분들도 이 포스트를 통해서 쉽게 자신만의 블로그를 구축했으면 좋겠습니다.
