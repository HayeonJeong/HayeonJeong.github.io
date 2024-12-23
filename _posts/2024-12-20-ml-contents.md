---
title: "[ML] 목차"
categories: ML
date: 2024-12-20 16:40:00 +09:00
---

인공지능대학원 면접에 대비해 공부한 내용을 정리했다.

질문들을 나열해서 공부했던 터라 뒤죽박죽인 개념들을 claude의 도움으로 카테고리화 했다.


1. **ML Core Concepts & Optimization**
    - overfitting(과적합)이란? 그것을 어떻게 방지하는지?
    - regularization, L1, L2 regression
    - regularization과 Normalization의 차이
    - cost function또는 loss function이란, cost 또는 loss란? 그리고 이것을 '최적화 한다'는 것은?
    - global optima, local optima with multiple model parameter에 대해
    - Gradient Descent란? 여기서 미분을 왜 쓰는지? 어떻게 미분을 쓰는지? '기울기'의 의미는?
    - Cross-Entropy Loss Function?
    - GD, SGD, SGD with minibatch란?
    - MLE (Maximum Likelihood Estimation)이란?

2. **Model Evaluation & Selection**
    - Cross Validation이란?
    - Grid Search란?
    - 다양한 파라미터 최적화 방법들
    - 계충에 따른 '최적화'의 개념
    - metric, measure와 confusion matrix(TP, TN FP, FN)
    - 차원의 저주(Curse of Dimensionality)란?
    - feature extraction vs feature selection이란?
    - 트레이드오프- 복잡도에 따른 편향-분산

3. **ML Algorithms & Methods**
    - 회귀 / 분류의 차이
    - logistic regression이란? (linear regression - logistic regression, softmax regression)
    - SVM(Support Vector Machine)의 기본 개념과 장점은 무엇인가?
    - Decision Tree
    - KNN
    - ensemble? bagging vs boosting? random forest를 이용한 classification & regression
    - (Multiple) Linear Regression
    - 'similarity'란?

4. **Deep Learning & Advanced Topics**
    - 지도/비지도/반지도학습이란?
    - 머신러닝 딥러닝 차이
    - 머신러닝에서의 '학습'과정에 대한 전반적인 내용
    - +불안정한 학습
    - 머신러닝에서 얘기하는 '모델'이란? (함수 그룹 생각)
    - transductive(직도적) learning & inductive(귀납적) learning?