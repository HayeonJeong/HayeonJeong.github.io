---
title: "[Privacy Manager 연구] 실험 중 오류 수정"
date: 2024-08-07 14:00:00 +09:00
---

# 실험 목적
개인 정보 동의서를 넣으면 키워드 중심으로 요약해주는 sLLM 학습.
일정 수준 성능 달성 시 사전에 명시한 개인정보 공개 범위를 기준으로 알아서 판단하는 sLLM 학습이 궁극적인 목표.

# 실험 과정
## 1. sLLM 모델 선정
- Llama 7B를 사용하려다가...
- huggingface sLLM leaderboard에 tinyllama가 있길래
- TinyLlama-chat 모델로 선정 (그냥 TinyLlama 모델은 text generation task)

## 2. GPT2 --(knowledge distillation)--> TinyLlama-chat
- GPT-4o랑 같이 코드 짜고 돌렸는데 **'텐서 크기 불일치' 오류** 발생
    - KLDivLoss를 계산할 때 두 로짓의 크기가 불일치.
    - 이는 두 모델의 vocabulary 크기가 다르기 때문에 발생.
    - GPT-2의 vocabulary 크기는 50257이고, TinyLlama 모델의 vocabulary 크기는 32000.
    - 이를 해결하기 위해서는 teacher 모델의 출력을 student 모델의 vocabulary 크기에 맞게 변환해야.
    - 아래는 해결한 코드
    ```python
    def distillation_loss(student_logits, teacher_logits, temperature=2.0):
        student_log_probs = log_softmax(student_logits / temperature, dim=-1)
        
        # teacher logits를 student vocabulary 크기로 변환
        teacher_probs = softmax(teacher_logits / temperature, dim=-1)
        
        # 필요에 따라 teacher_probs의 크기를 student_log_probs 크기에 맞게 패딩
        if teacher_probs.size(-1) < student_log_probs.size(-1):
            padding_size = student_log_probs.size(-1) - teacher_probs.size(-1)
            teacher_probs = torch.nn.functional.pad(teacher_probs, (0, padding_size), value=0)
        elif teacher_probs.size(-1) > student_log_probs.size(-1):
            teacher_probs = teacher_probs[..., :student_log_probs.size(-1)]

        loss = torch.nn.KLDivLoss(reduction='batchmean')(student_log_probs, teacher_probs)
        return loss
    ```