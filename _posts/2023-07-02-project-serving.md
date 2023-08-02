<!-- ---
layout: post
title:  "[프로젝트]기업 건전성 예측 모델 서빙"
date:   2023-07-02 00:00
image:  
tags:   project
---


*** 

### FastAPI와 Docker를 이용하여 Predict하는 모델 만들기 
<img src="/images/projectserving.jpg" width="100%"> 
- 프로젝트의 구성도는 위와 같다.
- FastAPI를 통해 Pytorh 모델을 API식으로 빌드하고 Docker를 활용하여 배포하여 web page에서 사용자가 입력하는 데이터를 전달받아 에측을 하고 예측값을 web page에 전달하여 사용자들이 쉽게 건전 / 비건전을 확인할 수 있게 하였다.


### 1. Pytorch 모델 API 빌드

torch.save를 이용하여 .pth형태로 저장하고  -->