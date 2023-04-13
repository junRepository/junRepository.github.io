---
layout: post
title:  Feature Engineering과 ML Pipeline
date:   2023-04-13 00:00
image:  
tags:   Feature_Store
---

# Feature Engineering란?
여러 데이터들을 가공하여 만들어진 Feature들을 JOIN등을 거쳐 학습에 필요한 데이터로 만드는 것<br/>
Feature는 관심 현상에 대해 개별적으로 측정 가능한 속성 또는 특징으로, 머신러닝 모델의 입력값이다.

<br/>

# ML Pipeline
ML Pipeline 자동화-DevOps에서의 Code변경처럼 데이터 변경시에 자동화된 단계를 하는 것을 말한다.
**Enterprise Data** -> **Feature Engineering** -> **Feature Store** -> **Model Training** -> **Model Testing** -> **Model Serving** -> **Model Monitoring** 의 단계를 자동화하는 것

***
###### DevOps - 아래의 개발 과정을 자동화하는 것을 말한다
1. 개발자가 모델을 만들고 
2. Jenkins등을 활용하여 Checkout, Compile, Test, Package, Deploy등의 과정으로 테스트하고
3. 서버에 배포하고 운영하는 것  

***

# Feature Store
실시간성 batch성 혼합형 데이터 원천에 대해서 Feature Pipeline을 정의해놓고 데이터가 들어오면 자동적으로 Feature를 생성해 내어서 짧은 주기로 ML모델에 Feature를 제공할 수 있게 되고, 최신의 데이터를 반영한 모델링으로 품질 높은 예측 결과를 제공가능

##### Feature Store를 사용할 때의 장점

1. 필요한 모든 데이터로부터 feature를 추출하고 제공한다.
2. 데이터 사이언티스트가 직접 Feature Store를 통해 운영에 필요한 Feature들을 배포하게 된다.
3. Feature들을 데이터 자산의 하나로 관리 가능해진다.
4. 급격한 데이터 분포 변화를 막고, 데이터 품질 신뢰를 얻을 수 있다. 