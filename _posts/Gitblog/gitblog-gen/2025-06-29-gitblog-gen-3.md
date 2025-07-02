---
title: "[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 생성 - ③ Google 검색 엔진에 노출시키기"
categories:
- Gitblog
- Jekyll기반  Chirpy 테마 Gitblog 생성
---

## **검색 엔진 노출?**
> **검색 엔진에 내 블로그를 노출시켜, 검색 엔진을 사용하는 사람들과 블로그 내용을 공유해보자.**

앞선 작업들을 통해서 우리는 `Gitblog`를 생성하고 배포했습니다. 하지만 이것만으로는 오랜 기간 동안 나만의 블로그가 될 뿐이지 다른 사람들과 블로그 내용을 공유할 수 없을지도 모릅니다. 내 블로그가 남들에게 노출될 때까지 마냥 기다리는 방법도 있긴 하지만, 빠른 기간 내에 다른 사람들에게 노출시키기 위해서는 `Google`과 같은 검색 엔진이 크롤링을 잘 할 수 있도록 직접 노출시켜야 합니다. 저는 `Google` 검색 엔진에 제 블로그를 노출시키고 싶기 때문에 `Google Search Console`, `sitemap.xml`, `robots.txt`에 대한 설명을 해보도록 하겠습니다.

## **Google Search Console**

> [Google Search Console 도움말](https://support.google.com/webmasters/answer/9128668?hl=ko&ref_topic=9128571&sjid=4295738945476696937-NC)을 참고하여 작성했습니다.
{: .prompt-info }

> **`Google Search Console`은 `Google`에서 무료로 제공하는 서비스로, 사용자가 사이트의 `Google` 검색결과 인지도를 모니터링하고 관리하며 문제를 해결하도록 도와줍니다.**<br>
> _(Google Search Console 도움말)_

`Google Search Console`의 주요한 기능은 아래와 같습니다.
- `Google`에서 사이트를 찾아 크롤링할 수 있는지 확인
- 사이트의 `Google` 검색 트래픽 데이터 확인
- 색인 생성 문제 해결 등등

여기서 주목해야할 점은 `Google`에서 사이트를 찾아 크롤링할 수  있는지 확인한다는 점입니다. `Google`은 지속적으로 크롤링을 하면서 웹 페이지를 탐색하며 정보를 수집하는데요. `Google`이 제 블로그를 크롤링하는 주기가 며칠에서 몇 주, 몇 개월까지 될 수 있습니다. 몇 개월이 되면 노출이 너무 늦기 때문에 `Google`이 크롤링을 쉽게 할 수 있도록 `Google Search Console`에 제 블로그 도메인을 등록한다고 생각하시면 좋을 것 같습니다. 그럼 이제부터 `Google Search Console`에 제 블로그를 등록해 보겠습니다.

#### **Google Search Console에 블로그 도메인 등록하기**

> 이 포스트는 Chirpy 테마(ver. 7.3.0) 기준으로 작성했습니다. 만약에 다른 테마를 사용하신다면, 3번에서 `권장 확인 방법`을 따라하시길 바랍니다.
{: .prompt-warning}


1. 먼저, [`Google Search Console`](https://search.google.com/search-console/welcome?utm_source=about-page)에 들어가서 아래의 사진처럼 `URL 접두어`에 블로그 도메인을 입력해줍니다.
![1](/assets/img/2025-06-29-gitblog-gen-3/1.png){: .shadow .rounded-10}

2. 이후 아래의 창이 뜨면, `속성으로 이동`을 클릭합니다.
![2](/assets/img/2025-06-29-gitblog-gen-3/2.png){: width="400"   .shadow .rounded-10}

3. `권장 확인 방법`에 표시한대로 `HTML` 파일을 `root`에 다운로드 시켜도 되지만, 저는 `HTML 태그`를 사용해서 소유권 확인을 해보도록 하겠습니다. 아래의 그림대로 `메타태그`를 복사합니다.
![3](/assets/img/2025-06-29-gitblog-gen-3/3.png){: width="400" .shadow .rounded-10}

4. 복사한 `메타태그`를 `Local`의 `root`에 위치한 `_config.yml`에 입력해야 합니다. `_config.yml`의 `webmaster_verifications` 항목에서 `google:`에 태그 내용을 입력합니다. 이때, 입력하는 내용은 복사한 `메타태그`의 `content`입니다.
	```yaml
	# Site Verification Settings
	webmaster_verifications:
		# 복사한 메타태그가 <meta name="google-site-verification" content="~~~~~~" /> 라면,
		# ~~~~~~를 (여기에 입력)으로 표시한 곳에 넣으시면 됩니다.
		google: (여기에 입력) 
		bing: # fill in your Bing verification code
		alexa: # fill in your Alexa verification code
		yandex: # fill in your Yandex verification code
		baidu: # fill in your Baidu verification code
		facebook: # fill in your Facebook verification code
	```
5. das
