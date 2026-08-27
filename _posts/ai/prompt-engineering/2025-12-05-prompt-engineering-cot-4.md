---
title: "[Prompt Engineering] CoT 논문 리뷰 - ④ 기타(논의사항, 관련 연구)"
categories:
- AI
- Prompt Enginnering
img_path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF-4"
image:
  path: "/assets/img/posts/ai/prompt-engineering/2025-12-05-prompt-engineering-CoF-4/1.png"
---

## **들어가기 전에**
이번 포스트는 제가 생각하기로는 이 논문에서 중요도가 떨어지는 부분이라고 생각합니다. 그래도 생각할 거리를 주는 `논의사항(Discussion)`과 `관련 연구(Related Work)`이기 때문에 한번쯤은 읽어보시길 추천드립니다.


## **논의사항**
### **앞서 다뤘던 내용들 정리**
[해당 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)은 `LLM`에서 `다단계 추론 행동(Multi-step Reasoning Behavior)`을 이끌어내기 위한 간단한 메커니즘으로서 `Chain-of-Thought Prompting`를 탐구했습니다. 3가지 실험으로 어떤 결과가 나왔는지 정리해보도록 하겠습니다.

1. [`산술 추론(Arithmetic Reasoning)` 실험](../prompt-engineering-cot-2/#arithmetic-reasoning-측정)
: `CoT Prompting`이 `Arithmetic Reasoning`의 성능을 크게 향상시켰습니다. 또한 `Ablation Study`에서 사용한 여러 변형보다 `CoT Prompting`의 성능 향상이 훨씬 크게 나타났으며, [`Robustness of Chain of Thought` 실험](../prompt-engineering-cot-2/#robustness-of-chain-of-thought)을 통해 서로 다른 `Annotator`, `Exemplar`, `LM`에서도 그 효과가 강건하게 나타남을 확인했습니다.

2. [`상식 추론(Commonsense Reasoning)` 실험](../prompt-engineering-cot-3/#commonsense-reasoning-측정)
: `Commonsense Reasoning` 실험을 통해 `CoT Reasoning`의 언어 기반적인 특성(Linguistic Nature)이 다양한 추론 과제에 폭넓게 적용될 수 있음을 확인했습니다.

3. [`기호적 추론(Symbolic Reasoning)` 실험](../prompt-engineering-cot-3/#symbolic-reasoning-측정)
: `CoT Prompting`이 `OOD(Out-of-Domain)` 평가를 통해서 `길이 일반화(Length Generalization)`를 가능하게 한다는 것을 확인했습니다.

모든 실험에서 `CoT Reasoning`은 `기성 언어 모델(Off-the-shelf Language Model)`에 단순히 `Prompting`을 적용하는 것만으로 실험했었습니다. 해당 논문은 어떠한 언어 모델도 `Fine-tuning`하지 않았다고 강조하고 있습니다.

> **그리고 [모델 규모의 증가에 따라 `CoT Reasoning`이 `Emergence(창발)`한다는 것](../prompt-engineering-cot-2/#실험-결과)은 이 논문 전반에 걸쳐 나타나는 주요한 주제였습니다.**

여러 추론 과제에서 `Standard Prompting`은 모델 규모가 증가해도 평평한 `Scaling Curve`를 보였지만, `CoT Prompting`에서는 모델 규모가 커짐에 따라 성능이 급격하게 향상되는 `Scaling Curve`가 나타났습니다. 이를 통해 모델 규모가 커지면서 `CoT Prompting` 능력이 `Emergence`하는 현상을 확인할 수 있었습니다.

> **결론적으로 `CoT Prompting`은 `LLM`이 성공적으로 수행할 수 있는 과제의 범위를 확장하는 것으로 보입니다.**

다시 말해, `Standard Prompting`을 통해 확인되는 성능은 `LLM`이 가진 능력의 하한선에 불과하다는 것입니다.

### **문제 제기**
앞선 결론을 통해 해당 논문으로 답이 제시되었다기 보다는 오히려 더 많은 질문이 제기될 가능성이 있다고 합니다. 예를 들어,

- 모델의 규모를 더욱 증가시키면 추론 능력이 얼마나 더 향상될 것으로 기대할 수 있는가?
- 언어 모델이 해결할 수 있는 과제의 범위를 확장할 수 있는 또 다른 `Prompting` 방법에는 무엇이 있을까?

그리고 `한계점(Limitations)`와 관련하여, 논의해야할 사항들이 있습니다.

1. `CoT`가 인간 추론자의 사고 과정을 모방한다고 하더라도, 이것이 신경망이 실제로 "추론"하고 있다는 것을 의미하는가?
: 해당 논문은 이 질문에 대한 답을 제공하는 것은 아닙니다. 그래서 이를 열린 질문으로 남겨둔다고 합니다.
2. `Few-shot` 환경에서는 `Exemplar`에 `CoT`를 수작업으로 추가하는 비용이 크지 않지만, `Fine-tuning`에서의 `Annotation` 비용은 감당하기 어려울 정도로 커질 수 있습니다.
: 다만, 이는 `합성 데이터 생성(Synthetic Data Generation)`이나 `Zero-shot Generalization`을 통해 잠재적으로 극복할 수도 있다고 합니다.
3. 올바른 `추론 경로(Reasoning Paths)`가 보장되지 않으며, 이로 인해 올바른 답과 잘못된 답이 모두 나올 수 있다고 합니다.
: 이처럼 `LM`의 `사실에 부합하는 생성(Factual Generations)`을 개선하는 것은 향후 연구 방향 중 하나라고 합니다. 해당 논문에서는 이와 관련하여 아래의 연구들을 인용하고 있습니다.
- [`"Measuring Attribution in Natural Language Generation Models"(arxiv, 2021)`](https://arxiv.org/pdf/2112.12870)
- [`"The Unreliability of Explanations in Few-shot Prompting for Textual Reasoning"(arxiv, 2022)`](https://arxiv.org/pdf/2205.03401)
- [`"Reframing Human-AI Collaboration for Generating Free-Text Explanations"(arxiv, 2022)`](https://arxiv.org/pdf/2112.08674)
4. `CoT Reasoning`이 큰 모델 규모에서만 `Emergence`한다는 점은 실제 응용 환경에서 서비스를 제공하는데에 많은 비용이 들 수 있습니다.
: 따라서 더 작은 모델에서도 추론을 유도할 수 있는 방법을 향후 연구 방향으로 제시하고 있습니다.

## **관련 연구**
[우리가 다루고 있는 논문](https://proceedings.neurips.cc/paper_files/paper/2022/file/9d5609613524ecf4f15af0f7b31abca4-Paper-Conference.pdf)에서는 다양한 연구 분야에서 영감을 받았으며, 이에 대해서는 `Appendix C`의 `Extended Related Work`에 자세하게 설명했다고 합니다. 그 중 가장 관련성이 높은 두 가지 연구 방향과 그에 관련된 논문을 다뤘습니다.

### **중간 단계(Intermediate Steps)**
먼저, `중간 단계(Intermediate Steps)`를 사용하여 추론 문제를 해결한 연구 방향에 대해서 살펴보겠습니다.

[`"Program induction by rationale generation: Learning to solve and explain algebraic word problems."(ACL, 2017)`](https://aclanthology.org/P17-1015/)은 일련의 중간 단계를 거쳐 문장으로 구성된 수학 문제를 해결하기 위해 자연어 풀이과정을 사용하는 아이디어를 선구적으로 제시했다고 합니다. 이 연구는 기존에 추론을 위해 형식 언어를 사용한 방식과 뚜렷한 차이점이 있었다고 합니다.

참고로 형식 언어를 사용한 논문들은 아래와 같습니다.
- [`"Solving General Arithmetic Word Problems"(ACL, 2015)`](https://aclanthology.org/D15-1202/)
- [`"Semantically-Aligned Equation Generation for Solving and Reasoning Math Word Problems"(ACL, 2019)`](https://aclanthology.org/N19-1272/)
- [`"MathQA: Towards Interpretable Math Word Problem Solving with Operation-Based Formalisms"(ACL, 2019)`](https://aclanthology.org/N19-1245/)
- [`"Neural Symbolic Reader: Scalable Integration of Distributed and Symbolic Representations for Reading Comprehension"(ICLR, 2019)`](https://openreview.net/forum?id=ryxjnREFwH)

[`"Training Verifiers to Solve Math Word Problems"(Arxiv, 2021)`](https://arxiv.org/abs/2110.14168) 연구는 더 큰 데이터셋을 구축하고, 모델을 처음부터 학습하는 대신에 사전 학습된 언어 모델을 `Fine-tuning`함으로써 `"Program induction by rationale generation: Learning to solve and explain algebraic word problems."(ACL, 2017)` 연구를 확장시켰다고 합니다.

이처럼 중간 단계를 활용하는 접근은 `프로그램 합성(Program Synthesis)` 분야에서도 찾아볼 수 있습니다. [`"Show Your Work: Scratchpads for Intermediate Computation with Language Models"(Arxiv, 2021)`](https://arxiv.org/abs/2112.00114) 연구에서는 `LM`이 `Python` 프로그램의 최종 출력을 바로 예측하는 대신, 중간 계산 결과를 한 줄씩 먼저 예측한 뒤 이를 바탕으로 최종 출력을 예측하도록 했습니다. 이러한 단계별 예측 방식이 최종 출력을 직접 예측하는 것보다 더 좋은 성능을 보였다고 합니다.

> 프로그램 합성(Program Synthesis)
> : 주어진 명세, 제약 조건, 입출력 예시 등을 만족하는 프로그램을 자동으로 생성하는 기술 또는 연구 분야입니다. 우리가 많이 사용하는 `Coding AI`와도 밀접하게 관련된 분야라고 생각하시면 됩니다. 다만 `Coding AI`는 프로그램이나 코드를 생성하는 것뿐만 아니라 코드 완성, 디버깅, 설명, 테스트 생성 등 다양한 작업을 수행한다는 점에서 더 넓은 의미로 사용될 수 있습니다.
{: .prompt-info}

### **Prompting 연구**
당연히 해당 논문은 `Prompting` 연구와도 밀접하게 관련되어 있습니다.

[`"Language Models are Few-Shot Learners"(NeurIPS, 2020)`](https://papers.nips.cc/paper/2020/hash/1457c0d6bfcb4967418bfb8ac142f64a-Abstract.html) 연구에 의해 `Few-shot Prompting`이 대중화된 이후, 모델의 `Prompting` 능력을 향상시키기 위한 여러 일반적인 접근법이 제시되었다고 합니다. 예를 들어,

1. `Prompt`를 자동으로 학습하는 방법 관련 연구
: [`"The Power of Scale for Parameter-Efficient Prompt Tuning"(ACL, 2021)`](https://aclanthology.org/2021.emnlp-main.243/)
2. 과제를 설명하는 지시사항을 모델에 제공하는 방법 관련 연구
: - [`"Finetuned Language Models are Zero-Shot Learners"(ICLR, 2022)`](https://openreview.net/forum?id=gEZrGCozdqR)
- [`"Multitask Prompted Training Enables Zero-Shot Task Generalization"(ICLR, 2022)`](https://arxiv.org/abs/2110.08207)
- [`"Training language models to follow instructions with human feedback"(Arxiv, 2022)`](https://arxiv.org/abs/2203.02155)

이러한 기존 접근법들은 `Prompt`의 입력 부분을 개선하거나 확장하는 방향으로 접근했습니다. 하지만 우리가 다루고 있는 이 `CoT` 논문의 연구에서는 `LM`의 출력에 `CoT`를 추가함으로써 기존 접근법과는 `직교하는 방향(Orthogonal Direction)`으로 접근했음을 다시 한번 강조했습니다.

> 직교하는 방향(Orthogonal Direction)
> : 직역하면 직교하는 방향이라는 의미입니다. 그런데 여기서는 기존 방법을 대체하거나 개선하는 동일한 방향이 아니라, 기존 방법과는 다른 독립적인 측면에서 접근한다는 의미로 사용되었습니다. 기존 `Prompting` 연구가 주로 `Input`을 개선하거나 확장하는 방향이었다면, 본 논문의 `CoT Prompting`은 `Output`에 추론 과정을 추가하는 별도의 방향으로 접근했다는 의미에서 이를 `Orthogonal Direction`이라고 표현한 것입니다.
{: .prompt-info}

## **마치며**
이번 포스트에는 `CoT` 논문의 `논의사항(Discussion)`과 `관련 연구(Related Work)`를 다뤘습니다. 이 연구를 위해 연구자들이 근거를 정말 많이 쌓아왔으며, 엄청난 노력이 느껴졌습니다. 제가 이 논문을 2022년에 봤었으면, `논의사항(Discussion)`에서 다뤘던 내용을 위주로 실험을 해봤지 않았을까 싶습니다. 이로서 `CoT` 논문의 해석은 끝났습니다. 저는 해당 논문을 읽으면서 많은 것을 배울 수 있었습니다. 그런데 해석하고 이해하는데 생각보다 너무 많은 시간이 걸려버려서 당분간은 제가 전문적으로 다뤘었던 `Vision AI` 쪽 논문을 정리할 것 같습니다. 이 포스트를 읽는 여러분께 도움이 되었기를 바라며 다른 주제의 포스팅으로 다시 찾아 뵙겠습니다. 감사합니다.