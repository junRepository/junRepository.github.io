---
layout: post
title:  Anaconda 가상환경 다루기
date:   2022-12-10 13:50
image:  
tags:   conda
---

### 가상환경(Virtual Environments)이란
-프로젝트마다 필요한 모듈이 다른데 필요한 모듈을 하나로 묶는 통 같은 것이다.  
-프로그래밍 언어들은 패키지의 버전이 계속 업데이트가 되고 있다. 버전이 다를 수록 기능들이 새로 생기거나 사라진다.  그러다보면 각각의 프로젝트에 맞게 돌릴 수 있는 버전이 다른데 이것을 쉽게 다루기 위해 가상환경을 사용합니다.

<br/>

1. conda 가상환경 리스트 확인  
```bash
# 방법 1
conda env list
# 방법 2
conda info --envs
```
<br/>

2. conda 가상환경 삭제하기  
```bash
# 방법 1
conda remove --name 가상환경 이름 --all
# 방법 2
conda env remove -n 가상환경 이름
```
<br/>

3. conda 가상환경 만들기  
```bash
conda create -n 가상환경 이름
#python 버전 설치
conda create -n 가상환경 이름 python=3.7
```
<br/>

4. conda 가상환경 활성화 / 비활성화  
```bash
#활성화
conda activate 가상환경 이름
#비활성화
conda deactivate 가상환경 이름
```
<br/>


5. conda 가상환경 이름 변경  

```bash
#conda에서 이름 변경 기능은 없다
#그래서 복사 생성 후 삭제를 통해서 변경이 가능함

#기존 가상환경을 새로운 이름으로 복제
conda create --name 변경할 이름 --clone 기존환경 이름
#복제한 가상환경 활성화
conda activate 변경할 이름
#기존 가상환경 삭제 
conda remove --name 기존환경 이름 --all
```
<br/>


