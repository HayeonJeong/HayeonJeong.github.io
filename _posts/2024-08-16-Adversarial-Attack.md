---
title: "[AAA] About Adversarial Attack (논문 간단 정리: Explaining and Harnessing Adversarial Examples, ICLR, 2018)"
date: 2024-08-16 21:31:00 +09:00
---
## 1. What is Adversarial Attack?

<img width="637" alt="image" src="https://github.com/user-attachments/assets/8e24b7a2-cd1e-467b-9051-322c0203c4b5">

## 2. Adversarial Example
- 의미: Adversarial Attack의 결과로 생성된 것.
- 처음 제시된 논문: Explaining and Harnessing Adversarial Examples (ICLR, 2018)

- Adversarial Example 생성 과정

    <img width="724" alt="image" src="https://github.com/user-attachments/assets/b6eea67d-45e8-4884-b15c-89c690b4d510">

    - **\(\mathbf{x}\)**: 원본 이미지 (예: 판다)
    - **\(\mathbf{x}'\)**: Adversarial Example, 즉 노이즈가 추가된 이미지
    - **\(\epsilon\)**: 노이즈의 크기를 조절하는 하이퍼파라미터
    - **\(J(\theta, \mathbf{x}, y)\)**: 모델이 현재 파라미터 \(\theta\)를 가지고 입력 \(\mathbf{x}\)와 레이블 \(y\)에 대해 계산한 손실 값
    - **\(\nabla_\mathbf{x} J(\theta, \mathbf{x}, y)\)**: 이 손실 함수 \(J\)를 입력 이미지 \(\mathbf{x}\)에 대해 미분한 것, 즉 손실이 입력 이미지 \(\mathbf{x}\)의 작은 변동에 따라 어떻게 변화하는지를 나타냄.
    - **\(\text{sign}(\nabla_\mathbf{x} J(\theta, \mathbf{x}, y))\)**: 기울기의 부호로, 손실을 증가시키기 위한 픽셀 값의 변화 방향을 결정.
    - **\(\epsilon\)**: 이 부호 방향에 따라 추가할 노이즈의 크기를 조절.

## 3. Adversarial Training

### 1) What is Adversarial Training?

<img width="652" alt="image" src="https://github.com/user-attachments/assets/c3c89edd-e002-4d5d-a661-1a95ddf0d3a2">

### 2) 'PGD' Based Adversarial Training
- PGD (Projected Gradient Descent)
    - 하나의 입력 이미지에 대해 적대적 예제를 생성하는 과정
    - 주어진 이미지에 대해 반복적으로 적대적 노이즈를 추가하여 모델의 예측을 변경하는 데 사용

- PGD 수식 설명
    
    <img width="100" alt="image" src="https://github.com/user-attachments/assets/068fefb3-03de-41c5-84ca-b80ef6538ffd">
    
    : adversarial example이 original class로 분류되도록 학습됨.

    <img width="500" alt="image" src="https://github.com/user-attachments/assets/4f8a7720-7866-441d-b02b-545b0bc20fad">

    : perturbation이 포함된 'x + perturbation'과 original image 사이의 손실을 최대화 하는 방향으로 만드는 adversarial example.

    <img width="400" alt="image" src="https://github.com/user-attachments/assets/82b3911c-c942-4202-a254-898aa4375701">

    : epsilon 거리 안에서 적대적 예제를 생성한다는 의미. (업데이트 된 방향에서 projection 한다는 의미)

    - **\(\mathbf{x}_t\)**: 현재 단계 \(t\)에서의 이미지
    - **\(\mathbf{x}_{t+1}\)**: 다음 단계에서의 업데이트된 이미지
    - **기울기 계산**: \(\nabla_\mathbf{x} J(\theta, \mathbf{x}_t, y)\)는 현재 이미지 \(\mathbf{x}_t\)에 대한 손실 함수의 기울기
    - **노이즈 추가**: \(\alpha \cdot \text{sign}(\nabla_\mathbf{x} J(\theta, \mathbf{x}_t, y))\)로 현재 이미지에 노이즈를 추가
    - **Projection(영사)**: Projection 연산은 이미지가 \(\epsilon\) 거리 내에 있도록 보장함
        - L_∞ 노름을 사용하여 epsilon 거리를 정의하고, 이 거리 내에서 적대적 예제를 생성. (각 픽셀의 최대 변경량 제어)

## Reference
- https://www.youtube.com/watch?v=SePQlKQd5xY&t=1s