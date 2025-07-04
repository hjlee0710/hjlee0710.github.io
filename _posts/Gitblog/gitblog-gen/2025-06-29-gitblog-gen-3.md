---
title: "[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 생성 - ③ Google 검색 엔진에 노출시키기"
categories:
- Gitblog
- Jekyll기반  Chirpy 테마 Gitblog 생성
---

## **검색 엔진 노출?**
> **검색 엔진에 내 블로그를 노출시켜, 검색 엔진을 사용하는 사람들과 블로그 내용을 공유해보자.**

앞선 작업들을 통해서 우리는 `Gitblog`를 생성하고 배포했습니다. 하지만 이것만으로는 검색 엔진에서 내 블로그가 쉽게 발견되지 않아, 오랜 시간 동안 나만 볼 수 있는 블로그로 남을 가능성이 높습니다. 검색 엔진이 크롤링을 하며 언젠가는 내 블로그를 찾아낼 수 있겠지만 꽤 오랜 시간을 기다려야 합니다. 내 블로그가 남들에게 노출될 때까지 마냥 기다리는 방법도 있긴 하지만, 빠른 기간 내에 다른 사람들에게 노출시키기 위해서는 `Google`과 같은 검색 엔진이 크롤링을 잘 할 수 있도록 직접 노출시켜야 합니다. 저는 `Google` 검색 엔진에 제 블로그를 노출시키고 싶기 때문에 이와 관련된 `Google Search Console`, `sitemap.xml`, `robots.txt`에 대한 설명을 해보도록 하겠습니다.

#### **Google Search Console란?**

> [Google Search Console 도움말](https://support.google.com/webmasters/answer/9128668?hl=ko&ref_topic=9128571&sjid=4295738945476696937-NC)을 참고하여 작성했습니다.
{: .prompt-info }

> **`Google Search Console`은 `Google`에서 무료로 제공하는 서비스로, 사용자가 사이트의 `Google` 검색결과 인지도를 모니터링하고 관리하며 문제를 해결하도록 도와줍니다.**<br>
> _(Google Search Console 도움말)_

`Google Search Console`의 주요한 기능은 아래와 같습니다.
- `Google`에서 사이트를 찾아 크롤링하고 색인할 수 있는지 확인
- 사이트의 `Google` 검색 노출 및 클릭 트래픽 데이터 확인
- 색인 생성 문제 해결 등등

위의 기능들로 `Google`이 내 사이트를 얼마나 잘 이해하고 있는지를 확인할 수 있습니다. `Google`은 내 사이트를 이해하기 위해서 주기적으로 웹을 크롤링하며 새로운 페이지나 수정된 페이지를 탐색합니다. 하지만, 그 주기는 며칠에서 몇 주, 심지어 몇 개월까지도 걸릴 수 있습니다.

> **따라서, `Google`이 내 블로그를 더 빠르고 정확하게 크롤링할 수 있도록 하기 위해서는 `Google Search Console`에 도메인을 등록하는 것 뿐만 아니라 `sitemap.xml`을 함께 등록하는 것이 효과적입니다.**

#### **sitemap.xml란?**
> [sitemaps.org](https://www.sitemaps.org/ko/index.html)를 참고하여 작성했습니다.
{: .prompt-info }

> **Sitemap은 웹마스터가 크롤링에 사용할 수 있는 사이트의 페이지에 대한 정보를 검색 엔진에 알리는 손쉬운 방법입니다.**
> _( sitemaps.org 설명글)_

`sitemap.xml`은 검색 엔진에서 사이트를 보다 지능적으로 크롤링할 수 있도록 각 URL에 대한 추가 메타데이터(마지막 업데이트된 날짜, 변경 빈도, 사이트의 다른 URL에 상대적인 중요도)와 함께 사이트에 대한 `URL`을 나열하는 `XML` 파일입니다. 

>**즉, 추가 메타데이터를 통해 Google이 내 사이트의 구조를 더 잘 이해하고, 새 콘텐츠를 더 빠르게 발견하도록 도와주는 것입니다.**

`Google Search Console` 등록만으로 크롤링 주기가 자동으로 줄지는 않지만, `sitemap.xml`을 제출하면 Google이 새 글이나 변경사항을 빠르게 감지할 수 있기 때문에 크롤링과 색인 속도는 간접적으로 향상될 수 있습니다.

#### **robots.txt란?**
> [robots.txt](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=ko)를 참고하여 작성했습니다.
{: .prompt-info }

> **robots.txt 파일은 크롤러가 사이트에서 액세스할 수 있는 URL을 검색엔진 크롤러에 알려 줍니다.**
> _(Google 검색 센터 Robots.txt 소개)_

`robots.txt` 파일은 크롤러에게 사이트에 접근할 수 있는 범위를 정해줌으로서 사이트의 크롤러 트래픽을 관리할 수 있습니다. 또한, 크롤러가 몇몇 파일에 접근할 수 없도록 지정할 수 있습니다.  우리는 이 robots.txt 파일에 sitemap.xml의 위치를 명시하여, 크롤러가 사이트 구조를 보다 쉽게 파악하고 URL 리스트에 접근할 수 있도록 도와줄 것이고 접근을 제한하고 싶은 경로들을 지정할 것입니다.

## **Google 검색 엔진에 노출시키기**
#### **sitemap.xml 생성**
> **`Local`의 `Root`에 아래의 코드를 파일명 `sitemap.xml`로 생성하시면 됩니다.**

> `sitemap.xml`의 위치는 꼭 `Root`일 필요는 없습니다.
{: .prompt-info }

```xml
{% raw %}---
layout: null
---

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd"
				xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
		{% for post in site.posts %}
		<url>
				<loc>{{ site.url }}{{ post.url }}</loc>
				{% if post.lastmod == null %}
				<lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
				{% else %}
				<lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
				{% endif %}

				{% if post.sitemap.changefreq == null %}
				<changefreq>weekly</changefreq>
				{% else %}
				<changefreq>{{ post.sitemap.changefreq }}</changefreq>
				{% endif %}

				{% if post.sitemap.priority == null %}
				<priority>0.5</priority>
				{% else %}
				<priority>{{ post.sitemap.priority }}</priority>
				{% endif %}

		</url>
		{% endfor %}
</urlset>{% endraw %}
```

위의 코드는 `Gitblog`를 사용하시는 분들이 많이 사용하는 `sitemap.xml`입니다. [`sitemap.xml` 샘플 양식](https://www.sitemaps.org/ko/protocol.html)을 크게 벗어나지 않아서 이해하기 쉽습니다. 

> 커스텀을 위해서 `<urlset>`, `<url>` 등등 각 태그의 내용을 이해하고 싶으시다면, [XML 태그 정의](https://www.sitemaps.org/ko/protocol.html#xmlTagDefinitions)를 참고해 주세요.
{: .prompt-info }

> 위의 코드에서 for문을 통해 `<loc>`에 어떤 페이지의 `URL`을 불러오는지 궁금하시다면  `Local`에서는 `http://localhost:4000/sitemap.xml`를 주소창에 입력하시거나 업로드한 블로그에서는 `https://<username>.github.io/sitemap.xml`을 입력하시면 됩니다.
> ![4](/assets/img/2025-06-29-gitblog-gen-3/4.png){: .shadow .rounded-10}
{: .prompt-info }

#### **robots.txt 생성**
> **`Local`의 `Root`에 아래의 코드를 파일명 `robots.txt`로 생성하시면 됩니다.**

> `<username>`은 알맞게 수정해야합니다.
{: .prompt-warning }

```
User-agent: *
Allow: /

Sitemap: https://<username>.github.io/sitemap.xml
```

> 커스텀을 위해서 `User-agent`, `Allow` 등등 각 항목의 내용을 이해하고 싶으시다면, [Google 검색 센터 robots.txt 파일 작성 및 제출 방법](https://developers.google.com/search/docs/crawling-indexing/robots/create-robots-txt?hl=ko)을 참고해 주세요.
{: .prompt-info }

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
5. 이제 원격 `GitHub`의 `Repository`에 `add`, `commit`, `push`하고 3번 과정에서 누르지 않았던 확인 버튼을 눌러서 등록 완료합니다.

#### **sitemap.xml를 Google Search Console에 제출**
앞서 말씀드린대로 `Google`이 내 블로그를 더 빠르고 정확하게 크롤링할 수 있도록 하기 위해서 `sitemap.xml`을 생성했었는데요. 이를 `Google Search Console`에 제출하지 않더라도 제대로 작동하지만, 검색 엔진이 이 파일의 존재를 반드시 인식한다고 보장할 수는 없습니다. 그래서 보다 확실하게 하기 위해서 이와 같은 작업을 한다고 생각하시면 되겠습니다. <br><br>
아래의 그림과 같이 `Google Search Console`의 `Sitemaps` 항목에 들어가서 `새 사이트맵 추가`에서 `sitemap.xml`을 입력하고 제출합니다.
![5](/assets/img/2025-06-29-gitblog-gen-3/5.png){: .shadow .rounded-10}

> 하지만 아래의 화면처럼 `상태`가 `가져올 수 없음`이 되어 있습니다. 기다리면 해결되니 걱정말고 기다리시면 됩니다.
![6](/assets/img/2025-06-29-gitblog-gen-3/6.png){: .shadow .rounded-10}
{: .prompt-info }

#### **(Optional) 메타태그 노출 방지**
위의 방법대로만 하면 문제없이 블로그가 


## 마치며
이번 포스트에는 `Google` 검색 엔진에 노출에 내 블로그를 노출시켰습니다. 하지만 이 방법으로는
