---
title: "[Technical Notes] 생각보다 간단한 Confusion Matrix"
categories:
- AI
- Technical Notes

math: true
img_path: "/assets/img/posts/ai/technical-notes/2026-10-24-confusion-matrix"
image:
  path: "/assets/img/posts/ai/technical-notes/2026-10-24-confusion-matrix/thumbnail.png"
---

> **해당 포스트는 [`"An introduction to ROC analysis"(Pattern Recognition Letters, 2006년)`](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)를 참고하여 작성했습니다. `ROC` 분석에 대해서 더 알고 싶으신 분은 해당 논문을 한 번 읽어보시길 권해드립니다.**
{: .prompt-info }

## **서론**

`분류 문제(Classification Problem)`를 다룰 때, 모델의 성능을 평가하기 위해 알아두어야 할 중요한 개념 중 하나가 `Confusion Matrix(혼동 행렬)`입니다. 여기서 `분류 문제(Classification Problem)`의 대표적인 유형으로는 `이진 분류(Binary Classification)`, `다중 클래스 분류(Multi-class Classification)`, `다중 레이블 분류(Multi-label Classification)` 등이 있습니다.

이러한 분류 방식은 `Vision AI`의 `Image Classification` 분야에서 직접적으로 활용되며, 객체의 위치와 클래스를 함께 예측하는 `Object Detection` 분야에서도 활용됩니다.

따라서 `Confusion Matrix`는 `Vision AI`의 `Image Classification` 분야에서 모델의 분류 결과를 분석하는 데 활용되며, `Object Detection` 분야에서 사용하는 `TP`, `FP`, `FN` 등의 성능 평가 개념을 이해하는 데에도 중요한 기초라고 할 수 있습니다.

> **이번 포스트에서는 `Confusion Matrix`를 설명한 논문을 기반으로 하되, 조금 더 직관적으로 이해하기 쉽게 설명드리겠습니다.**

## **Confusion Matrix란?**

먼저, [논문](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)에서의 설명을 살펴보겠습니다.

> **`Confusion Matrix`는 `인스턴스(Instance)`들의 집합과 `분류기(Classifier)`가 주어지면, 해당 인스턴스 집합의 분류 결과를 나타내는 `2 x 2` 행렬로 구성할 수 있습니다.**

익숙하지 않은 용어들이 좀 많습니다. 여기서는 두 개의 클래스만 사용하는 `이진 분류(Binary Classification)`를 가정하면서 설명드리도록 하겠습니다.

### **분류기(Classifier)와 인스턴스(Instance)**

분류기(Classifier)
: `Classification Model`이라고 하기도 하며, `인스턴스(Instance)`를 예측 클래스에 `대응시키는 함수(Mapping)`입니다.

: > **즉, 분류기는 인스턴스가 어떤 클래스에 속하는지를 예측하는 역할을 합니다.**

인스턴스(Instance)
: > **일반적으로 머신러닝에서 `Instance`라고 하면 모델이 처리하는 개별 데이터 사례를 의미합니다.**
: 예를 들어, 이미지 분류 문제에서는 모델에 입력되는 이미지 한 장이 `Instance`가 될 수 있고, `LLM`에서는 응답 생성을 요청하는 `Prompt` 하나가 `Instance`가 될 수 있습니다. 
: 다만 이번 포스트에서는 클래스 분류의 대상이 되는 인스턴스를 중심으로 설명하겠습니다.

### **양성(Positive)과 음성(Negative)**

앞선 `Confusion Matrix`의 정의에서 `인스턴스 집합의 분류 결과를 나타내는 2 x 2 행렬`이라는 설명을 이해하기 위해서는 먼저 `양성(Positive)`과 `음성(Negative)`에 대해서 알아야 합니다.

[논문](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)에서는 이진 분류를 가정하여 각 인스턴스가 `양성(Positive)`과 `음성(Negative)` 클래스 레이블로 구성된 집합 $$\{p, n\}$$의 원소 중 하나에 대응된다고 설명합니다.

그런데 여기서 `양성(Positive)`과 `음성(Negative)`은 무엇을 의미할까요?

> **우리가 `관심 있는 클래스(Target Class)`를 지정했을 때, 해당 클래스에 속하는 인스턴스의 `True Class`는 `Positive`로, 속하지 않는 인스턴스의 `True Class`는 `Negative`로 나타냅니다.**

![1]({{ page.img_path }}/1.png){: .shadow .rounded-10}

예를 들면, 저는 `강아지`를 분류하고 싶습니다. 이때, `강아지` 사진인 `Instance`는 `Target Class`인 `강아지`에 속하므로 `Positive`라는 클래스 레이블을 부여합니다. 반면 `고양이`, `말`과 같이 다른 동물 사진은 `Target Class`인 `강아지`에 속하지 않으므로 `Negative`라는 클래스 레이블을 부여합니다.

이러한 구분은 `이진 분류(Binary Classification)`뿐만 아니라 다른 분류 문제에서도 `Target Class`를 기준으로 적용할 수 있습니다.

예를 들어, `강아지`, `고양이`, `말`을 구분하는 다중 클래스 분류 문제에서도 `강아지`를 `Target Class`로 지정하면 `강아지`는 `Positive`, 나머지 `고양이`와 `말`은 `Negative`로 묶어 평가할 수 있습니다.

다만, 다중 클래스 분류 문제에서 `Positive`와 `Negative`로 구분하는 것은 원래의 클래스들을 없애거나 모델을 이진 분류기로 변경한다는 의미가 아닙니다. 특정 클래스의 성능을 평가하기 위해 해당 클래스와 나머지 클래스들을 이진 분류 형태로 구분하는 것입니다.

이제 다시 `강아지`와 `강아지 아님`을 구분하는 이진 분류 문제로 돌아오겠습니다.

지금까지는 인스턴스의 `실제 정답(Ground Truth)`을 기준으로 `True Class`를 구분했습니다. 즉, 모델이 어떤 클래스로 예측했는지와는 관계없이 인스턴스가 실제로 어떤 클래스에 속하는지를 나타낸 것입니다.

하지만 인스턴스를 분류기에 입력하여 예측한 결과 역시 `Positive` 또는 `Negative`로 구분할 수 있습니다.

> **분류기가 `Target Class`에 속한다고 예측한 경우에는 `추정된 클래스(Hypothesized Class)`를 `Positive`로, 속하지 않는다고 예측한 경우에는 `Negative`로 구분합니다.**

![2]({{ page.img_path }}/2.png){: .shadow .rounded-10}

여기서 `추정된 클래스(Hypothesized Class)`는 분류기가 예측한 클래스를 의미합니다.

예를 들어, `강아지`와 `강아지 아님`을 구분하는 이진 분류기가 있다고 가정하겠습니다. 저는 `강아지`를 분류하고 싶습니다.

이때, `강아지` 사진인 `Instance`를 분류기에 입력했더니 분류기가 올바르게 `강아지`로 예측했습니다. 그렇다면 해당 예측 결과는 `Target Class`인 `강아지`에 해당하므로 `Hypothesized Class`는 `Positive`입니다.

만약에 `고양이` 사진인 `Instance`를 입력했을 때, 분류기가 올바르게 `강아지 아님`으로 예측했다면 어떻게 될까요?

예측 결과가 `Target Class`인 `강아지`에 속하지 않기 때문에 `Hypothesized Class`는 `Negative`입니다.

추가로 `고양이` 사진인 `Instance`를 분류기에 입력했더니, 분류기가 잘못 예측하여 `강아지`라고 판단한 경우에는 어떻게 될까요?

비록 잘못된 예측이지만, 예측 결과가 `Target Class`인 `강아지`에 해당하므로 `Hypothesized Class`는 `Positive`입니다.

> **결론적으로 `Positive`와 `Negative`는 예측의 정답 여부가 아니라, 우리가 평가하려는 `Target Class`에 해당하는지를 기준으로 구분하는 것입니다.**

따라서 인스턴스의 `실제 클래스(True Class)`와 분류기가 예측한 `추정된 클래스(Hypothesized Class)`는 서로 다를 수 있으며, 이 둘을 비교하여 분류 결과가 올바른지 판단할 수 있습니다.

### **그래서 Confusion Matrix란?**

앞선 `Confusion Matrix`의 정의를 다시 가져오겠습니다.

> **`Confusion Matrix`는 `인스턴스(Instance)`들의 집합과 `분류기(Classifier)`가 주어지면, 해당 인스턴스 집합의 분류 결과를 나타내는 `2 x 2` 행렬로 구성할 수 있습니다.**

이제 인스턴스가 무엇인지, 분류기가 무엇인지 알고 있습니다. 그런데 왜 인스턴스 집합의 분류 결과가 `2 x 2` 행렬이 되는 것일까요?

우리가 `Confusion Matrix`라고 하면 많이 봤을 그림으로 설명드리도록 하겠습니다.

![3]({{ page.img_path }}/3.png){: .shadow .rounded-10}

앞서 설명드린 [`양성(Positive)과 음성(Negative)`](#양성positive과-음성negative)를 적용한다면 위의 표의 항목들을 잘 이해할 수 있을 것입니다.

`Actual(= True Class, 실제 클래스)`은 `Instance`의 실제 정답이 `Target Class`에 해당하면 `Positive`, 해당하지 않으면 `Negative`로 구분합니다.

`Predicted(= Hypothesized Class, 예측 클래스)`는 `Classifier`에 `Instance`를 입력하여 얻은 예측 결과가 `Target Class`에 해당하면 `Positive`, 해당하지 않으면 `Negative`로 구분합니다.

> **즉, `True Class`와 `Hypothesized Class`가 각각 `Positive`, `Negative`라는 두 가지 경우의 수를 가지고 있기 때문에, 해당 인스턴스 집합의 분류 결과를 `2 x 2` 행렬로 나타낼 수 있는 것입니다.**

이제 `Confusion Matrix`가 왜 `2 x 2` 행렬로 구성되는지 이해했습니다. 다음으로는 표의 각 항목이 무엇을 의미하는지 살펴보고, 이를 통해 어떠한 성능 평가 지표를 계산할 수 있는지 확인해 보겠습니다.

## **Confusion Matrix 해석**

앞서 `True Class`와 `Hypothesized Class`의 조합으로 `Confusion Matrix`가 `2 x 2` 행렬로 구성되는 이유를 살펴봤습니다.

이번에는 `Confusion Matrix`의 각 셀 내부에 있는 용어의 의미를 살펴보고, 이를 활용한 성능 평가 지표들을 알아보겠습니다.

![4]({{ page.img_path }}/4.png){: .shadow .rounded-10}

먼저, `Confusion Matrix`의 각 셀을 구성하고 있는 `True`, `False`, `Positive`, `Negative`라는 용어의 의미를 살펴보겠습니다.

> Hypothesized Class의 Y, N에 대한 설명
> : 위의 그림은 [Fawcett(2006) 논문의 Figure 1](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)에 제시된 `Confusion Matrix`입니다. 논문에서는 실제 클래스(`True Class`)의 `Positive`, `Negative`를 각각 소문자 `p`, `n`으로 나타내고, 이와 예측 클래스를 구분하기 위해 `Hypothesized Class`의 `Positive`, `Negative`는 각각 대문자 `Y`, `N`으로 표기합니다.
{: .prompt-info }


### **True, False, Positive, Negative**

여기서 각각의 용어를 조금 더 쉽게 이해하려면 `True`, `False`와 `Positive`, `Negative`를 구분해서 생각하면 됩니다.

`True`와 `False`는 `True Class`와 `Hypothesized Class`가 일치하는지를 의미합니다.

- **True**
  : 분류기가 올바르게 예측한 경우입니다.
- **False**
  : 분류기가 잘못 예측한 경우입니다.

그런데 `Confusion Matrix`를 살펴보면 조금 특이한 부분이 있습니다. `True`에 해당하는 셀들은 주대각선(`Main Diagonal`)에 위치하고, `False`에 해당하는 셀들은 주대각선 밖에 위치한다는 점입니다.

왜 그런지 `True`에 해당하는 셀들부터 먼저 살펴보도록 하겠습니다.

**True Positive(TP): True Class와 Hypothesized Class가 모두 Positive인 경우**

- `True Class = Positive`
: 실제 인스턴스가 `Target Class`에 속합니다.
- `Hypothesized Class = Positive`
: 분류기가 예측했을 때, 해당 인스턴스가 `Target Class`에 속한다고 예측한 경우입니다.

> **즉, 실제 인스턴스가 Target Class에 속하고, 분류기도 동일하게 예측했으므로 올바른 예측입니다.**

**True Negative(TN): True Class와 Hypothesized Class가 모두 Negative인 경우**

- `True Class = Negative`
: 실제 인스턴스가 `Target Class`에 속하지 않습니다.
- `Hypothesized Class = Negative`
: 분류기가 예측했을 때, 해당 인스턴스가 `Target Class`에 속하지 않는다고 예측한 경우입니다.

> **이 경우에도 실제 인스턴스가 Target Class에 속하지 않고, 분류기도 동일하게 예측했으므로 올바른 예측입니다.**

두 경우의 공통점은 `True Class`와 `Hypothesized Class`가 서로 일치한다는 점입니다.

$$
\text{True Class} = \text{Hypothesized Class}
$$

따라서 `True Positive(TP)`와 `True Negative(TN)`는 분류기가 올바르게 예측한 경우이며, `Confusion Matrix`의 주대각선에 위치합니다.

반면 `False`에 해당하는 셀들은 어떻게 될까요?

**False Positive(FP): True Class는 Negative이지만 Hypothesized Class는 Positive인 경우**

- 실제 인스턴스는 `Target Class`에 속하지 않지만, 분류기는 `Target Class`에 속한다고 잘못 예측했습니다.

**False Negative(FN): True Class는 Positive이지만 Hypothesized Class는 Negative인 경우**

- 실제 인스턴스는 `Target Class`에 속하지만, 분류기는 `Target Class`에 속하지 않는다고 잘못 예측했습니다.

이 두 경우에는 `True Class`와 `Hypothesized Class`가 서로 일치하지 않습니다.

$$
\text{True Class} \neq \text{Hypothesized Class}
$$

따라서 `False Positive(FP)`와 `False Negative(FN)`는 분류기가 잘못 예측한 경우이며, `Confusion Matrix`의 주대각선 밖에 위치합니다.

이제 셀 안의 `Positive`와 `Negative`가 무엇을 의미하는지 살펴보겠습니다.

- **Positive**
  : 분류기가 예측할 때, `Instance`가 `Target Class`에 속한다고 예측한 경우입니다. 즉, `Hypothesized Class`가 `Positive`인 경우입니다.
- **Negative**
  : 분류기가 예측할 때, `Instance`가 `Target Class`에 속하지 않는다고 예측한 경우입니다. 즉, `Hypothesized Class`가 `Negative`인 경우입니다.

> **결론적으로 `True`와 `False`는 실제 클래스와 예측 클래스의 일치 여부를, `Positive`와 `Negative`는 분류기가 예측한 클래스(`Hypothesized Class`)를 의미합니다.**

이처럼 `True Class`와 `Hypothesized Class`를 비교하면 `True Positive(TP)`, `False Positive(FP)`, `False Negative(FN)`, `True Negative(TN)`라는 네 가지 분류 결과를 구분할 수 있습니다.

### **Confusion Matrix를 이용한 성능 평가 지표**

![5]({{ page.img_path }}/5.png){: .shadow .rounded-10}

지금까지 위의 그림에서 `Confusion Matrix`의 네 가지 분류 결과인 `TP`, `FP`, `FN`, `TN`에 대해 알아봤습니다. 이번에는 이러한 분류 결과들을 이용하여 어떻게 모델의 성능을 평가하는지를 살펴보도록 하겠습니다.

먼저, `Confusion Matrix`의 각 셀에는 해당 분류 결과가 발생한 `Instance`의 개수가 기록됩니다. 예를 들어 `TP = 10`이라면, 실제 클래스와 예측 클래스가 모두 `Positive`인 인스턴스가 10개라는 뜻입니다.

그리고 [논문](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)에서는 실제 `Positive` 인스턴스의 전체 개수를 $P$, 실제 `Negative` 인스턴스의 전체 개수를 $N$으로 표기합니다.

$$
P = TP + FN
$$

$$
N = FP + TN
$$

여기서 대문자 `N`은 실제 `Negative` 인스턴스의 전체 개수이며, 앞서 `Hypothesized Class`에서 예측 `Negative`를 나타내기 위해 사용한 `N`과는 의미가 다릅니다.

따라서 전체 인스턴스의 개수는 다음과 같습니다.

$$
P + N = TP + FP + FN + TN
$$

이러한 관계를 바탕으로 각 성능 평가 지표가 무엇을 의미하는지 살펴보겠습니다.

#### **Accuracy (정확도)**

`Accuracy`는 분류기가 전체 인스턴스 중 얼마나 많은 인스턴스를 올바르게 분류했는지를 나타내는 지표입니다.

$$
\text{Accuracy} = \frac{TP+TN}{P+N}
$$

먼저 분모인 `P + N`은 전체 인스턴스의 개수를 의미합니다.

그리고 분자인 `TP + TN`은 분류기가 올바르게 예측한 인스턴스의 개수입니다. 앞서 설명했듯이 `TP`와 `TN`은 모두 `True Class`와 `Hypothesized Class`가 일치하는 경우이기 때문입니다. 따라서 위 수식은 다음과 같이 해석할 수 있습니다.

$$
\text{Accuracy} = \frac{\text{올바르게 분류한 인스턴스 수}}{\text{전체 인스턴스 수}}
$$

> **즉, `Accuracy`는 전체 인스턴스를 기준으로 분류기가 얼마나 정확하게 예측했는지를 나타내는 지표입니다.**

그런데 `Accuracy`에는 한 가지 주의해야 할 점이 있습니다. 전체 인스턴스 중 `Negative`가 차지하는 비율이 매우 높고, 분류기가 이를 대부분 올바르게 예측한다면 `Accuracy` 수식에서 `TN`과 `N`이 큰 비중을 차지하게 됩니다. 이로 인해 상대적으로 `Positive`와 관련된 값들의 영향력이 작아집니다.

> **따라서 분류기가 `Positive`를 제대로 분류하지 못하더라도 `Negative`를 올바르게 분류한 결과가 `Accuracy`에 크게 반영되어 높은 `Accuracy`를 얻을 수 있습니다.**

즉, `Accuracy`가 높다는 사실만으로 모든 클래스에 대해 좋은 분류 성능을 보인다고 판단하기는 어렵습니다.

이러한 한계를 보완하기 위해 특정 클래스에 대한 성능을 살펴볼 수 있는 지표로 `Precision`과 `Recall`이 있습니다.

#### **Precision (정밀도)**

`Precision`은 분류기가 `Positive`라고 예측한 인스턴스 중 실제로 `Positive`인 인스턴스의 비율을 나타냅니다.

$$
\text{Precision} = \frac{TP}{TP+FP}
$$

여기서 분모인 `TP + FP`에 주목할 필요가 있습니다. `TP`와 `FP`는 모두 `Hypothesized Class`가 `Positive`인 경우입니다. 

즉, 분류기가 `Positive`라고 예측한 모든 인스턴스의 개수를 의미합니다.

반면 분자인 `TP`는 그중 실제 클래스도 `Positive`여서 분류기가 올바르게 예측한 인스턴스의 개수입니다. 

따라서 위 수식은 다음과 같이 해석할 수 있습니다.

$$
\text{Precision} = \frac{\text{올바르게 Positive로 예측한 인스턴스 수}}{\text{Positive로 예측한 인스턴스 수}}
$$

> **즉, `Precision`은 분류기가 `Positive`라고 예측한 결과들이 얼마나 정확한지를 나타내는 지표입니다.**

`Precision`이 높다는 것은 `Positive`로 예측한 인스턴스 중 `False Positive(FP)`의 비율이 낮다는 의미입니다.

따라서 잘못된 `Positive` 예측을 줄이는 것이 중요한 상황에서 유용한 성능 평가 지표라고 볼 수 있습니다.

#### **Recall (재현율, True Positive Rate)**

`Recall`은 `True Class`가 `Positive`인 인스턴스 중 분류기가 `Positive`라고 올바르게 예측한 인스턴스의 비율을 나타냅니다.

[논문](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)에서는 이를 `True Positive Rate(TPR)` 또는 `Hit Rate`라고도 설명합니다. `Recall(= TPR)`은 `ROC Curve`를 그릴 때 정말 중요한 개념이기 때문에, 나중에 한 번 더 다루도록 하겠습니다.

$$
\begin{aligned}
\text{Recall} = \text{TPR} &= \frac{TP}{P} \\
&= \frac{TP}{TP+FN}
\end{aligned}
$$

여기서 분모인 `TP + FN`은 실제 클래스가 `Positive`인 모든 인스턴스의 개수입니다. `TP`와 `FN`은 모두 `True Class`가 `Positive`인 경우이기 때문입니다.

반면 분자인 `TP`는 그중 분류기가 `Positive`라고 올바르게 예측한 인스턴스의 개수입니다. 따라서 위 수식은 다음과 같이 해석할 수 있습니다.

$$
\text{Recall} = \frac{\text{올바르게 Positive로 예측한 인스턴스 수}}{\text{실제로 Positive인 인스턴스 수}}
$$

> **즉, `Recall`은 실제 `Positive`인 인스턴스들을 분류기가 얼마나 놓치지 않고 찾아냈는지를 나타내는 지표입니다.**

`Recall`이 높다는 것은 실제 `Positive`인 인스턴스 중 `False Negative(FN)`로 잘못 분류되는 비율이 낮다는 의미입니다. 따라서 실제 `Positive`인 인스턴스를 놓치지 않는 것이 중요한 상황에서 유용합니다.

여기서 `Precision`과 `Recall`의 차이를 다시 살펴보겠습니다.

> **`Precision`은 분류기가 `Positive`라고 예측한 결과를 기준으로 계산하지만, `Recall`은 실제로 `Positive`인 인스턴스를 기준으로 계산합니다.**

두 지표는 모두 `TP`를 분자로 사용하지만, 평가 기준이 다르기 때문에 서로 다른 성능을 나타내는 것입니다.

#### **False Positive Rate (FPR, 거짓 양성률)**

앞서 `Recall`이 실제 `Positive`인 인스턴스를 기준으로 계산하는 지표라는 것을 살펴봤습니다.

이번에는 반대로 실제 `Negative`인 인스턴스를 기준으로도 분류기의 성능을 평가하는 `False Positive Rate(FPR)`를 살펴보도록 하겠습니다.

`False Positive Rate(FPR)`는 실제로 `Negative`인 인스턴스 중 분류기가 `Positive`라고 잘못 예측한 인스턴스의 비율을 나타냅니다.

$$
\begin{aligned}
\text{FPR} &= \frac{FP}{N} \\
&= \frac{FP}{FP+TN}
\end{aligned}
$$

분모인 `FP + TN`은 실제 클래스가 `Negative`인 모든 인스턴스의 개수입니다.

반면 분자인 `FP`는 실제로는 `Negative`이지만 분류기가 `Positive`라고 잘못 예측한 인스턴스의 개수입니다.

따라서 위 수식은 다음과 같이 해석할 수 있습니다.

$$
\text{FPR} = \frac{\text{잘못 Positive로 예측한 인스턴스 수}}{\text{실제로 Negative인 인스턴스 수}}
$$

> **즉, `FPR`은 실제로 `Negative`인 인스턴스들을 분류기가 얼마나 자주 `Positive`로 잘못 판단하는지를 나타내는 지표입니다.**

`FPR`이 높다는 것은 실제 `Negative`인 인스턴스 중 잘못된 `Positive` 예측이 많다는 것을 의미합니다.

따라서 다른 조건이 같다면 `FPR`은 낮을수록 좋습니다.

여기서 주의할 점은 `FPR`과 `Precision`이 서로 다른 기준으로 계산된다는 사실입니다.

두 지표 모두 `FP`와 관련이 있지만, `Precision`은 `Positive`라고 예측한 인스턴스를 기준으로 하고, `FPR`은 실제 `Negative`인 인스턴스를 기준으로 합니다.

또한, `FPR`은 `Recall(= TPR)`과 더불어 `ROC Curve`를 그릴 때 정말 중요한 개념이기 때문에, 나중에 한 번 더 다루도록 하겠습니다.

#### **F-measure (F1-score)**

이번에는 `F-measure`에 대해 살펴보겠습니다. 앞서 `Precision`과 `Recall`은 서로 다른 기준으로 분류기의 성능을 측정한다고 설명했습니다.

그런데 두 지표를 함께 고려하고 싶다면 어떻게 해야 할까요?

해당 [논문](https://www.researchgate.net/publication/222511520_Introduction_to_ROC_analysis)에서는 `F-measure`를 아래와 같이 정의합니다.

$$
\text{F-measure} = \frac{2}{\frac{1}{\text{Precision}}+\frac{1}{\text{Recall}}}
$$

이를 정리하면 다음과 같습니다.

$$
\text{F1-score} = 2 \times \frac{\text{Precision}\times\text{Recall}}{\text{Precision}+\text{Recall}}
$$

이 수식은 `Precision`과 `Recall`의 `조화평균(Harmonic Mean)`을 의미합니다. 일반적인 산술평균과 달리 조화평균은 두 값 중 하나가 상대적으로 낮을 경우 그 영향을 크게 받습니다.

따라서 `Precision`이 높더라도 `Recall`이 낮거나, 반대로 `Recall`이 높더라도 `Precision`이 낮다면 `F1-score`는 두 값의 단순 산술평균보다 낮아집니다.

> **즉, `F1-score`는 `Precision`과 `Recall`을 함께 고려하여 두 지표의 균형을 평가하는 성능 지표입니다.**

이를 `TP`, `FP`, `FN`으로 다시 표현하면 다음과 같습니다.

$$
\text{F1-score} = \frac{2TP}{2TP+FP+FN}
$$

여기서 주목할 부분은 `TN`이 수식에 포함되지 않는다는 점입니다.

즉, `F1-score`는 실제 `Negative`를 올바르게 예측한 개수를 직접 반영하지 않습니다. 따라서 `F1-score`만으로 모델의 모든 분류 성능을 판단하기보다는 평가 목적에 맞게 다른 지표들과 함께 살펴볼 필요가 있습니다.

#### **Specificity (특이도)**

`Specificity`는 실제로 `Negative`인 인스턴스 중 분류기가 `Negative`라고 올바르게 예측한 인스턴스의 비율을 의미합니다.

$$
\text{Specificity} = \frac{TN}{FP+TN}
$$

여기서 분모는 실제 `Negative`인 인스턴스의 전체 개수이고, 분자인 `TN`은 그중 올바르게 `Negative`로 예측한 인스턴스의 개수입니다.

따라서 `Specificity`는 다음과 같이 해석할 수 있습니다.

$$
\text{Specificity} = \frac{\text{올바르게 Negative로 예측한 인스턴스 수}}{\text{실제로 Negative인 인스턴스 수}}
$$

그런데 `Specificity`의 수식을 잘 살펴보면 앞서 설명한 `FPR`과의 관계를 발견할 수 있습니다.

$$
\begin{aligned}
\text{Specificity}
&= \frac{TN}{FP+TN} \\[6pt]
&= 1-\frac{FP}{FP+TN} \\[6pt]
&= 1-\text{FPR}
\end{aligned}
$$

즉, `Specificity`와 `FPR`은 실제 `Negative`인 인스턴스를 기준으로 각각 올바르게 예측한 비율과 잘못 예측한 비율을 나타냅니다.

> **따라서 `Specificity`는 실제 `Negative`인 인스턴스를 얼마나 올바르게 구분하는지를 나타내며, `FPR`과는 서로 보완적인 관계를 가집니다.**

#### **성능 평가 지표 정리**

지금까지 `Confusion Matrix`를 바탕으로 계산할 수 있는 여러 성능 평가 지표를 살펴봤습니다.

각 지표에서 가장 중요한 부분은 **어떤 인스턴스 집합을 기준으로 비율을 계산하는가**입니다.

- `Accuracy`: 전체 인스턴스를 기준으로 올바르게 분류한 비율
- `Precision`: Positive로 예측한 인스턴스를 기준으로 올바르게 예측한 비율
- `Recall(TPR)`: 실제 Positive인 인스턴스를 기준으로 올바르게 Positive로 예측한 비율
- `FPR`: 실제 Negative인 인스턴스를 기준으로 잘못 Positive로 예측한 비율
- `F1-score`: Precision과 Recall의 조화평균
- `Specificity`: 실제 Negative인 인스턴스를 기준으로 올바르게 Negative로 예측한 비율

각 지표는 서로 다른 관점에서 분류기의 성능을 나타냅니다.

따라서 단순히 수식의 값이 높거나 낮다는 사실만 확인하기보다는, 어떤 인스턴스를 기준으로 어떠한 분류 결과를 측정하는 지표인지 이해하는 것이 중요합니다.

## **Confusion Matrix 계산 예시**

지금까지 `Confusion Matrix`를 이용하여 계산할 수 있는 다양한 성능 평가 지표들을 살펴봤습니다. 이번에는 실제로 숫자를 대입해 보면서 각 지표를 계산하고, 그 결과가 무엇을 의미하는지 살펴보겠습니다. 앞서 사용했던 `강아지`를 `Target Class`로 지정한 이진 분류기를 예시로 들어보겠습니다.

총 100장의 이미지로 구성된 데이터셋이 있으며, 실제 클래스(`True Class`)는 다음과 같다고 가정하겠습니다.

- `Positive`: 강아지 사진 20장
- `Negative`: 강아지가 아닌 사진 80장

이 데이터셋을 분류기에 입력했더니 다음과 같은 결과가 나왔습니다.

- `TP = 12`: 강아지 사진을 강아지로 올바르게 예측한 경우
- `FP = 4`: 강아지가 아닌 사진을 강아지로 잘못 예측한 경우
- `FN = 8`: 강아지 사진을 강아지가 아니라고 잘못 예측한 경우
- `TN = 76`: 강아지가 아닌 사진을 강아지가 아니라고 올바르게 예측한 경우

이를 `Confusion Matrix`로 나타내면 다음과 같습니다.

| Hypothesized Class / True Class | Positive | Negative |
|:---|---:|---:|
| **Positive** | TP = 12 | FP = 4 |
| **Negative** | FN = 8 | TN = 76 |

이제 이 값을 이용하여 앞서 살펴본 성능 평가 지표들을 계산해 보겠습니다.

### **Accuracy**

$$
\begin{aligned}
\text{Accuracy}
&= \frac{TP+TN}{P+N} \\
&= \frac{12+76}{20+80} \\
&= 0.88
\end{aligned}
$$

`Accuracy`는 **88%**입니다.

즉, 전체 100장의 이미지 중 88장을 올바르게 분류했다는 의미입니다.

그런데 실제 강아지 사진 20장 중 올바르게 분류한 사진은 12장에 불과합니다.

> **따라서 전체적인 `Accuracy`는 88%로 높아 보이지만, 강아지라는 `Target Class`에 대한 분류 성능도 그만큼 높다고 판단하기는 어렵습니다.**

### **Precision**

$$
\begin{aligned}
\text{Precision}
&= \frac{TP}{TP+FP} \\
&= \frac{12}{12+4} \\
&= 0.75
\end{aligned}
$$

`Precision`은 **75%**입니다.

즉, 분류기가 강아지라고 예측한 사진 16장 중 실제로 강아지인 사진은 12장이라는 의미입니다.

나머지 4장은 실제로 강아지가 아니지만 강아지라고 잘못 예측한 `FP`에 해당합니다.

### **Recall (TPR)**

$$
\begin{aligned}
\text{Recall}
&= \frac{TP}{TP+FN} \\
&= \frac{12}{12+8} \\
&= 0.6
\end{aligned}
$$

`Recall`은 **60%**입니다.

즉, 실제로 강아지인 사진 20장 중 분류기가 강아지라고 올바르게 예측한 사진은 12장이라는 의미입니다.

나머지 8장은 실제로 강아지이지만 분류기가 강아지가 아니라고 잘못 판단하여 놓친 `FN`에 해당합니다.

여기서 `Precision`과 `Recall`의 차이가 드러납니다.

> **`Precision`은 강아지라고 예측한 결과의 75%가 실제 강아지라는 의미이고, `Recall`은 실제 강아지 사진 중 60%만 찾아냈다는 의미입니다.**

### **False Positive Rate (FPR)**

$$
\begin{aligned}
\text{FPR}
&= \frac{FP}{FP+TN} \\
&= \frac{4}{4+76} \\
&= 0.05
\end{aligned}
$$

`FPR`은 **5%**입니다.

즉, 실제로 강아지가 아닌 사진 80장 중 4장을 강아지라고 잘못 예측했다는 의미입니다.

### **Specificity**

$$
\begin{aligned}
\text{Specificity}
&= \frac{TN}{FP+TN} \\
&= \frac{76}{4+76} \\
&= 0.95
\end{aligned}
$$

`Specificity`는 **95%**입니다.

즉, 실제로 강아지가 아닌 사진 80장 중 76장을 올바르게 강아지가 아니라고 예측했다는 의미입니다.

앞서 설명한 것처럼 `Specificity`와 `FPR`은 서로 보완적인 관계를 가집니다.

$$
\text{Specificity} + \text{FPR} = 0.95 + 0.05 = 1
$$

### **F1-score**

$$
\begin{aligned}
\text{F1-score}
&= \frac{2TP}{2TP+FP+FN} \\
&= \frac{2\times12}{2\times12+4+8} \\
&= \frac{24}{36} \\
&\approx 0.667
\end{aligned}
$$

`F1-score`는 약 **66.7%**입니다.

앞서 계산한 `Precision = 75%`, `Recall = 60%`의 조화평균이며, 두 지표의 산술평균인 67.5%보다 낮게 계산되었습니다.

이는 `F1-score`가 `Precision`과 `Recall`을 함께 고려하는 지표이기 때문입니다.

### **계산 결과 정리**

| 성능 지표 | 계산 결과 |
|---|---:|
| Accuracy | 88% |
| Precision | 75% |
| Recall (TPR) | 60% |
| FPR | 5% |
| Specificity | 95% |
| F1-score | 약 66.7% |

이 결과를 살펴보면 분류기의 `Accuracy`는 88%이지만, 실제 강아지 사진을 얼마나 찾아냈는지를 나타내는 `Recall`은 60%에 불과합니다.

반면 실제로 강아지가 아닌 사진을 올바르게 구분하는 `Specificity`는 95%로 상대적으로 높게 나타났습니다.

> **즉, 동일한 분류기와 동일한 데이터셋에서 계산했더라도 어떤 성능 지표를 사용하는지에 따라 모델의 성능에 대한 해석이 달라질 수 있습니다.**

따라서 모델의 성능을 평가할 때는 단순히 특정 지표가 높다는 사실만으로 판단하기보다는, 우리가 해결하려는 문제에서 어떤 분류 결과가 중요한지 고려하여 적절한 성능 지표를 선택해야 합니다.

## **마치며**
이번 포스트에서는 전반적인 `Confusion Matrix`의 개념과 이를 이용한 성능 평가 지표를 다뤘습니다. 

`Confusion Matrix`는 분류 모델의 예측 결과를 정리하는 행렬이며, `Image Classification`과 `Object Detection` 등의 성능 평가 개념을 이해하는 데 도움이 됩니다. 성능 지표의 수식을 계산하는 것은 비교적 간단하지만, 그 의미를 정확히 이해하는 것은 별개의 문제입니다. 저도 `Positive`, `Negative`의 의미를 명확하게 구분하지 못해 오랫동안 헷갈렸습니다.

그래서 이번 포스트에서는 `Confusion Matrix` 자체만 설명하는 것이 아니라, 그 기반이 되는 `True Class`와 `Hypothesized Class`에서의 `Positive`, `Negative`부터 설명드렸습니다.
이 포스트를 읽은 뒤에는 `Confusion Matrix`를 기반으로 한 성능 평가 지표들의 의미를 조금 더 명확하게 이해하실 수 있기를 바랍니다.