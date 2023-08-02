---
layout: post
title:  "Anaconda GPU 설치"
date:   2023-08-02 00:00
image:  
tags:   conda
---

## 목차
* NVIDIA 설치
* Anaconda 설치
* CUDA 설치 & cuDNN 설치
* 환경 변수 설정
* Pytorch & Tensorflow 설치
<br/>

***

## 01. NVIDIA 설치
NVIDIA 드라이버 최신버전으로 설치하거나 기존 설치가 되어있으면 업데이트를 한다.<br/>
[NVIDIA 드라이버 다운로드](https://www.nvidia.co.kr/Download/index.aspx?lang=kr)<br/>
<img src="/images/NVIDIA.jpg" width="60%">

***

## 02. Anaconda 설치
[Anaconda](https://www.anaconda.com/download)에서 Download를 클릭하여 설치를 진행한다.<br/>
<img src="/images/anaconda.jpg" width="60%">

***

## 03. CUDA & cuDNN 설치sklearn conda install
#### 03-1 CUDA, cuDNN 지원 확인
CUDA를 설치하기 전에 [CUDA | Wikiwand](https://www.wikiwand.com/en/CUDA#/GPUs_supported)에서 GPU에 지원되는 CUDA 버전을 찾는다.<br/>
GTX1080Ti를 사용하기 때문에 6.1을 지원하는 CUDA버전을 찾는다.<br/>
<img src="/images/cuda1.jpg" width="100%"><br/>
녹색 박스 중 6.1을 지원하는 CUDA 버전이 CUDA 8.0 이상부터 지원해준다.<br/> 
작성자 같은 경우는 CUDA 11.2를 설치하였다.<br/>
<img src="/images/cuda2.jpg" width="80%"><br/>
[TensorFlow](https://www.tensorflow.org/install/source_windows?hl=ko#tested_build_configurations)에서 GPU 부분을 보면 CUDA 버전과 Python 버전에 맞는 cuDNN 버전이 나와있다.<br/>
\<python, cuDNN, CUDA 버전에 맞는 tensorflow-gpu 버전을 잘 기억해두자 tensorflow 설치할 때 참고해야 한다. \><br/>
작성자 같은 경우는 cuDNN 8.1을 설치하였다.
<img src="/images/tensorflow.jpg" width="80%"><br/>

#### 03-2 CUDA, cuDNN 설치
위에서 CUDA와 cuDNN 버전을 찾았다면 해당 버전을 찾아 설치한다.<br/>
[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)에서 CUDA버전을 찾아 설치한다.<br/>
<img src="/images/cudainstall.jpg" width="80%"><br/>
cmd 창에서 `nvcc -V`를 입력하여 설치가 잘 되었는지 확인한다. <br/>

[cuDNN Archive](https://developer.nvidia.com/rdp/cudnn-archive)에서 cuDNN 버전을 찾아 설치한다. **(다운하기 위해 로그인을 해야한다.)**<br/>
<img src="/images/cudnninstall.jpg" width="80%"><br/>
cuDNN을 설치를 하면 압축을 풀고 `bin`, `include`, `lib` 폴더가 있는 데 이것들을 복사를 한 후 CUDA가 설치되어 있는 경로에 붙어넣기를 한다.<br/>
작성자는 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\ 에 CUDA가 설치가 되었다.<br/>

***

## 04. 환경 변수 철정
1. 윈도우 키를 누른 후 `시스템 환경 변수 편집`를 입력하여 들어간다.
2. 우측 하단에 환경 변수를 클릭한다.<br/>
    <img src="/images/setting1.jpg" width="50%"><br/>
3. 사용자 변수에서 Path를 더블클릭을 하여 환경 변수 편집에 들어가 새로 만들기를 누른다.<br/>
    <img src="/images/setting3.jpg" width="50%"><br/>
4. 그리고 좀 전에 설치한 CUDA의 bin, include, lib 경로를 새로 만든다.<br/>
    <img src="/images/setting2.jpg" width="50%"><br/>
5. 확인을 누르고 재부팅을 한다. | 재부팅을 안 하면 환경변수 적용이 안 된다.

***

## 05. Pytorch & Tensorflow 설치
1) Pytorch와 Tensorflow를 설치하기 전에 `Anaconda Prompt`에서 romanaconda 환경 업데이트를 한다.
```bash
conda update -n base conda
```
2) 패키지 업데이트
```bash
conda update --all
```
3) 패키지 관리 시스템인 pip 업데이트
```bash
python -m pip install --upgrade pip
```
<br/>

#### 05-1 Pytorch
[Pytorch](https://pytorch.kr/get-started/locally/)공식 사이트에서 Pytorch를 설치한다.<br/>
밑에 conda~~로 나와있는 코드를 anaconda prompt에 입력하여 설치를 진행한다.<br/>
작성자는 CUDA 11.2 버전이 없어서 11.7 버전으로 설치를 하였다.<br/>
<img src="/images/pytorch.jpg" width="80%"><br/>

#### 05-2 Tensorflow
cuDNN버전을 찾을때 보았던 tensorflow-gpu 버전을 설치를 한다.<br/>
<img src="/images/tensorflow.jpg" width="70%"><br/>
```bash
pip install tensorflow-gpu==2.10.0
```

***

## 06. GPU 확인
설치가 잘 되었는지 확인한다.<br/>
Anaconda Prompt를 실행하여 아래 코드를 입력한다.<br/>
#### Tensorflow
```bash
python
>>> from tensorflow.python.client import device_lib
>>> print(device_lib.list_local_devices())
```
아래 `physical_device_desc:`에 자신의 그래픽카드가 뜨면 Tensorflow 설치는 성공이다.<br/>
#### Pytorch
```bash
python
>>> import torch
>>> DEVICE = ("cuda" if torch.cuda.is_available() else "cpu")
>>> print(DEVICE)
```
아래 `cuda`가 뜨면 Pytorch 설치는 성공이다.<br/>


<br/>
<br/>
<br/>
\<참고\>  
[Lupin's Development blog](https://devhyeon.tistory.com/29)<br/>