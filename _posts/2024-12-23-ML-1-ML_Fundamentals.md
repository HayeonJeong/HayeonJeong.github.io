---
title: "[ML, kor] ML Fundamentals (기계학습 기초)"
categories: ML
date: 2024-12-23 17:40:00 +09:00
---
```Python
[1] ML Core Concepts & Optimization
    1. overfitting(과적합)이란? 그것을 어떻게 방지하는지?
    2. regularization, L1, L2 regression
    3. regularization과 Normalization의 차이
    4. cost function또는 loss function이란, cost 또는 loss란? 그리고 이것을 '최적화 한다'는 것은?
    5. global optima, local optima with multiple model parameter에 대해
    6. Gradient Descent란? 여기서 미분을 왜 쓰는지? 어떻게 미분을 쓰는지? '기울기'의 의미는?
    7. Cross-Entropy Loss Function?
    8. GD, SGD, SGD with minibatch란?
    9. MLE (Maximum Likelihood Estimation)이란?
```

# 1. overfitting(과적합)이란? 그것을 어떻게 방지하는지?

## 1) 문제 정의 (매/특/다/계)

과적합(Overfitting)은 모델이 훈련 데이터에 너무 잘 맞춰져서 새로운 데이터에 대한 일반화 능력이 떨어지는 문제이다. 이는 모델의 매개변수가 너무 많거나, 특징 공간(Feature Space)의 차원이 높거나, 다항식 차수가 너무 높거나, 계수 값이 극단적으로 큰 경우에 발생할 수 있다.

과적합된 모델은 “훈련 데이터의 노이즈까지 학습하여” 훈련 세트에서는 오차가 낮지만 검증/테스트 세트에서는 오차가 높게 나타난다.


## 2) 해당 문제에 대한 일반적인 접근법 (엘/드/엌/스탑)

과적합 문제를 해결하기 위한 일반적인 접근법은 “**정규화(Regularization)**” 기법들이 있다.

1. Lp norm은 손실 함수에 가중치의 p-norm을 추가하여 가중치를 제한하는 기법이다 ($L + \lambda ||\mathbf{w}||_p^p$, $p \in \{1, 2\}$) 가중치를 고루 작게 만들 수 있는 l2 정규화 방법이 주로 사용된다.
    
    → 모든 모델에서 사용 가능
    
2. 드롭아웃(Dropout)은 학습 시에 무작위로 드롭 확률(p)에 따라 뉴런의 출력을 0으로 만들어 모델의 일반화 성능을 높이는 기법이다. (단, 테스트 시에는 모든 뉴런을 활성화한다.)
    
    → 파라미터(뉴런)가 많을 때 사용 / 파라미터가 적으면 정보 손실이 너무 크기 때문
    
3. 데이터 증강(Data Augmentation)은 학습 데이터에 변형을 가하여 다양성을 증가시키는 방법이다. 이미지 데이터의 경우 렌즈 왜곡, 이동, 전단(Shear), 확대/축소, 회전 등의 변형을 적용할 수 있다.
4. 조기 종료(Early Stopping)는 학습 시 검증 오차를 관찰하다가 증가하기 시작하면 학습을 중단하는 기법이다.

## 3) 일반적인 접근법의 제한 사항 (하/트, 하-정/드/학)

정규화 기법들을 적용할 때 몇 가지 제한 사항이 있다.

- 첫째, 하이퍼파라미터 선택의 어려움이 있다. 정규화 강도($\lambda$), 드롭아웃 비율, 학습률 등 최적의 하이퍼파라미터를 찾는 것이 쉽지 않다.
- 둘째, 편향-분산 트레이드오프 문제가 있다. 모델 복잡도를 줄이면 편향(Bias)은 증가하고 분산(Variance)은 감소하는 반면, 모델 복잡도를 늘리면 편향은 감소하고 분산은 증가한다. 이 둘 사이의 최적 지점을 찾는 것이 어렵다.

이러한 제한 사항들로 인해 과적합 문제를 완전히 해결하기는 쉽지 않다.

|  | 편향(부정확도) | 분산(불안정성) |
| --- | --- | --- |
| **복잡도 ↑** | **↓** | **↑** |
| 복잡도 ↓ | **↑** | **↓** |

## 4) 제한 사항에 대한 해결 방안 (교/단/곡)

제한 사항을 극복하기 위해 여러 가지 해결 방안을 사용할 수 있다.

- 첫째, 교차 검증(Cross Validation) 전략을 사용하여 **여러 하이퍼파라미터 조합을 시도**하고 최적의 조합을 찾는다.
- 둘째, 단계적 탐색(Coarse to fine in stages)을 통해 처음에는 **대략적인 범위**에서 짧은 시간 동안 학습을 수행하여 **유망한** 하이퍼파라미터를 찾고, **점진적으로 더 정밀한 탐색**을 수행한다.
- 셋째, 학습 중 학습 곡선(손실 곡선)을 지속적으로 모니터링하여 과적합이나 과소적합 징후를 조기에 발견한다. 이러한 방법들을 통해 과적합 문제를 더 효과적으로 해결할 수 있다.

# 2. regularization, L1/L2 regression

- Regularization (규제화): 과적합 방지를 위한 Cost 함수에 패널티 추가
---

1. L1 Regularization (회귀 문제에 적용 시, L1 Regression = Lasso Regularization)
    - $Cost = Loss + \lambda \Sigma |w|$
    - 일부 가중치를 0으로 만들어줌 → 불필요한 특성 제거
2. L2 Regularization (회귀 문제에 적용 시, L2 Regression = Ridge Regularization)
    - $Cost = Loss + \lambda \Sigma w^2$
    - 모든 가중치를 작게 제한 → 전반적인 복잡도 감소

---

* $\lambda$로 규제 강도 조절