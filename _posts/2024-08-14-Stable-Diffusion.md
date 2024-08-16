---
title: "Stable Diffusion에 대해 몰랐던 사실들"
date: 2024-08-16 21:31:00 +09:00
---

최근에 논문 스터디를 하며 Stable Diffusion을 다시 짚어보다가 몰랐던 것을 하나 알게 되었다.

## Stable Dffusion Architecture
![image](https://github.com/user-attachments/assets/7e06a6ef-d31e-4ca0-aca8-ea3aa2ba7be4)

### 1) switch
- 최근에 Uni-ControlNet을 공부해서 합리적 추론으로 인코더 디코더의 해상도 별로 text condition을 넣을 건지 안넣을 건지 결정하는 건줄 알았다.
- 근데 timestep에 따라 u-net 전체에 주는 text condition의 weight를 정하는 거였음.

### 2) T (Timestep)
- 몰랐던건 아니지만... 확실히 하기
- hypter-parameter로, 얼만큼의 step을 걸쳐 denoising할 건지 결정하는 요소임.
- denoising하는 u-net 구조는 하나임 t에 따라 달라지지 않음.