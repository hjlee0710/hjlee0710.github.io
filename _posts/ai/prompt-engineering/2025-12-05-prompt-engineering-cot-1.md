---
title: "[Prompt Engineering] CoT 논문 리뷰 - ① 서론"
categories:
- AI
- Prompt Enginnering
img_path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF-1"
image:
  path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF-1/1.png"
---

## **CoT(Chain of Thought)이란?**
`CoT`는 [`"Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"(NeurIPS, 2022년)`](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf) 논문에서 제시된 `LLM Reasoning(대규모 언어 모델 추론)` 기법입니다. 해당 논문에서는 `LLM Reasoning` 기법인 `CoT(Chain of Thought, 사고 과정)`를 제시하여, `Prompt Engineering` 분야에서 `Reasoning` 중심의 `Prompt` 기술의 중요성을 부각시켜서 해당 분야를 확장시킨 랜드마크 논문라는 점에 큰 의미가 있습니다.
> **`CoT`는 `Arithmetic(수리)`, `Commonsense(상식적)`, `Symbolic(논리)` 추론 성능을 향상시킨다는 점이 가장 큰 핵심이라고 볼 수 있습니다.**

이 문장을 이해하기 위해서는 먼저, `Arithmetic`, `Commonsense`, `Symbolic` 추론 성능에 대해서 이해할 필요가 있습니다.

### **Arithmetic Reasoning(수리 추론)**
> **`Arithmetic Reasoning`는 숫자 계산 등과 같은 단순 계산을 넘어서, 문장을 읽고 적절한 수학적 연산을 선택하고 적용하는 능력입니다.**

예시를 확인하면 좀 더 이해하기 쉬울 겁니다.

---
**예시**

---

**Q: 철수는 사과를 3개 들고 있었고, 민수가 2개 더 주었다. 철수는 사과를 총 몇 개 갖게 되는가?**

**A: 5개**

---

위의 질문에서 `3 + 2`를 하라는 지시는 없었지만, 인간은 쉽게 `더하기`를 해야겠다는 생각을 합니다. 이러한 추론 능력이 바로 `Arithmetic Reasoning`입니다. 하지만, `LLM`은 이러한 `Arithmetic Reasoning` 능력이 부족합니다.

### **Commonsense Reasoning(상식적 추론)**
> **일상적인 상식과 경험을 바탕으로, 명시되지 않은 정보도 추론해서 이해하거나 답을 도출하는 능력입니다.**

예시를 확인하면 좀 더 이해하기 쉬울 겁니다.

---
**예시**

---

**Q: 철수는 운동을 마친 후 옷이 땀으로 흠뻑 젖었다. 철수가 가장 먼저 할 일은 무엇일까?**

**A: 옷을 갈아입는다.**

---

위의 질문에서는 `옷을 갈아입는다.`라는 문장이 없습니다. 하지만, 일상적인 상식과 경험이 있다면 `옷이 땀으로 흠뻑 젖었다.`→`옷을 갈아입는다.`가 자연스럽게 답변으로 `Commonsense Reasoning`가 됩니다. `LLM`에서는 이러한 비직접적 정보 추론이 어렵기 때문에 `CoT`를 사용합니다.

### **Symbolic Reasoning(형식적 추론)**
> **수식, 기호, 규칙 등 기호 기반 규칙이나 패턴을 이해하고 적용하는 사고 능력입니다.**

예시를 확인하면 좀 더 이해하기 쉬울 겁니다.

---
**예시**

---

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
: - 즉, 새로운 작업마다 별도의 언어 모델을 `Fine-Tuning`하는 것보다 입력–출력 예시 몇 개(`Few-Shot`)만 `Prompt`에 포함(`In-Context`)시키는 것만으로도 모델에게 작업을 알려줄 수 있는 것입니다.
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

### **마치며**
이번에는 해당 논문의 `CoT`와 평가 기준인 3가지 추론(`Arithmetic`, `Commonsense`, `Symbolic`)에 대해서 다뤘습니다. 익숙하지 않은 분야이다 보니까 익숙하지 않은 용어를 찾는데 시간이 꽤 걸리는 것 같습니다. 다음 포스트에는 해당 논문의 실험 결과들을 살펴보도록 하겠습니다.
