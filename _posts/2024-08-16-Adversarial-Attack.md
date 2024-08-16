---
title: "[AAA] About Adversarial Attack (논문 별 공격 기법 정리)"
date: 2024-08-16 21:31:00 +09:00
---
## 1. What is Adversarial Attack?

<img width="637" alt="image" src="https://github.com/user-attachments/assets/8e24b7a2-cd1e-467b-9051-322c0203c4b5">

## 2. Adversarial Example
- 의미: Adversarial Attack의 결과로 생성된 것.
- 처음 제시된 논문: Explaining and Harnessing Adversarial Examples (ICLR, 2015)

- Adversarial Example 생성 과정; **FGSM(Fast Gradient Sign Method)**
    : 입력 𝑥에 대해 손실 함수 𝐿(𝜃,𝑥,𝑦)의 그래디언트를 사용하여, 모델이 가장 크게 오분류를 유발할 수 있는 방향으로 한 번에 작은 변화를 줌.

    <img width="724" alt="image" src="https://github.com/user-attachments/assets/b6eea67d-45e8-4884-b15c-89c690b4d510">

    - x: 원본 이미지 (예: 판다)
    - x′: Adversarial Example, 즉 노이즈가 추가된 이미지
    - ϵ: 노이즈의 크기를 조절하는 하이퍼파라미터
    - J(θ,x,y): 모델이 현재 파라미터 θ를 가지고 입력 x와 레이블 y에 대해 계산한 손실 값
    - ∇xJ(θ,x,y): 이 손실 함수 J를 입력 이미지 x에 대해 미분한 것, 즉 손실이 입력 이미지 x의 작은 변동에 따라 어떻게 변화하는지를 나타냄.
    - sign(∇xJ(θ,x,y)): 기울기의 부호로, 손실을 증가시키기 위한 픽셀 값의 변화 방향을 결정.

## 3. Adversarial Training

### 1) What is Adversarial Training?

<img width="652" alt="image" src="https://github.com/user-attachments/assets/c3c89edd-e002-4d5d-a661-1a95ddf0d3a2">

### 2) 'PGD' Based Adversarial Training
- 논문: Towards Evaluating the Robustness of Neural Networks (ICLR, 2018)
- PGD (Projected Gradient Descent)
    - 하나의 입력 이미지에 대해 적대적 예제를 생성하는 과정
    - 주어진 이미지에 대해 반복적으로 적대적 노이즈를 추가하여 모델의 예측을 변경하는 데 사용

- PGD 수식 설명
    
    <img width="100" alt="image" src="https://github.com/user-attachments/assets/068fefb3-03de-41c5-84ca-b80ef6538ffd">
    
    : adversarial example이 original class로 분류되도록 학습됨.

    <img width="500" alt="image" src="https://github.com/user-attachments/assets/4f8a7720-7866-441d-b02b-545b0bc20fad">

    : perturbation이 포함된 'x + perturbation'과 original image 사이의 손실을 최대화 하는 방향으로 만드는 adversarial example.

    <img width="400" alt="image" src="https://github.com/user-attachments/assets/82b3911c-c942-4202-a254-898aa4375701">

    : 업데이트 된 방향이 주어진 범위 S 내에서 유지되도록 projection 함 (epsilon 거리 안에서 적대적 예제를 생성함)

    - **\(\mathbf{x}_t\)**: 현재 단계 \(t\)에서의 이미지
    - **\(\mathbf{x}_{t+1}\)**: 다음 단계에서의 업데이트된 이미지
    - **기울기 계산**: \(\nabla_\mathbf{x} J(\theta, \mathbf{x}_t, y)\)는 현재 이미지 \(\mathbf{x}_t\)에 대한 손실 함수의 기울기
    - **노이즈 추가**: \(\alpha \cdot \text{sign}(\nabla_\mathbf{x} J(\theta, \mathbf{x}_t, y))\)로 현재 이미지에 노이즈를 추가
    - **Projection(영사)**: Projection 연산은 이미지가 \(\epsilon\) 거리 내에 있도록 보장함
        - L_∞ 노름을 사용하여 epsilon 거리를 정의하고, 이 거리 내에서 적대적 예제를 생성. (각 픽셀의 최대 변경량 제어)

## 4. Other Adversarial Attack Methods

### 1) L-BFGS (Limited-memory Broyden-Fletcher-Goldfarb-Shanno)
- 제안된 논문: Intriguing properties of neural networks. (ICLR, 2014)

- 모델이 목표 클래스로 잘못 분류하도록 만드는 최소한의 변화를 찾음.
    - 주로 targeted attack에 사용.
- 입력 데이터를 조정할 때, 최적화 알고리즘(즉, BFGS)을 사용해 공격을 수행.
---
<img width="378" alt="image" src="https://github.com/user-attachments/assets/b70fac24-4751-4464-bebb-ac72a30a8237">

- δ: 입력 이미지 𝑥에 추가할 변동(perturbation).
- ∥δ∥_2: ℓ2 노름을 최소화하여 가능한 작은 변동을 찾음.
- 𝑓(𝑥+𝛿): 모델이 변형된 입력 𝑥+𝛿를 예측한 결과.
- 𝑦_target: 목표(target) 레이블.


### 2) DeepFool
- 제안된 논문: DeepFool: A Simple and Accurate Method to Fool Deep Neural Networks (CVPR, 2016)
- DeepFool은 모델의 결정 경계를 넘는 최소한의 변화(perturbation)를 찾아서 입력 이미지를 변형하는 공격 방법
- 비선형 분류기를 점진적으로 선형 근사해서, 가장 가까운 결정 경계를 넘도록 입력 데이터를 변형함
---
<img width="220" alt="image" src="https://github.com/user-attachments/assets/882e7770-6a73-4d0e-89df-dd6b62480d27">

- δ: 입력 이미지 𝑥에 추가할 변동.
- 𝑓(𝑥): 모델의 예측 함수.
- ∇𝑓(𝑥): 모델의 예측 함수에 대한 그래디언트.
∥∇𝑓(𝑥)∥_2: 그래디언트의 ℓ2 노름.

### 3) Logit-space Projected Gradient Ascent (LS-PGA)
- 제안된 논문: Thermometer encoding: One hot way to resist adversarial examples. (ICLR, 2018)
- 모델의 최종 로짓(logit) 값(소프트맥스 함수 이전의 값)을 기반으로 공격을 수행
- PGD와 유사하지만, 입력 데이터를 수정할 때 손실 함수 대신 로짓 값의 변화에 집중함.
- 특정 클래스의 확률을 높이는 데 효과적임.

---
<img width="342" alt="image" src="https://github.com/user-attachments/assets/00bbd7c8-3147-446c-a4c6-b30147289c29">

<img width="250" alt="image" src="https://github.com/user-attachments/assets/da39742d-7ce4-441f-b049-763e59386832">

## Reference
- https://www.youtube.com/watch?v=SePQlKQd5xY&t=1s