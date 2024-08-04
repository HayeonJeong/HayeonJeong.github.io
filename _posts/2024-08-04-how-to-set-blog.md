---
title: How did I set up the blog
date: 2024-08-04 19:16:00 +09:00
---
## Chirpy로 만들기
https://devpro.kr/posts/Github-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-(3)/
- fork하는 방법은 오류 발생.
- 직접 다운 및 옮기기

## local에서는 되는데 배포 안되는 오류 해결
https://velog.io/@hashnsalt/Github-Blog-%EB%A7%8C%EB%93%A4%EA%B8%B0-2

## workflow scope 오류 해결
https://tigris-data-science.tistory.com/entry/workflow-scope-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0

- 새로운 토큰을 Git 설정에 추가:
```C++
git remote set-url origin https://<NEW_TOKEN>@github.com/HayeonJeong/hayeonjeong.github.io.git

- 그리고 commit, push
