---
title: "[Gitblog] Jekyll 기반  Chirpy 테마 Gitblog 생성 - ① 환경설정"
categories:
- Gitblog
- Jekyll기반  Chirpy 테마 Gitblog 생성
---

## **Gitblog란?**
--- 
>**Gitblog는 GitHub 기반의 블로그 플랫폼입니다.**

정식 명칭은 [`GitHub pages`](https://pages.github.com/)입니다.
#### **Gitblog 장점**
---
>**GitHub와 연동하여 계정 관리가 용이하다는 장점이 있습니다.**

개발자들은 프로젝트를 관리하기 위해서 `GitHub`을 적극적으로 활용하기 때문에 계정을 보유한 경우가 많고 주기적으로 관리합니다. 그래서 `Gitblog`를 사용하면 다른 플랫폼의 추가적인 계정 개설이 필요 없습니다.

>**자유도 높은 블로그 플랫폼입니다.**

많은 개발자들이 블로그 플랫폼으로 `Gitblog` 외에도 `Tistory`, `Notion` 등을 활용합니다. `Gitblog` 이외의 블로그 플랫폼들은 직관적이고 쉽게 사용할 수 있어서 많은 개발자들이 애용합니다. 하지만 쉬운만큼 자유도 측면에서 제한적인 부분도 있습니다. `Gitblog`는 `Jekyll`과 같은 정적 웹사이트 생성기를 사용하여 웹을 구축하기 때문에, 좀 더 사용자의 기호에 맞게 블로그 관리가 가능합니다.
#### **Gitblog 단점**
---
>**Git 활용능력과 프론트앤드 개발능력이 없으면, Gitblog를 관리하는데 어려움이 있을 수 있습니다.**


블로그 관리를 위해서는 프론트앤드 개발능력(`HTML`, `CSS` 등)이 요구됩니다. 또한, `GitHub` 원격저장소에 블로그 소스코드들을 관리하기 때문에 `Git` 활용능력이 요구됩니다. 하지만 만들어진 테마를 가져와서 사용하는 경우가 많기 때문에 그렇게까지 깊은 능력을 요하지는 않습니다.

## **Gitblog 환경설정**
---
이 포스트는 `Ubuntu 20.04` 환경 또는 `Mac` 환경을 기준으로 작성했습니다.

#### **Ruby 설치**
---
> **루비는 간결함과 생산성을 강조한 동적인 오픈 소스 프로그래밍 언어입니다. 루비의 우아한 문법으로 자연스럽게 읽히고 쓰기 편한 프로그램을 만들 수 있습니다.**<br>
> _(Ruby 공식 홈페이지)_

`Ruby`는 언어의 문법이 쉽고 확장이 편리해서 잘 디자인된 라이브러리를 이용하면 프로그래밍을 처음 시작한 사람도 복잡한 작업을 상대적으로 쉽게 사용할 수 있다고 합니다. <br>
>아래의 내용들은 [How to Install Ruby on Ubuntu](https://phoenixnap.com/kb/install-ruby-ubuntu) 포스트를 참고하여 작성했습니다.
{: .prompt-info}

##### **Rbenv을 활용한 Ruby 설치**
---
>**Rbenv는 Ruby의 버전을 독립적으로 사용할 수 있도록 도와주는 패키지 입니다.**

`Rbenv`이 없으면 최신 버전의 `Ruby`를 사용하기 어렵습니다.

>`Mac`에서는 `apt` 대신 `brew`를 사용하시면 됩니다.
{: .prompt-info }

1. 패키지 `Repository` 정보를 업데이트 합니다.
```bash
sudo apt update
```
2. `Ruby`를 위한 라이브러리와 컴파일러를 설치해줍니다.<br>
(기존에 개발경험이 있으시다면 설치가 되어있을 것입니다.)
```bash
sudo apt install git curl autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev -y
```
3. (✅ `Ubuntu`, `Mac` 구분) `Rbenv`를 설치합니다.
	- `Ubuntu`에서는 Shell 스크립트 다운로드 및 실행합니다.
	```bash
	curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
	```
	- `Mac`에서는 brew를 사용합니다.<br>
			(`Mac`에서는 기본적으로 `Ruby` 2.6 버전이 설치되어 있지만, 최신 버전이 아니라서 여러 기능이 제한됩니다.)
	```bash
	brew install rbenv ruby-build
	```
4. (✅ `Ubuntu`, `Mac` 구분) `Rbenv`을 사용하기 위해서는 Path 환경변수에 `$HOME/.rbenv/bin`를 추가해줄 필요가 있습니다. `Ubuntu`와 `Mac`은 Path 환경변수를 입력하는 파일이 달라 구분해서 입력해주세요.<br>
(이 작업을 해주지 않으면 `Rbenv`와 `Ruby`의 버전을 제대로 추적할 수 없습니다.)
	- `Ubuntu`에서는 `~/.bashrc`에 Path 환경변수를 추가하도록 하겠습니다.
	```bash
	echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
	echo 'eval "$(rbenv init -)"' >> ~/.bashrc
	source ~/.bashrc
	```
	- `Mac`에서는 `~/.zshrc`에 Path 환경변수를 추가하도록 하겠습니다.
	```bash
	echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
	echo 'eval "$(rbenv init -)"' >> ~/.zshrc
	source ~/.zshrc
	```
5. 제대로 설치가 되었는지 `Rbenv` 버전을 확인합니다.
```bash
rbenv -v
```
6. `Rbenv`로 설치가능한 `Ruby` 버전들을 확인합니다.
```bash
rbenv install -l
```
![1](/assets/img/2025-05-30-gitblog-gen-1/1.png){: .shadow .rounded-10}
7. 설치 가능한 `Ruby` 버전 중 필요한 버전을 설치하도록 합니다. 저 같은 경우는 위에 그림
8. 에서 알 수 있듯이 3.4.4 버전이 가장 최신 버전이네요.
```bash
rbenv install 3.4.4 //3.4.4 대신에 선호하시는 버전으로 수정하셔도 됩니다.
```
8. 전역적으로 어떤 `Ruby` 버전을 사용할지 정해줘야 합니다.
```bash
rbenv global 3.4.4 //7번에서 어떤 버전을 선택했는가에 따라 다르겠죠?
```
9. 제대로 `Ruby`가 설치되었는지 확인해봅니다.
```bash
ruby -v
```
![2](/assets/img/2025-05-30-gitblog-gen-1/2.png){: .shadow .rounded-10}


##### **(Optional) 패키지 Repository로 Ruby 설치(❌ 비추천)**
---
>패키지 `Repository`로 `Ruby`를 설치하는 방법은 최신 버전의 `Ruby`를 사용할 수 없기 때문에 비추천하는 바입니다.
{: .prompt-warning}

<details>
<summary>그래도 알고 싶다면 Click!!</summary>
<div markdown="1">
			
>`Mac`에서는 `apt` 대신 `brew`를 사용하시면 됩니다.
{: .prompt-info }

1. 패키지 `Repository` 정보를 업데이트 합니다.
```bash
sudo apt update
```
2. `Repository` 상에서 `Ruby`를 설치합니다.
```bash
sudo apt install ruby-full -y
```
3. 제대로 설치가 되었는지 `Ruby` 버전을 확인합니다.
```bash
ruby -v
```
		
</div>
</details>

#### **Node.js 설치**
---
`Node.js`는 `JavaScript` 코드를 브라우저 밖에서 실행할 수 있게 해주는 런타임 환경입니다. `Jekyll`과 같은 정적 웹사이트 생성기를 사용하기 위해서는 설치가 필수입니다.<br>
>아래의 내용들은 [Linux/Ubuntu Node js 노드 설치, 최신 버전 설치하기](https://seokbong.tistory.com/273) 포스트를 참고하여 작성했습니다.
{: .prompt-info}

1. (✅ `Ubuntu`, `Mac` 구분) `Node.js`와 `NPM(Node Package Manager)`을 설치해줍니다.
	- `Ubuntu`에서는 아래와 같이 설치합니다.
	```bash
	sudo apt install nodejs && sudo apt install npm
	```
	- `Mac`에서는 아래와 같이 설치합니다.(`NPM` 자동 설치)
	```bash
	brew install node
	```
2. `Node.js`의 버전 관리자인 `n`을 `NPM`을 통해서 설치해줍니다.
```bash
sudo npm install -g n
```
3. 최신 LTS 버전의 `Node.js`와 `NPM`을 설치해줍니다.
```bash
sudo n lts
```

#### **Jekyll 설치**
`Jekyll`은 앞서 언급했던 대로 정적 웹사이트 생성기입니다. 앞선 과정에서 `Ruby`와 `Node.js`를 설치해준 이유가 바로 `Jekyll`을 설치하기 위함입니다.

1. `Bundler`를 설치합니다. `Bundler`는 `Ruby` 패키지 간의 의존성을 관리하는 패키지입니다. `Bundler`도 `Ruby` 패키지이기 때문에 gem을 사용해서 설치합니다. 
```bash
gem install bundler
```
2. `Jekyll`를 설치합니다. 
```bash
gem install jekyll
```
3. `GitHub pages`를 설치합니다. `GitHub pages`(= `Gitblog`)는 `Jekyll`를 `Gitblog`에 랜더링하기 위한 패키지입니다.
```bash
gem install github-pages
```

## **마치며**
---
위의 설치 과정들을 통해서 `Jekyll` 기반의 `Gitblog`를 제작할 수 있는 환경이 설정되었습니다. 어떤 개발을 하든 개발 환경을 구축하는 건 꽤 시간이 걸립니다. 이 포스트가 도움이 되면 좋겠습니다.<br>
다음 포스트에서는 우리의 목표인 `Chirpy` 테마를 `Gitblog`에 어떻게 적용시킬 수 있는지 정리하도록 하겠습니다.
