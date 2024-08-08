---
title: Knowledge Distillation 개념
date: 2024-08-06 14:00:00 +09:00
---

# Knowledge Distillation
: 딥러닝 모델의 지식 증류 기법
- 지식을 증류한다: 학습된 모델로부터 지식을 추출.

## 1. Knowledge Distillation가 등장한 이유
- 모델 배포할 때, 정확도 높은데 오래걸리는 모델보다 정확도 낮지만 엄청 빠른 모델이 배포 측면에서는 더 좋음.
- 그럼 복잡한 모델이 학습한 generalization 능력을 단순한 모델에 전달해주면 되지 않을까?
- teacher model -> student model

## 2. Knowledge Distillation은 언제 처음 나왔나?
- Distilling the Knowledge in a Neural Network, NIPS workshop, 2014

## 3-1. Knowledge Distillation은 어떻게? (w/ Hinton’s KD)
### 1) soft label
image classification 마지막 softmax 레이어 수식 (i번째 클래스에 대한 확률값):
$$
q_i = \frac{\exp(z_i)}{\sum_j \exp(z_j)}
$$

출력값의 분포를 더 soft하게 만들어 모델에 반영:
$$
q_i = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}
$$
- T = temperature
    - T가 높아지면: soft하게
    - T가 낮아지면: hard하게

Ex:
- original (hard) targets
    ![image](https://github.com/user-attachments/assets/d99d45df-3c5b-479e-a317-9dcf81be17cf)
- output (q)
![image](https://github.com/user-attachments/assets/2125dc5e-181d-4be2-8fdd-903b1233d815)
- soft output = dark knowledge
![image](https://github.com/user-attachments/assets/7507a593-5ac9-4147-ba64-6a01f7bc244a)

### 2) distillation loss
![image](https://github.com/user-attachments/assets/004ccfd9-8aa2-428d-be9e-4dad53818990)
$$
L = \sum_{(x,y) \in D} L_{KD}(S(x, \theta_S, \tau), T(x, \theta_T, \tau)) + \lambda L_{CE}(\hat{y}, y)
$$
- $L_{KD}$: Distillation Loss
    - 잘 학습된 Teacher model의 soft labels와 Student model의 soft predictions를 비교하여 손실함수를 구성
    - $\tau$: 동일하게 설정하고 Cross Entropy Loss를 사용
- $L_{CE}$: 기존 이미지 분류에서 사용하는 Cross Entropy Loss

### 3) Summary
1. Teacher Network 학습
2. Student Network 학습
    - Student Network soft prediction + Teacher Network soft label → distillation loss 구성
    - Student Network (hard) prediction + Original (hard) label → classification loss 구성

## 3-2. Knowledge Distillation은 어떻게? (GPT로 설명)
- GPT는 input seq이 주어졌을 때 바로 다음 단어로 올 단어들의 확률을 맞추는 방법으로 학습함.
    - 입력값: 토큰의 시퀀스
    - 예측할 출력값: 다음에 올 단어의 one-hot-vector
    ![image](https://github.com/user-attachments/assets/c88e6b35-e93c-438e-b3b1-ed336a1bfec6)
- 반면, teacher 모델의 출력
    ![image](https://github.com/user-attachments/assets/3d07e066-6fbb-4237-afa7-c540d0393b15)
    - 사전(vocab)에 가지고 있는 모든 단어들에 대해 등장 확률을 계산함. (원핫 벡터보다 더 많은 정보를 담고 있음)
- 그래서, student 모델이 one hot vector 대신에 teacher 모델의 출력값을 따라하도록 학습시키면, 더 작은 모델로도 teacher 모델과 대등한 성능을 낼 수 있게 되는 것.
    1. teacher 모델의 출력을 softmax에 넣어서 전체 사전에서 각 토큰의 등장 가능성으로 변환해야.
        ![image](https://github.com/user-attachments/assets/d4d90dd5-aba6-4548-ba14-b361e9b6a974)
    2. 사전에 있는 모든 단어에 대해 확률을 계산하기 때문에, 극단적인 값들이 등장 -> Temperature 지정으로 soften.
        ![image](https://github.com/user-attachments/assets/5676c636-5794-4d96-88a0-54e52f161f92)
- 전체 모델의 동작 구조
    ![image](https://github.com/user-attachments/assets/7e61b2d5-1457-451a-9555-2ba6e638ae1c)

## Reference
- https://baeseongsu.github.io/posts/knowledge-distillation/
- https://tilnote.io/pages/6480a73ee92fe5ef635f4d77