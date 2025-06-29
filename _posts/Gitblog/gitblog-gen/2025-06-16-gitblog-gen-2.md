---
title: "[Gitblog] Jekyll 기반 Chirpy 테마 Gitblog 생성 -  ② Chirpy 테마 적용 및 배포"
categories:
- Gitblog
- Jekyll기반  Chirpy 테마 Gitblog 생성
---

## **Chirpy 테마란?**
---
> **A minimal, responsive, and feature-rich Jekyll theme for technical writing.**<br>
>_( jekyll-theme-chirpy 설명글)_

[`Chripy 테마`](https://github.com/cotes2020/jekyll-theme-chirpy)는 위의 인용글 그대로 `기술 문서를 위한 최소한의 구조이면서도, 반응형이며, 다양한 기능을 갖춘 Jekyll 테마`입니다. `Chirpy 테마`의 인기는 해당 테마의 `GitHub` 레파지토지 `Fork`, `Star` 수를 보면 실감할 수 있습니다.

> **제작된 테마를 활용한 블로그 관리의 장점은 블로그 관리의 난이도를 크게 낮춰준다는 점입니다.**

앞선 포스트에서 다룬 [`Gitblog`의 단점](../gitblog-gen-1/#gitblog-단점)에서 언급한 대로 `Jekyll`을 활용하여 블로그를 처음부터 끝까지 만들려면 고도의 프론트앤드 개발능력이 필요로 합니다. 하지만 `Chirpy 테마`와 같이 제작된 테마를 쓰면 처음부터 코드를 작성하지 않고, 딱 필요한 부분만 수정하면 되므로 요구되는 프론트앤드 개발능력이 크게 낮아집니다.


## **Chirpy 테마 적용 및 배포**
`Chirpy 테마`를 적용시킨 개인 블로그를 배포하기 위해서는 아래의 작업을 순서대로 진행하면 됩니다.
1. [Chirpy 테마 Fork 하기](#chirpy-테마-fork-하기)
2. [Repository를 Local에서 수정할 수 있도록 설정](#repository를-local에서-수정할-수-있도록-설정)
3. [Repository를 Remote에 배포하기](#repository를-remote에-배포하기)

이제 하나씩 진행해보도록 하겠습니다.

>아래의 내용들은 [jekyll-theme-chirpy (Getting Started)](https://chirpy.cotes.page/posts/getting-started/)를 기반으로 작성했습니다.
{: .prompt-info }
#### **Chirpy 테마 Fork 하기**
---

`Chirpy 테마`를 사용하기 위해서는 자신의 `GitHub`에 `Repository`를 생성해줘야합니다. 생성법은 아래와 같이 2가지가 있습니다.
1. Chirpy Starter를 사용하기
: 이 방법은 업그레이드를 단순화하고 불필요한 파일을 분리하며, 최소한의 설정으로 글쓰기에 집중할 수 있습니다.
2. Repository를 Fork하기
: 이 방법은 기능이나 UI 디자인을 수정하는 데는 편리하지만, 업그레이드 시에는 어려움이 따릅니다. 따라서 Jekyll에 익숙하고 이 테마를 대대적으로 수정할 계획이 있는 경우가 아니라면 사용하지 않는 것이 좋습니다.

저는 차후 UI 디자인을 추가적으로 수정할 의향이 있기 때문에 `Repository를 Fork하기` 방법을 사용하도록 하겠습니다.<br>
이제부터 아래의 순서대로 `Fork`해보겠습니다.

1. `GitHub`에 로그인하기
2. [`Repository`를 `Fork`하고](https://github.com/cotes2020/jekyll-theme-chirpy/fork) 붉은 박스 안의 내용 수정하기
![1](/assets/img/2025-06-16-gitblog-gen-2/1.png){: .shadow .rounded-10}
  - Owner
  : 자신이 생성하고자 하는 `Owner`를 골라주세요.
  - Repository name
  : `jekyll-theme-chirpy`대신에 `<username>.github.io`를 입력해야 합니다. 이때, `<username>`대신에 자신의 `GitHub username`을 소문자로 입력해주셔야 합니다. 왠만하면 `Owner`과 동일합니다.
3. [`GitHub Actions`](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow)를 사용하여 `Repository`를 배포할 수 있도록 설정합니다. 아래의 그림에서처럼 붉은 박스를 따라 **Settings > Pages > Build and deployment > Source > GitHub Actions**와 같이 설정해줍니다.
![2](/assets/img/2025-06-16-gitblog-gen-2/2.png){: .shadow .rounded-10}


>`GitHub` Free 플랜을 사용하신다면 `Repository`는 공개(public) 상태여야 합니다.
{: .prompt-info}


#### **Repository를 Local에서 수정할 수 있도록 설정**
---
자신의 `GitHub`에 `Repository`를 생성했기 때문에, 이제부터는 개인 블로그를 위한 설정을 진행하도록 하겠습니다.

>`<username>` 대신에 자신의 `GitHub username`을 입력해주세요.
{: .prompt-info}

1. `Fork`한 `Repository`를 `clone`으로 `Local` 환경에 저장합니다.
 ```bash
 git clone https://github.com/<username>/<username>.github.io.git
 ```
2. `<username>.github.io.git`에서 `Repository`를 초기화시킵니다.
```bash
cd <username>.github.io.git
bash tools/init.sh
```
3. 의존성 있는 `Ruby` 패키지들을 설치하기 위해서 아래의 명령어를 실행시킵니다.
```bash
bundle
```
4. `Local` 환경에서 제대로 작동하는지 확인하기 위해서 아래의 명령어를 실행시킵니다.
```bash
bundle exec jekyll serve
```
아래 그림의 붉은 박스로 표시한 주소(`http://127.0.0.1:4000/`)를 주소창에 입력하여 `Local` 환경에서 블로그가 제대로 생성되었는지 확인할 수 있습니다.
![3](/assets/img/2025-06-16-gitblog-gen-2/3.png){: .shadow .rounded-10}

> `livereload`옵션을 사용하시면 `Local` 환경에서 수정하는대로 바로 반영할 수 있습니다. 하지만 `_config.yml`은 수정해도 바로 반영되지 않습니다.
> ```bash
> bundle exec jekyll serve --livereload
> ```
{: .prompt-tip}

#### **Repository를 Remote에 배포하기**
---

지금까지는 `Local` 환경 `Repository`에서만 수정하고 제대로 작동하는지 확인했습니다. 이제는 수정한 `Local` 환경 `Repository`를 `Remote` 환경에 업로드해서 온라인에서 제 블로그에 접근할 수 있는지 확인해보도록 하겠습니다.

1. 업로드 하기 전에 먼저, `_config.yml`를 수정해야합니다. `_config.yml`는 `<username>.github.io` 디렉토리에 바로 위치하고 있기 때문에 찾기 쉽습니다.
![4](/assets/img/2025-06-16-gitblog-gen-2/4.png){: .shadow .rounded-10}
위의 사진은 `_config.yml` 내용의 일부입니다. 다양한 설정값이 있지만 일단은 배포가 목적이기 때문에 `url`설정만 바꿔주도록 하겠습니다. 나머지 설정은 [차후](../gitblog-mod/2025-06-16-gitblog-mod-1.md)에 다루도록 하겠습니다. `<username>` 대신에 자신의 `GitHub username`을 입력해주세요.
```yaml
url: "https://<username>.github.io"
```

2. 이제 업로드할 준비가 완료되었습니다. `git` 명령어를 입력하여 `Remote` 환경에 업로드하도록 하겠습니다. 
```bash
git add .
git commit -m "Generate my Gitblog."
git push origin master
```

3. 업로드를 하면 아래의 화면과 같이 `GitHub`에서 붉은 박스로 표시한 것 처럼 검토가 진행됩니다.
![5_1](/assets/img/2025-06-16-gitblog-gen-2/5_1.png){: .shadow .rounded-10}
업로드가 성공한다면 붉은 박스로 표시한 것 처럼 `Sucess`를 확인할 수 있어요.
![5_2](/assets/img/2025-06-16-gitblog-gen-2/5_2.png){: .shadow .rounded-10}

4. 마지막으로 `https://<username>.github.io`를 주소창에 입력하여, 배포가 되었는지 확인합니다.<br>
(`Sucess`를 확인해도 `Update`를 하는데는 3분 정도 기다려야 합니다.)
![6](/assets/img/2025-06-16-gitblog-gen-2/6.png){: .shadow .rounded-10}

> 앗! `Fail`이 떴네요. 하지만 너무 걱정하지 마세요. 이유를 자세하게 설명해주거든요. 아래의 그림에서는 잘못된 링크를 걸어서 문제가 발생했네요.
> ![5_3](/assets/img/2025-06-16-gitblog-gen-2/5_3.png){: .shadow .rounded-10}
{: .prompt-tip}
## **마치며**
---
이번 포스트에서는 `Chirpy 테마`를 `Fork`해서 개인 블로그로 배포까지 했습니다. 하지만 배포만 하게 되면 오직 자신만의 블로그가 되어 버립니다. 다른 사람들과 블로그를 공유하려면 `Google`, `Naver` 등과 같은 검색 엔진에 노출시켜야 하는데요. 다음 포스트에 이에 관한 내용을 다뤄보도록 하겠습니다.