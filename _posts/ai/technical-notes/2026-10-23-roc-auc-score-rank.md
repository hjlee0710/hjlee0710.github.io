---
title: "[Technical Notes] ROC-AUC의 Score Rank 불변성"
categories:
- AI
- Technical Notes
img_path: "/assets/img/posts/ai/prompt-engineering/technical-notes/roc-auc-score-rank"
image:
  path:
---

## **ROC-AUC(Area Under the ROC Curve)란?**

`ROC-AUC(Area Under the ROC Curve)`는 모델이 Positive와 Negative 데이터를 얼마나 잘 구분하는지를 평가하는 지표입니다.





> Detection에서 Positive와 Negative의 의미
> : 이 개념을 다루기 전에 명확하게 구분해야하는 것이 있습니다. `Detection` 해야하는 대상, 모델이 추론한 결과, 실제 정답, 이렇게 총 3가지를 구분해야 합니다. 우리가 많이 보는 


Positive와 Negative는 관찰하고자 하는 대상이 있으면 Model이 관찰하고자 하는 대상을 관찰했으면 Positive, Model이 이외의 대상으로 예측을 했다면 Negative입니다. 예를 들어, 우리는 Dog를 관찰하고 싶습니다. 
{: .prompt-info}





기본적으로 이진 분류(Binary Classification)를 대상으로 정의되지만, 다중 라벨 분류(Multi-label Classification)에서도 각 라벨을 독립적인 이진 분류 문제로 간주하여 ROC-AUC를 계산할 수 있습니다.