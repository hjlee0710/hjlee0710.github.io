---
title: "[Prompt Engineering] CoT 논문 리뷰 - ③ 실험(Commonsense, Symbolic Reasoning) 및 결론"
categories:
- AI
- Prompt Enginnering
img_path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF-3"
image:
  path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF-3/1.png"
---

## **Commonsense Reasoning 측정**
`CoT`는 [**"[Prompt Engineering] CoT 논문 리뷰 - ② 실험(Arithmetic Reasoning)"**](../prompt-engineering-cot-2) 포스트에서 다룬 것처럼 수학 서술형 문제에 특히 적합하지만, `CoT`가 자연어 기반이라는 특성 덕분에 실제로는 더 광범위한 종류의 `Commonsense Reasoning(상식 추론)` 문제에도 적용할 수 있습니다. 

> **`Commonsense Reasoning` 문제는 일반적인 배경지식을 전제로 물리적 상호작용과 인간의 상호작용에 대해 추론하는 것을 포함합니다.**

`Commonsense Reasoning`는 세상과 상호작용하는 데 핵심적인 능력이지만, 현재의 자연어 이해 시스템으로는 여전히 해결하기 어려운 과제라고 합니다.

### **실험 환경**

- **벤치 마크**
: `CSQA`, `StrategyQA`, `BIG-bench(Date Understanding, Sports Understanding)`, `SayCan` 데이터셋을 사용했습니다.

	- CSQA
	: 사전 지식을 요구하는 복잡한 `의미(Semantics)`를 포함하는 세상에 대한 상식 질문을 제시합니다.

	- StrategyQA
	: 질문에 답하기 위해 `여러 단계에 걸친 추론 전략(Multi-hop Strategy)`을 도출할 것을 요구합니다.
	
	- BIG-bench
	: 1. Date Understanding
		: 주어진 문맥으로부터 날짜를 추론하는 과제입니다.
	2. Sports Understanding
		: 스포츠와 관련된 문장이 `그럴듯한지(Plausible)` 또는 `그럴듯하지 않은지(Implausible)`를 판단하는 과제입니다.
		
	- SayCan
	: 자연어 명령을 `유한한 선택지 집합(Discrete Set)`으로부터 로봇 행동들의 순서로 매핑하는 과제입니다.
	

	---
	**Commonsense Reasoning 예시들**

	---

	![10]({{ page.img_path }}/10.png){: .shadow}
	_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Figure 3` 중 `Commonsense Reasoning` 예시_
	
	위의 그림에서 노란색으로 하이라이트 표시가 된 부분은 `CoT`입니다.

	---

- **Prompts**
: 앞선 [Arithmetic Reasoning 측정](../prompt-engineering-cot-2)과 동일한 실험 환경을 따랐다고 합니다. `CSQA`와 `StrategyQA`의 경우에는 학습 데이터셋에서 예시들을 무작위로 선택한 뒤, 이를 `Few-shot` 예시로 사용하기 위해 `CoT`를 수작업으로 작성했다고 합니다. 두 개의 `BIG-bench` 데이터셋은 학습 데이터셋이 존재하지 않기 때문에, 평가 데이터셋의 처음 10개의 예시를 `Few-shot` 예시로 선택했고 나머지 평가 데이터셋으로 성능 결과를 측정했다고 합니다. 마지막으로 `SayCan`의 경우에는 해당 데이터셋의 논문인 [`"Do As I Can, Not As I Say: Grounding Language in Robotic Affordances(Arxiv, 2022)"`](https://arxiv.org/abs/2204.01691)에서 언급된 학습 데이터셋의 6개 예시를 사용하였으며, 이들에 대해서도 `CoT`를 수작업으로 작성했다고 합니다.

	---
	**SayCan 예시들**

	---

	![11]({{ page.img_path }}/11.png){: .shadow}
	_[`"Do As I Can, Not As I Say: Grounding Language in Robotic Affordances(Arxiv, 2022)"`](https://arxiv.org/abs/2204.01691)의 `Table 4`_

	위의 그림에서 하이라이트 표시가 된 부분이 바로 `CoT`입니다. 위의 예시에는 3개 밖에 없는데, 나머지는 [해당 논문](https://arxiv.org/abs/2204.01691)에서 찾아보시면 되겠습니다.

	---

### **결과**
![12]({{ page.img_path }}/12.png){: .shadow}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Figure 7`_

위의 `Figure 7`은 `LLM` 중의 하나인 `PaLM`으로 실험한 결과입니다. `PaLM` 이외에 `LaMDA`, `GPT-3` 및 다양한 모델 규모에 대한 전체 결과는 [본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Table 4`를 참고해주시기 바랍니다.

모든 데이터셋에서 모델의 `크기를 확장(Scale Up)`할수록 `Standard Prompting`의 성능이 향상되었다고 합니다. 또한 `CoT Prompting`을 적용하면 성능이 더욱 향상되었으며, 이러한 성능 향상은 `PaLM 540B`에서 가장 크게 나타났습니다.

실험 결과를 종합해보면, `CoT Prompting`을 적용한 `PaLM 540B`는 `Standard Prompting`과 비교하여 우수한 성능을 달성했습니다. 구체적으로 `StrategyQA` 데이터셋에서는 `SOTA`를 69.4%에서 75.6%로 뛰어넘었으며, `BIG-bench`의 `Sports Understanding`에서는 `보조 도구의 도움을 받지 않은 스포츠 애호가(Unaided Sports Enthusiast)`의 `Solve rate`인 84%보다 높은 성능인 95.4%를 보였다고 할 수 있습니다.

> **결론적으로 CoT Prompting이 다양한 Commonsense Reasoning 능력을 요구하는 과제에서도 성능을 향상시킬 수 있음을 보여줍니다.** <br>
> (다만 `CSQA`에서는 성능 향상이 매우 제한적이라는 점을 유의해야합니다.)

## **Symbolic Reasoning 측정**
[이 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)에서 마지막으로 다루는 실험은 `Symbolic Reasoning(형식적 추론)`입니다. `Symbolic Reasoning`은 사람에게는 쉽지만, 언어 모델에게는 잠재적으로 어려운 과제일 수도 있다고 합니다. [본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)에서는 `CoT Prompting`이 `Standard Prompting`으로는 수행하기 어려운 `Symbolic Reasoning` 과제를 `LM`이 수행할 수 있도록 할 뿐만 아니라, `Few-shot` 예시에서 보지 못했던 것보다 더 긴 `추론 시점(Inference-time)`의 입력에 대해서도 `길이 일반화(Length Generalization)`를 가능하게 한다는 것을 보여준다고 합니다.

> 길이 일반화(Length Generalization)란?
> : `길이 일반화(Length Generalization)`는 보다 긴 입력이나 더 많은 추론 단계를 요구하는 문제에도 잘 작동하는 것입니다. 길이 일반화를 실험해보기에 `Symbolic Reasoning`이 가장 적합하다고 합니다. 그 이유는 `Symbolic Reasoning`에서 추론 단계 수만 조절하여, **추론 단계 수에 대해서만** 성능 평가가 가능하기 때문이라고 합니다. `Symbolic Reasoning`이 왜 길이 일반화 실험에 적합한지 `Arithmetic Reasoning`와 비교해서 아래의 예시로 살펴보겠습니다.
> : ---
> : **Arithmetic Reasoning 실험이 길이 일반화에 적합하지 않은 이유에 대한 예시**
> : ---
> : **2 단계 Arithmetic Reasoning을 요구하는 Q:**
> : 사과가 3개 있다. 2개를 더 샀다. 1개를 먹었다. 몇 개인가?
> : **추론 단계:**
> : 정의(사과 3개)-> 1번 추론(3 + 2 = 5) -> 2번 추론(5 - 1 = 4) -> 정답: 4개<br><br>
> : **8 단계 Arithmetic Reasoning을 요구하는 Q:**
> : 사과가 3개 있다. 2개를 더 샀다. 1개를 먹었다. 친구가 4개를 주었다. 2개를 팔았다. 3개를 샀다. 5개를 버렸다. 3개를 또 샀다. 1개를 잃어버렸다. 몇 개인가?
> : **추론 단계:**
> : 정의(사과 3개)-> 1번 추론(3 + 2 = 5) -> 2번 추론(5 - 1 = 4) ... -> 8번 추론(7 - 1 = 6) -> 정답: 6개
> : ---
> : 위의 예시에서 볼 수 있듯이 `Arithmetic Reasoning`에서는 각 추론마다 숫자도 달라지고 사용하는 연산이 달라지기 때문에 변수가 많습니다. 그래서 오직 추론 단계 수에 대해서만 성능 평가를 하기에는 적절치는 않습니다.
> : 그럼 이번에는 `Symbolic Reasoning` 예시를 살펴보겠습니다.
> : ---
> : **Symbolic Reasoning([Coin Flip](#tasks)) 예시**
> : `Coin Flip`은 모델에게 사람들이 동전을 뒤집거나 뒤집지 않았을 때, 아직 동전이 계속 앞면을 향하고 있는지를 답하도록 요구하는 과제라고 생각하시면 되겠습니다.
> : ---
> : **2 단계 Symbolic Reasoning을 요구하는 Q:**
> : A coin is heads up. Alice flips the coin. Bob does not flip the coin. Is the coin still heads up?
> : **추론 단계:**
> : Coin의 상태(Heads) -> 1번 추론(Tails) -> 2번 추론(Tails) -> 정답: No<br><br>
> : **8 단계 Symbolic Reasoning을 요구하는 Q:**
> : A coin is heads up. Alice flips the coin. Bob does not flip the coin. Charlie flips the coin. David flips the coin. Emma does not flip the coin. Frank flips the coin. Grace does not flip the coin. Henry flips the coin. Is the coin still heads up?
> : **추론 단계:**
> : Coin의 상태(Heads) -> 1번 추론(Tails) -> 2번 추론(Tails) ... -> 8번 추론(Tails) -> 정답: No
> : ---
> : 이 예시를 보면 알 수 있듯이 `Symbolic Reasoning`의 `Coin Flip` 과제는 단순하게 `Coin`의 상태가 `Heads` 상태인지, `Tails` 상태인지만 기억하면 되고, 일정한 규칙이 있기 때문에 추론 단계 수만 조절하여, **추론 단계 수에 대해서만** 성능 평가가 가능합니다.
{: .prompt-info}


### **Tasks**
아래와 같은 두 개의 `Toy Task`를 수행했다고 합니다.

> Toy Task
> : 복잡한 실제 문제를 단순화하여 특정 능력만 평가하기 위해 만든 작은 규모의 실험 과제. 
> : > **즉, 연구 목적을 위해 의도적으로 단순하게 만든 실험용 과제라고 합니다.**
{: .prompt-info}

- Last Letter Concatenation
: 이 `과제(Task)`에서는 모델에게 이름을 구성하는 각 단어의 마지막 글자를 이어 붙이도록 요구했다고 합니다. 이 과제는 `LM`이 `CoT` 없이도 수행할 수 있는 `첫 글자 이어 붙이기(First Letter Concatenation)`보다 더 어려운 버전이라고 합니다. 예시는 아래와 같습니다.
: ---
: **"Amy Brown" 단어에 대한 First Letter Concatenation, Last Letter Concatenation  예시**
: ---
: - **First Letter Concatenation**: "**A**my **B**rown" → "**AB**"
: - **Last Letter Concatenation**: "Am**y** Brow**n**" → "**yn**"
: ---
: [본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)에서는 실험을 위해 [`Name Census`](https://namecensus.com/) 데이터에서 상위 1,000개의 `이름(First Names)`과 `성(Last Names)`을 무작위로 이어 붙여 `전체 이름(Full Names)`을 생성했다고 합니다.

- Coin Flip
: 이 과제에서는 모델에게 사람들이 동전을 뒤집거나 뒤집지 않았을 때, 아직 동전이 계속 앞면을 향하고 있는지를 답하도록 요구했다고 합니다. 예시는 아래와 같습니다.
: ---
: **Coin Flip 예시**
: ---
: **Q:**
: “A coin is heads up. Phoebe flips the coin. Osvaldo does not flip the coin. Is the coin still heads up?”
: **Q(번역):**
: 동전이 앞면을 향하고 있다. Phoebe가 동전을 뒤집었다. Osvaldo는 동전을 뒤집지 않았다. 동전은 여전히 앞면을 향하고 있는가?
: <br>
: **A:**
: “No”.
: **A(번역):**
: 아니요.
: ---

이러한 `Symbolic Reasoning` 과제는 규칙이 명확하게 정의되어 있기 때문에, 각 과제에 대해 아래와 같은 두 가지 유형의 테스트 세트를 고려했다고 합니다.

1. 학습 데이터 또는 `Few-shot` 예시와 동일한 단계 수를 갖는 예시들로 구성된 `동일 분포(In-domain)` 테스트 세트
2. 평가 예시가 `Few-shot` 예시보다 더 많은 단계를 포함하는 `분포 외(Out-Of-Domain, OOD)` 테스트 세트

`ODD` 테스트 세트를 위해서 `Last Letter Concatenation` 과제에서는 모델이 두 단어로 이루어진 이름만을 `Few-shot` 예시로 본 뒤, 3개 또는 4개의 단어로 이루어진 이름에 대해 마지막 글자 이어 붙이기를 수행하도록 했다고 합니다. 마찬가지로 `Coin Flip` 과제에서도 동전을 뒤집을 수 있는 횟수에 대해 `Last Letter Concatenation`와 동일한 방식을 적용했다고 합니다. 앞서 설명했던 `길이 일반화(Length Generalization)`를 확인하기 위해서 이렇게 `Few-shot` 예시보다 더 많은 추론 단계를 요구하는 질문을 테스트로 사용한 것 같습니다.

### **실험 환경**

실험 환경은 앞의 [`Arithmetic Reasoning 측정`](../prompt-engineering-cot-2), [`Commonsense Reasoning 측정`](#commonsense-reasoning-측정)에서의 실험환경과 동일한 방법 및 모델을 사용했다고 합니다. 또한 각 과제에 대해 `Few-shot` 예시를 위한 `CoT`를 수작업으로 작성하였으며, 이는 아래의 `Figure 3`와 같이 작성했다고 합니다.

![13]({{ page.img_path }}/13.png){: .shadow}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Figure 3` 중 `Symbolic Reasoning` 예시_

### **결과**

이러한 `In-Domain` 및 `OOD` 평가 결과는 `PaLM`에 대해서는 아래의 `Figure 8`과 같습니다.

![14]({{ page.img_path }}/14.png){: .shadow}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Figure 8`_

그리고 `LaMDA`에 대한 결과는 아래의 `Appendix Table 5`와 같습니다.

![15]({{ page.img_path }}/15.png){: .shadow}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 5`_

`In-Domain` 평가에서 `PaLM 540B`는 `CoT Prompting`이 거의 100%에 가까운 `Solve Rate`을 보였습니다. `Coin Flip` 과제에서는 `Standard Prompting`도 100%에 가깝지만 `LaMDA 137B`는 그렇지 않다는 점은 눈여겨 볼만 합니다.

또한 이러한 `In-Domain` 평가는 `Few-shot` 예시에 포함된 `CoT`가 `Symbolic Reasoning` 과제를 수행하기 위한 완전한 해결 방법을 이미 제공하기 때문에, 모델의 `Symbolic Reasoning` 능력을 확인하는 `Toy Tasks`라고 할 수 있다고 합니다.

> **즉, 모델이 하는 일은 `Few-shot` 예시의 `CoT`에서 제시된 동일한 단계들을 테스트 시점에 주어진 새로운 `Symbols(기호)`에 반복해서 적용하는 것으로, `Symbolic Reasoning` 능력을 요구하는 작업입니다.**

그런데 이러한 단순 `Task`에도 불구하고, 작은 규모의 모델에서는 `Few-shot` 예시에 포함된 `CoT`를 통해 문제의 해결 방법이 이미 제공되어 있음에도 여전히 낮은 성능을 보입니다. 해당 논문에서는 `Unseen Symbols(새로운 기호)`에 대해 추상적인 조작을 수행하는 능력이 약 100B 규모의 모델에서 비로소 나타난다고 합니다.

> 논문에서는 이 부분을 다룰 때, `these three tasks`라고 언급하지만 2개의 `Toy task`만 다루고 있습니다. 제가 생각하기에는 오타인 것 같습니다.
{: .prompt-info}

한편, `OOD` 평가에서는 `Standard Prompting`이 두 과제 전부 실패했습니다. 반면 `CoT Prompting`을 사용한 경우에는 언어 모델의 성능이 모델 규모가 커질수록 향상되는 경향을 보였습니다. 다만, 성능은 `In-Domain` 환경에서보다는 낮았습니다.

> **따라서 `CoT Prompting`은 충분히 큰 규모의 `LM`에서 `CoT`로 제시된 것 보다 더 긴 추론 단계를 요구하는 상황에서도 잘 작동하여 `Length Generalization`을 가능하게 한다고 볼 수 있습니다.**

> 표와 그림을 보니, `Coin Flip` 과제의 `OOD` 평가에서 `Standard Prompting`의 `Solve Rate`가 50%인데 실패라구요?
> : `Coin Flip`은 정답이 "앞면이냐? 뒷면이냐?"의 이지선다이기 때문에 50%는 기본으로 먹고 들어가는 점수입니다. 그래서 50%와 비슷하거나 낮으면 실패라고 볼 수 밖에 없습니다.
{: .prompt-info}

## **결론**

해당 논문에서는 `LM`의 추론 능력을 향상시키기 위해 간단하면서도 폭넓게 적용할 수 있는 방법으로 `Chain-of-Thought Prompting`를 탐구했습니다. `Arithmetic, Symbolic, Commonsense Reasoning`에 대한 실험을 통해, `Chain-of-Thought Reasoning`이 모델 규모에 따라 나타나는 [`Emergent Property`](../prompt-engineering-cot-2/#실험-결과)임을 알 수 있었습니다. 
> **즉, `Standard Prompting`에서는 모델 규모를 확장해도 평평한 `Scaling Curve`를 보였던 추론 과제들이, `Chain-of-Thought Prompting`을 사용하면 충분히 큰 규모의 `LM`에서 크게 향상된 성능을 보였습니다.**

마지막으로 해당 논문이 바라는 점은 언어 모델이 수행할 수 있는 추론 과제의 범위를 더욱 확장함으로써 언어 기반 추론 접근법에 대한 후속 연구가 활발히 이어지는 것입니다.

## **마치며**
이번 포스트는 `CoT Prompting`의 `Commonsense, Symbolic Reasoning` 측정 기준에서의 실험결과와 결론까지 살펴봤습니다. 언어 기반 추론 접근법에 대한 설명이라 그렇게 어렵지는 않아서 재밌게 읽었습니다. 그래도 중간중간 이해하기 힘든 부분도 있었기에 포스트를 작성할 때는 이런 부분을 어떻게 하면 더 잘 풀어 쓸 수 있을까 고민했던 것 같습니다. 그런 제 고민이 잘 전달 되었는지 잘 모르겠습니다.<br><br>
`CoT Prompting`에 대해 중요한 내용은 이번 포스트까지 다룬 것만 봐도 된다고 생각합니다. 하지만 아직 해당 논문에서 다루지 않은 부분이 조금 있습니다. `관련 연구(Related Work)`와 `논의사항(Discussion)`입니다. 이 부분은 다음 포스팅으로 짧게 살펴보도록 하겠습니다.