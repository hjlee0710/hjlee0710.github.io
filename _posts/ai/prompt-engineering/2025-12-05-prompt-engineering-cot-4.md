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

> 결론적으로 `CoT Prompting`은 `LLM`이 성공적으로 수행할 수 있는 과제의 범위를 확장하는 것으로 보입니다.

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
: 이처럼 `LM`의 `사실에 부합하는 생성(Factual Generations)`을 개선하는 것은 향후 연구 방향 중 하나라고 합니다. 해당 논문에서는 이와 관련하여 다음 연구들을 인용하고 있습니다.[`"Measuring Attribution in Natural Language Generation Models"(arxiv, 2021)`](https://arxiv.org/pdf/2112.12870), [`"The Unreliability of Explanations in Few-shot Prompting for Textual Reasoning"(arxiv, 2022)`](https://arxiv.org/pdf/2205.03401), [`"Reframing Human-AI Collaboration for Generating Free-Text Explanations"(arxiv, 2022)`](https://arxiv.org/pdf/2112.08674)
4. `CoT Reasoning`이 큰 모델 규모에서만 `Emergence`한다는 점은 실제 응용 환경에서 서비스를 제공하는데에 많은 비용이 들 수 있습니다.
: 따라서 더 작은 모델에서도 추론을 유도할 수 있는 방법을 향후 연구 방향으로 제시하고 있습니다.

## **관련 연구**
본 연구는 다양한 연구 분야에서 영감을 받았으며, 이에 대해서는 확장된 관련 연구(Related Work) 절(Appendix C)에서 자세히 설명한다. 여기서는 그중에서도 아마 가장 관련성이 높은 두 가지 연구 방향과 그에 관련된 논문들을 설명한다.

첫 번째로 관련된 연구 방향은 추론 문제를 해결하기 위해 중간 단계(intermediate steps)를 사용하는 것이다.

Ling et al. (2017)은 일련의 중간 단계를 거쳐 수학 문장제(Math Word Problems)를 해결하기 위해 자연어 근거(natural language rationales)를 사용하는 아이디어를 선구적으로 제시했다.

이들의 연구는 추론을 위해 형식 언어(formal languages)를 사용한 기존 문헌과 뚜렷한 대조를 이룬다(Roy et al., 2015; Chiang and Chen, 2019; Amini et al., 2019; Chen et al., 2019).

Cobbe et al. (2021)은 더 큰 데이터셋을 구축하고, 모델을 처음부터 학습하는 대신 그 데이터셋을 사용하여 사전 학습된 언어 모델을 Fine-tuning함으로써 Ling et al. (2017)의 연구를 확장했다.

프로그램 합성(Program Synthesis) 분야에서는 Nye et al. (2021)이 먼저 중간 계산 결과(intermediate computational results)를 한 줄씩 예측한 다음, 이를 통해 Python 프로그램의 최종 출력을 예측하는 데 언어 모델을 활용했다. 그리고 이러한 단계별 예측(step-by-step prediction) 방법이 최종 출력을 직접 예측하는 것보다 더 나은 성능을 보인다는 것을 보여주었다.

자연스럽게도 이 논문은 최근 이루어진 방대한 Prompting 연구와도 밀접하게 관련되어 있다.

Brown et al. (2020)에 의해 Few-shot Prompting이 대중화된 이후, 모델의 Prompting 능력을 향상시키기 위한 여러 일반적인 접근법이 제시되었다. 예를 들어, Prompt를 자동으로 학습하는 방법(Lester et al., 2021)이나 과제를 설명하는 지시사항(instructions)을 모델에 제공하는 방법(Wei et al., 2022a; Sanh et al., 2022; Ouyang et al., 2022) 등이 있다.

이러한 접근법들이 Prompt의 입력 부분을 개선하거나 확장하는 것(예: 입력 앞에 지시사항을 추가하는 것)과 달리, 우리의 연구는 이와 **직교하는 방향(orthogonal direction)**을 취하여 언어 모델의 출력에 Chain of Thought를 추가한다.

핵심적으로 마지막 문장을 직역하면

이러한 접근법들이 Prompt의 입력 부분을 개선하거나 확장하는 반면(예를 들어 입력 앞에 지시사항을 추가하는 것), 우리의 연구는 언어 모델의 출력에 Chain of Thought를 추가하는 직교하는 방향을 취한다.

여기서 orthogonal direction은 기존 Prompting 연구와 전혀 관계없다는 의미가 아니라, 기존 연구가 주로 입력(Input)을 개선하는 방향이었다면 CoT 연구는 출력(Output)을 확장하는 다른 축의 접근법이라는 의미로 이해하면 돼.