---
title: "[Prompt Engineering] CoT 논문 리뷰"
categories:
- AI
- Prompt Enginnering
img_path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF"
image:
  path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF/1.png"
---

## **CoT(Chain of Thought)이란?**
`CoT`는 [`"Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"(NeurIPS, 2022년)`](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf) 논문에서 제시된 `LLM Reasoning(대규모 언어 모델 추론)` 기법입니다. 해당 논문에서는 `LLM Reasoning` 기법인 `CoT(Chain of Thought, 사고 과정)`를 제시하여, `Prompt Engineering` 분야에서 `Reasoning` 중심의 `Prompt` 기술의 중요성을 부각시켜서 해당 분야를 확장시킨 랜드마크 논문라는 점에 큰 의미가 있습니다.
> **`CoT`는 `Arithmetic(수리)`, `Commonsense(상식적)`, `Symbolic(논리)` 추론 성능을 향상시킨다는 점이 가장 큰 핵심이라고 볼 수 있습니다.**

이 문장을 이해하기 위해서는 먼저, `Arithmetic`, `Commonsense`, `Symbolic` 추론 성능에 대해서 이해할 필요가 있습니다.

### **Arithmetic Reasoning(수리 추론)**
> **`Arithmetic Reasoning`는 숫자 계산 등과 같은 단순 계산을 넘어서, 문장을 읽고 적절한 수학적 연산을 선택하고 적용하는 능력입니다.**

예시를 확인하면 좀 더 이해하기 쉬울 겁니다.

---
**예시)**

**Q: 철수는 사과를 3개 들고 있었고, 민수가 2개 더 주었다. 철수는 사과를 총 몇 개 갖게 되는가?**

**A: 5개**

---

위의 질문에서 `3 + 2`를 하라는 지시는 없었지만, 인간은 쉽게 `더하기`를 해야겠다는 생각을 합니다. 이러한 추론 능력이 바로 `Arithmetic Reasoning`입니다. 하지만, `LLM`은 이러한 `Arithmetic Reasoning` 능력이 부족합니다.

### **Commonsense Reasoning(상식적 추론)**
> **일상적인 상식과 경험을 바탕으로, 명시되지 않은 정보도 추론해서 이해하거나 답을 도출하는 능력입니다.**

예시를 확인하면 좀 더 이해하기 쉬울 겁니다.

---
**예시)**

**Q: 철수는 운동을 마친 후 옷이 땀으로 흠뻑 젖었다. 철수가 가장 먼저 할 일은 무엇일까?**

**A: 옷을 갈아입는다.**

---

위의 질문에서는 `옷을 갈아입는다.`라는 문장이 없습니다. 하지만, 일상적인 상식과 경험이 있다면 `옷이 땀으로 흠뻑 젖었다.`→`옷을 갈아입는다.`가 자연스럽게 답변으로 `Commonsense Reasoning`가 됩니다. `LLM`에서는 이러한 비직접적 정보 추론이 어렵기 때문에 `CoT`를 사용합니다.

### **Symbolic Reasoning(논리적 추론)**
> **수식, 기호, 규칙 등 기호 기반 규칙이나 패턴을 이해하고 적용하는 사고 능력입니다.**

예시를 확인하면 좀 더 이해하기 쉬울 겁니다.

---
**예시)**

**Q: 만약 A → B이고, B → C이면, A → ?**

**A: C**

---

위의 질문에서는 `→`와 같은 논리 도식을 사용했습니다. 우리와 같은 인간은 `→`를 `~가 ~이다.`로 약속해서 사용하고 있기 때문에 규칙 찾기, 논리 도식 등에서 `Symbolic Reasoning`을 사용할 수 있습니다. 하지만, `LLM`에게는 어려운 추론이기 때문에 `CoT`로 도와준다고 생각하시면 되겠습니다.

>**2026년인 현재**, 여기까지 읽어 보신 분들은 살짝 의아할 수 있습니다. **"난 `CoT`를 사용하지 않아도 `LLM`이 저런 추론들 다 하던데..."**라고 생각하실 분들이 계실겁니다. 
>
>**맞습니다!!** 해당 논문은 2022년 당시 최신 모델인 `ChatGPT v3`를 가지고 실험했기 때문에 `CoT`가 꼭 필요한 기법이었습니다.
>
>**하지만, 지금은 `CoT` 방식이 사전 학습과 튜닝 과정에 내재되어 있고 내부적으로 자동 활성화되는 방식으로 작동하기 때문에, `CoT Prompt`없이도 추론을 잘하게 되었습니다.**
{: .prompt-warning}
## **기술 동향**
`NLP(Natural Language Processing, 자연어 처리)`관련 기술은 `Language model(언어 모델)`들을 통해서 발전해왔다고 합니다. `LM`모델이 커지면서 성능향상과 `Sample Efficiency`와 같은 이점을 얻었습니다.
>>**`Sample Efficiency`는 모델이 얼마나 적은 학습 데이터(샘플)만으로도 높은 성능을 낼 수 있는지를 나타내는 개념입니다.**
>
>예를 들어, `GPT`처럼 수십억 개의 파라미터를 가진 대형 언어 모델은 모델 규모가 커질수록 동일한 데이터로 더 잘 일반화하거나, 적은 `Fine-Tuning` 데이터로도 높은 성능을 내는 경향이 있어서 `Sample Efficiency` 관점에서 좋아졌다고 판단합니다.
{: .prompt-info}

>**하지만, `LM`모델이 커져도 `Arithmetic`, `Commonsense`, `Symbolic` 추론 성능 분야에서는 취약한 모습을 보여왔습니다.**

이를 해결하기 위해서 두 가지 아이디어를 사용했다고 합니다.
1. `Arithmetic Reasoning` 성능을 높이기 위해서는 최종 답에 도달하기까지의 자연어 기반 풀이 과정(`Rationales`)을 생성하는 것이 도움 된다는 점
: - 이전 연구들은 모델이 자연어 기반 중간 사고 과정(`Intermediate Steps`)을 생성할 수 있도록 모델을 새로 학습, 사전학습한 모델에 `Fine-Tuning`, 자연어 대신 형식 언어(`Formal Language`)를 사용하는 신경-기호적 방법(`Neuro-Symbolic Methods`)를 사용했습니다.
- **참고.** 최종 답에 도달하기까지의 자연어 기반 풀이 과정(`Rationales`) ≈  자연어 기반 중간 사고 과정(`Intermediate Steps`)

2. `LLM`은 `Prompting`을 이용한 `In-Context Few-Shot Learning`이 유망하다는 점
: - 즉, 새로운 작업마다 별도의 언어 모델을 `Fine-Tuning`하는 것보다 입력–출력 예시 몇 개(`Few Shot`)만 `Prompt`에 포함(`In-Context`)시키는 것만으로도 모델에게 작업을 알려줄 수 있는 것입니다.
- 해당 방식은 이미 [`"Language models are few-shot learners"(NeurIPS, 2020년)`](https://papers.nips.cc/paper_files/paper/2020/hash/1457c0d6bfcb4967418bfb8ac142f64a-Abstract.html) 논문에서 입증이 되었습니다.

하지만, 위의 두가지 아이디어에는 큰 한계점이 있다고 합니다.
1. `Rationale(풀이 과정)` 기반 학습 및 `Fine-Tuning` 방식은, 고품질의 `Rationale`을 대량으로 만드는 데 높은 비용이 듭니다.
: - 일반적으로 입력-출력 쌍으로 학습시키는 것보다 `Rationale(풀이 과정)`을 대량으로 만들어서 학습시키는 과정에서 많은 비용이 듭니다.
2. 전통적인 `Few-Shot Prompting`(= `Prompting`을 이용한 `In-Context Few-Shot Learning` 방식)은 `Reasoning` 능력 성능이 매우 낮다는 점입니다.
: - `LM`의 규모를 키운다고 해도 `Reasoning` 능력은 향상하지 않는다고 합니다.

그래서 이 논문에서는 두 아이디어의 장점은 살리되, 단점을 회피할 수 있는 새로운 방식을 제안합니다.
> **입력(`Input`), 사고 과정(`Chain of Thought`), 정답(`Output`)으로 이루어진 3요소를 `Prompt`로 주는 방식입니다.**

이 방식을 통해서 `LM`이 `Reasoning` 문제에 대해 `Few-Shot Prompting`을 잘 수행할 수 있는 지를 탐색하는 것이 이 논문의 주요한 내용입니다.

>**`Chain of Thought`는 최종 답에 도달하기 위한 일련의 중간 자연어 추론 단계들이며, `Chain of Thought`를 사용하는 방식을 `Chain of Thought Prompting`이라고 정의하고 있습니다.**

아래의 사진이 바로 **`Chain of Thought Prompting`** 입니다.

![1]({{ page.img_path }}/1.png){: .shadow .rounded-10}

## **CoT(Chain of Thought) Prompting이란?**
우리가 수학적 문제를 풀 때, 풀이 과정을 적는 것 처럼 `LM`에도 이와 같은 사고 과정(`CoT`)을 생성할 수 있는 능력을 부여하는 것이 `CoT(Chain of Thought) Prompting`의 목표입니다.

>**즉, 모델이 충분히 큰 `LLM`이면, `Few-Shot Prompting`에 `CoT(Chain of Thought)`예시를 포함시켜 줬을 경우, 모델이 스스로 `CoT(Chain of Thought)`를 생성할 수 있다는 것이  `CoT(Chain of Thought) Prompting`입니다.**

참고로 논문에서는 답에 도달하기 위한 사고의 흐름을 모방하는 것이기 때문에 이 과정을 풀이(`Solution`)나 설명(`Explanation`)으로 명명하지 않고 사고 과정(`CoT`)이라고 명명했다고 합니다.

### **CoT Prompting의 특성**
1. `CoT`는 원칙적으로 모델이 복잡한 다단계 문제를 중간 단계들로 나누어 처리할 수 있게 해줍니다.
: - 중간 단계 중에서 더 많은 추론이 필요한 단계가 있다면, 해당 단계에 더 많은 계산 자원을 할당할 수 있습니다.
2. `CoT`는 모델의 행동을 해석할 수 있는 창(`Window`)을 제공합니다.
: - 모델이 어떻게 특정한 답에 도달했는지를 추정할 수 있는 단서가 됩니다.
- 각 단계에서 어떤 답이 나왔는지 알고 있으면, 어디서 추론이 잘못되었는지를 디버깅할 수도 있습니다.
- 하지만 모델 내부에서 실제로 어떤 계산이 이루어졌는지를 완전히 규명하는 것은 아직 미해결 과제라고 합니다.
3. **`CoT` 방식의 추론은 `Arithmetic`, `Commonsense`, `Symbolic` 과제에 활용될 수 있으며, 원칙적으로는 인간이 언어를 통해 해결할 수 있는 모든 과제에 적용 가능합니다.**
4. **`CoT` 추론은 충분히 큰 범용 언어 모델(`Off-The-Shelf LLM`)에 대해, `Few-Shot Prompting` 예시에 `CoT` 예시를 포함시키는 것만으로도 쉽게 유도할 수 있습니다.**

## **실험**
[앞서 설명한 `Arithmetic`, `Commonsense`, `Symbolic` 추론 성능](#cotchain-of-thought이란)을 평가 기준으로 측정한 결과를 살펴보도록 하겠습니다.
### **Arithmetic Reasoning 측정**
[앞서 설명한 대로](#arithmetic-reasoning수리-추론) `LLM`에게는 꽤 어려운 과제입니다. 하지만 이 실험에서는 `540B 파라미터`를 가진 `LLM`에 `CoT Prompting`을 적용시킴으로서, 특정 작업에 맞춰서 파인 튜닝한 모델과 비슷한 성능을 보인다고 합니다. 특히, 난이도가 있는 `GSM8K` 벤치마크에서 `SOTA`를 달성했다고 합니다.
#### **실험 환경**
- **벤치 마크**
: `GSM8K`, `SVAMP`, `ASDiv`, `AQuA`, `MAWPS` 데이터셋을 사용했습니다. 만약에, 각 데이터셋에 대한 예시가 궁금하시면 [`본 논문`](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 12`를 살펴보시기 바랍니다.

	- GSM8K
	: 전통적인 수학 문장 문제로 아래의 예시처럼 질문이 주어집니다.

	- SVAMP
	: 문제 구조가 다양한 수학 문장 문제입니다.
	
	- ASDiv
	: 문제 유형이 다양한 수학 문장 문제입니다.

	- AQuA
	: 대수 기반의 문장 문제이며, 객관식으로 주어집니다.
		
	- MAWPS(SingleOp, SingleEq, AddSub, MultiArith)
	: 다양한 출처에서 수집된 수학 문제입니다.
	
	---
	**벤치 마크 GSM8K 예시)**

	Q:
	: Josh decides to try flipping a house. He buys a house for $80,000 and then puts
	in $50,000 in repairs. This increased the value of the house by 150%. How
	much profit did he make?

	번역:
	: Josh는 주택 재테크를 시도해 보기로 결심합니다. 그는 집을 $80,000에 구입한 후, $50,000을 들여 수리를 합니다. 이로 인해 집의 가치는 150% 증가합니다. Josh는 얼마의 이익을 얻게 되었을까요?

	---
	
- **표준 Prompting**
: `Baseline`으로 [`"Language models are few-shot learners"(NeurIPS, 2020년)`](https://papers.nips.cc/paper_files/paper/2020/hash/1457c0d6bfcb4967418bfb8ac142f64a-Abstract.html) 논문의 `Few-Shot Prompting` 방식을 사용한다고 합니다. `Few-Shot Prompting` 방식은 `입력-출력 쌍 형태의 예시`를 먼저 제시를 합니다. 이후, `테스트용 예시`를 입력하여 모델에게는 `모델의 직접적인 답변` 출력만을 요구합니다. 모든 예시들은 전부 질문과 답변들로만 이루어져 있습니다. 아래의 그림을 보면 조금더 이해가 쉬울 것입니다.

	![2]({{ page.img_path }}/2.png){: .shadow .rounded-10}

- **CoT Prompting**
: `CoT Prompting`에서 가장 중요한 것은 사고 과정(`CoT`)을 포함시키는 것입니다. 기존 벤치마크는`CoT`가 없고 오직 질문과 답만 있기 때문에, 해당 논문에서는 총 8개의 `Few-Shot 예시`에 맞는  `CoT`를 수작업으로 구성했습니다. 아래의 그림과 같은 8개의 동일한 `CoT` 예시 집합을 `AQuA`를 제외한 모든 벤치마크에서 사용했다고 합니다.
	
	![3]({{ page.img_path }}/3.png){: .shadow .rounded-10}
	
> `AQuA` 벤치마크는 [앞서 말했듯이](#실험-환경) 객관식으로 구성되어 있기 때문에, 위의 `CoT` 예시 집합은 `AQuA` 벤치마크에서 사용하지 못합니다. 대신에, `AQuA` 벤치마크에서는 해당 벤치마크에서 4개의 `Few-Shot 예시`를 뽑아내고 이에 맞는 `CoT`를 직접 작성해줬다고 합니다. 해당 예시는 [본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 21`를 읽어보시면 되겠습니다.
{: .prompt-info}

- **LLM**
: 총 5 종류의 `LLM`을 사용하여 실험했다고 합니다. `GPT-3`, `LaMDA`(422M, 2B, 8B, 68B, 137B), `PaLM`(8B, 62B, 540B), `UL2 20B`, `Codex`(code-davinci-002)을 사용했다고 합니다.

	- GPT-3의 사용모델
	: `GPT-3`을 기반으로 사람의 명령을 더 잘 이해하도록 만든 모델인 `InstructGPT`를 사용했습니다. 해당 모델을 기반으로 파라미터 크기가 다른 총 4개의 모델,`text-ada-001`(약 350M)`text-babbage-001`(약 1.3B), `text-curie-001`(약 6.7B), `text-davinci-002`(약 175B)을 사용했다고 합니다.

- **LM 생성 방식 및 실험 설정**
	- 모든 `LLM`모델은 `Greedy Decoding`으로 샘플링했다고 합니다.<br>(참고로 [`"Self-consistency improves chain of thought reasoning in language models."`](https://arxiv.org/pdf/2203.11171) 논문에서는 여러 번 생성해서 가장 많이 나온 답을 선택하면 성능이 향상된다고 합니다.)
	- `LaMDA` 모델과 같은 경우는 5개의 랜덤 시드로 예시 순서를 섞어서 평균 결과를 확인했다고 합니다. 하지만, 시드 간의 차이가 크지 않았기 때문에, 다른 모델들은 단일 예시 순서만으로 실험 결과를 확인했다고 합니다. 
	> **즉, 앞서 수작업으로 작성했었던 `CoT`를 추가한 `Few-Shot 예시` 8개를 순서를 바꿔가면서 학습을 시켜봤지만 성능차이가 없었다고 이해하시면 됩니다.**

	> **`Greedy Decoding`으로 샘플링 했다고 했는데, `Greedy Decoding`이 뭘까요?**
	> <br>일단 이러한 전략이 왜 필요한지 이해하면 좋을 것 같습니다. `LLM`이 매 순간 어떤 단어를 고를지 결정할 필요가 있는데, 이때 어떤 전략을 선택하는가에 따라서 문장이 달라진다고 합니다. 그 중 `Greedy Decoding`은 `가장 높은 확률을 가진 단어를 매 단계에서 하나씩 선택하는 방식`이라고 합니다. 
	> <br><br> 예를 들어, 다음 단어 후보가 `apple`(0.4), `banana`(0.25), `orange`(0.22), `table`(0.13)이라고 했을 때, 확률이 가장 높은 `apple`이 다음 단어로 선택됩니다. 
	> <br>
	>> **즉, `Greedy Decoding` 방식은 단순하고 빠르지만, 항상 높은 확률의 단어만 선택하기 때문에 창의성이나 다양성이 떨어질 수 있다는 단점이 있다고 합니다.**
	>
	>그래서 `Beam Search`, `Random Sampling` 등의 다양한 전략들도 적재적소로 사용됩니다.
	{: .prompt-info}

#### **실험 결과**
실험 결과를 요약하면 아래의 `Figure 4`와 같습니다. 이 실험으로 도출된 세가지 핵심 요점이 있다고 합니다. 자세한 실험 결과를 확인하고 싶으시다면 [본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 2`를 읽어보세요.
![4]({{ page.img_path }}/4.png){: .shadow .w-50 .rounded-10}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Figure 4`_
![5]({{ page.img_path }}/5.png){: .shadow .rounded-10}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 2`_

1. **CoT Prompting은 LLM에서 모델 Scale에 따라 `Emergent Ability`가 보입니다.**
	- 위의 `Figure 4`를 보면 알 수 있듯이 `100B`보다 큰 규모의 모델에서는 `Emergent Ability`가 보입니다.
	- 소형 모델들은 문법적으로는 유창하지만, 논리적으로 맞지 않는 `CoT`를 생성해 오히려 `Standard Prompting`보다 성능이 더 낮은 경우도 있다고 합니다.

	> **`Emergent Ability`란?**
	> <br>모델의 규모가 일정 수준을 넘었을 때, 이전에는 보이지 않던 새로운 능력이 갑자기 나타나는 현상입니다. 위의 `Figure 4`에서 `GPT`를 `GSM8K` 데이터셋으로 실험한 결과를 살펴봤을때, 규모(`scale`)가 7B일 때는 `Standard prompting`과 `Chain-of-thought prompting` 간의 `solve rate` 값 차이가 크게 나지 않습니다. 하지만 178B에서 `Standard prompting`과 `Chain-of-thought prompting` 간의 `solve rate` 값 차이가 확실하게 나오면서 `Emergent Ability`가 발생했다고 볼 수 있습니다. [`"Emergent Abilities of Large Language Models"`](https://arxiv.org/abs/2206.07682) 논문을 참고해주세요.
	{: .prompt-info}

2. **문제 난이도가 높을수록 CoT의 성능 향상이 더 큽니다.**
	> **`데이테셋의 문제 난이도`란?**
	> <br>문제 난이도는 위의 `Figure 4`의 `Standard prompting` 실험 결과로 판단합니다. `GSM8K` 데이터셋은 모델의 규모가 커져도 `solve rate`가 `20%`로 넘기지 못하며 크게 향상되지 않습니다. 반면에 `MAWPS` 데이터셋은 규모가 `100B`를 넘겼을때, `solve rate`가 크게 향상됩니다. 그래서 `GSM8K`은 비교적 문제의 난이도가 높다고 하고 `MAWPS`는 비교적 문제의 난이도가 낮다고 합니다.
	{: .prompt-info}

	- 위의 `Figure 4`를 살펴보면, `GSM8K` 데이터셋과 같이 난이도가 높은 문제에서는 `GPT` 및 `PaLM`과 같은 대형 모델에서 `CoT prompting`이 모델의 성능을 2배 이상 향상시킵니다.
	- 반면, `MAWPS`처럼 단계가 한 번이면 풀리는 난이도가 낮은 문제에서는 성능 향상이 없거나 오히려 떨어지기도 한다고 합니다.
	<br>([본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 3`를 참고하시면 더 잘 이해할 수 있을 것입니다.)

3. **GPT-3 175B 및 PaLM 540B는 기존 `SOTA`와 비교해도 뛰어납니다.**
	- `CoT prompting`을 적용한 `PaLM 540B`는 `GSM8K`, `SVAMP`, `MAWPS`에서 새로운 최고 성능을 달성했습니다.
	<br>(`GPT-3 175B`는 `SVAMP` 데이터셋으로 실험한 결과가 `SOTA`와 크게 다르지 않아서 `PaLM 540B`를 중정점으로 다룬 것 같습니다.)
	- 단, `SVAMP`의 경우는 `Standard prompting`만으로도 기존 최고 성능을 이미 넘겼습니다.
	- `GSM8K`, `SVAMP`, `MAWPS` 외의 나머지 두 데이터셋 (`AQuA`, `ASDiv`)에서는 `CoT prompting`를 적용한 `PaLM`의 성능이 `Standard prompting` 실험 값의 2% 이내로 근접함으로 큰 성능 향상은 보이지 않았습니다.

위의 세가지의 핵심 요점 뿐만 아니라, **`CoT 추론이 왜 효과적인지?`**, **`모델 Scaling이 왜 CoT의 능력을 개선시키는지?`**에 대한 이유도 분석했다고 합니다.

CoT 추론이 왜 효과적인지?
: 이를 확인하기 위해서 `LaMDA 137B`가 생성한 `CoT`를 수작업으로 분석했다고 합니다.
- `GSM8K` 데이터셋에서 정답을 맞힌 50개의 랜덤 샘플 중, 48개는 논리적·수학적으로도 올바른 `CoT`를 생성했다고 합니다. 하지만 나머지 2개는 우연히 정답을 맞힌 경우였다고 합니다.
<br>([본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix D.1`과 `Appendix Table 8`를 참고해주시면 되겠습니다.)

- `GSM8K` 데이터셋에서 정답을 틀린 50개의 랜덤 샘플 중, 46%는 거의 맞았지만 사소한 실수(계산 실수, 기호 해석 오류, 한 단계 누락 등)가 있었다고 합니다. 나머지 54%는 주요한 의미 이해 실패나 논리 불일치 오류를 포함하고 있었다고 합니다.
<br>([본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix D.2`를 참고해주시면 되겠습니다.)

모델 Scaling이 왜 CoT의 능력을 개선시키는지?
: `CoT` 능력이 `Scaling`(스케일 증가)와 함께 개선되는 이유를 파악하기 위해, `PaLM 62B`와 `PaLM 540B`가 만든 오류를 비교 분석했다고 합니다.
- `PaLM 540B`는 `PaLM 62B` 모델에서 나타난 주요 오류 유형(단계 누락, 의미 이해 실패 등)을 상당 부분 해결했다고 합니다.
<br>([본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix A.1`을 참고해주시면 되겠습니다.)

#### **Ablation Study**
`CoT prompting`이 성능을 향상시킨다는 결과가 관찰되면서, 자연스럽게 다음과 같은 의문이 제기됩니다.

> **"이러한 성능 향상이 정말 CoT 자체 때문인가, 아니면 다른 형태의 프롬프팅으로도 동일한 효과를 얻을 수 있는가?"**

이를 검증하기 위해 연구진은 아래의 `Figure 5`에서 제시된 세 가지 `Ablation Study`을 수행했다고 합니다.

![6]({{ page.img_path }}/6.png){: .shadow .w-50 .rounded-10}
_[본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Figure 5`_

1. **Equation Only**
- 실험 이유
: `CoT prompting`이 효과적이라고 추정되는 이유 중 하나는 **"LLM이 문제를 해결하기 위해서 수학적 `Equation`을 생성하기 때문이다."**입니다. 그래서 이를 검증하기 위해서 `Equation Only`를 실험했다고 합니다.
- 실험 조건
: 모델이 최종 답변을 내기 전에 수학적 `Equation`만 출력하도록 하는 프롬프트를 실험했다고 합니다.
- 실험 결과 
: 위의 `Figure 5`를 보면 알 수 있듯이 `Equation Only` 방식은 `GSM8K` 데이터셋에서는 큰 도움을 주지 못했습니다. **이는 `CoT`에서 제공되는 자연어 기반 `Reasoning` 단계를 거치지 않고는 `GSM8K` 데이터셋 질문의 `Sementics(의미들)`를 `Equation`으로 변환하기 어렵다는 것을 시사한다고 합니다.** 
: 반면, 한두 단계 정도의 단순한 `Reasoning`만 필요한 데이터셋에서는 `Equation Only` 방식이 실제로 성능을 향상시켰다고 합니다. 이러한 데이터에서는 질문으로부터 `Equation`을 비교적 쉽게 도출할 수 있기 때문입니다.
<br>([본 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)의 `Appendix Table 6`을 참고해주시면 되겠습니다.)

2. **Variable Compute Only**
- 실험 이유
: `CoT prompting`이 효과적이라고 추정되는 이유 중 다른 하나는 **"`Reasoning` 과정 자체가 아니라, 모델이 더 많은 토큰을 생성하면서 더 많은 계산을 수행할 수 있기 때문이 아닐까?"**입니다. 그래서 이를 검증하기 위해서 `Variable Compute Only`를 실험했다고 합니다.


CoT에서는 중간 추론 문장을 생성하므로 출력 토큰 수가 늘어난다. 따라서 모델이 어려운 문제에 대해 더 많은 계산 자원을 사용할 수 있게 된다는 설명이 가능하다.

연구진은 이 효과를 CoT의 추론 효과와 분리하기 위해 특수한 실험을 수행하였다.

모델에게 추론 문장을 생성하게 하지 않고, 대신 문제를 해결하는 데 필요한 수식 길이만큼 마침표(.)만 출력하도록 한 것이다.

예를 들어,

..............

와 같은 형태의 출력만 생성하게 하여, 토큰 수는 CoT와 비슷하게 유지하되 실제 추론 내용은 제거하였다.

실험 결과 이 방식은 기본 프롬프팅(Standard Prompting)과 거의 동일한 성능을 보였다.

이는 단순히 계산량이나 출력 토큰 수가 증가하는 것만으로는 CoT의 성능 향상을 설명할 수 없음을 의미한다.

즉,

CoT의 효과는 "더 오래 생각해서"가 아니라, 자연어로 중간 추론 단계를 표현하는 과정 자체에서 비롯된다는 증거로 해석된다.

3. Chain of Thought After Answer
- CoT의 또 다른 가능성 있는 설명은 다음과 같다.

CoT 프롬프트가 모델이 사전학습(pretraining) 과정에서 습득한 관련 지식을 더 잘 불러오도록 도와주는 것일 뿐 아닐까?

만약 그렇다면 추론 과정이 실제로 답을 구하는 데 필요하지 않을 수도 있다.

이를 검증하기 위해 연구진은 특이한 설정을 실험하였다.

모델이 먼저 정답을 출력한 뒤,

그 다음에 CoT 형태의 추론을 생성하도록 프롬프트를 구성한 것이다.

즉,

정답 → 추론

순서로 출력하게 하여,

실제로 최종 답을 생성할 때 CoT를 활용하는지 여부를 분리해서 측정하였다.

실험 결과 이 방식 역시 기본 프롬프팅과 거의 동일한 성능을 보였다.

이는 CoT의 효과가 단순히 관련 지식을 활성화(knowledge activation)시키는 데서 오는 것이 아니라는 점을 시사한다.

오히려 CoT 안에 포함된 순차적(sequential) 추론 과정 자체가 최종 답변을 생성하는 데 실질적인 역할을 한다는 것을 보여준다.

### **Commonsense Reasoning 측정**

#### **실험 환경**

#### **결과**