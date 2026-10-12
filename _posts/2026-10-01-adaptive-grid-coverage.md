---
layout: post
title: "[석사 연구] 카메라 탐지 영역을 고려한 적응형 그리드 기반 다수 로봇 커버리지 경로계획"
date: 2026-10-01 00:03:00 +0900
permalink: /research/adaptive-grid-coverage/
image: research/thesis-final/fig-3-18.png
tags: [Research, Robotics, Coverage-Planning]
description: "석사학위논문을 바탕으로 센서 풋프린트, CNN 지도 특징, 적응형 그리드, 로봇 영역 할당과 STC 경로 생성 과정을 설명하고 비교 실험 결과를 분석합니다."
research: true
---

[← 남준우 포트폴리오]({{ '/portfolio/' | relative_url }})

이 글은 석사학위논문 **「실내 환경에서의 카메라 탐지 영역을 고려한 적응형 그리드 기반 다수 로봇 커버리지 경로계획 연구」**의 연구 배경, 제안 방법과 실험 결과를 정리한 글입니다. 본문과 그림은 `2026_졸업논문_남준우_최종.docx`를 기준으로 하며, 그림·표 번호는 논문의 번호를 따릅니다.

| 항목 | 내용 |
| :--- | :--- |
| 저자 | 남준우 |
| 학위·소속 | 동국대학교 대학원 컴퓨터·AI학과 석사학위논문, 2026 |
| 지도교수 | 정진우 |
| 연구 대상 | 2차원 실내 창고 레이아웃에서의 다수 로봇 커버리지 경로계획 |
| 핵심 구성 | 센서 풋프린트, ResNet18 셀 특징, 적응형 그리드, 연결 영역 할당, 지역 균형화, STC 계열 경로 생성 |

## 1. 연구 배경과 문제 정의

커버리지 경로계획(Coverage Path Planning, CPP)은 로봇이 주어진 작업 영역을 빠짐없이 방문하거나 관측하도록 경로를 만드는 문제입니다. 실내 창고의 감시·순찰에서는 로봇이 지나간 궤적과 함께 **센서가 각 위치에서 실제로 관측할 수 있는 영역**을 고려해야 합니다.

![실내 창고에서 드론 카메라의 센서 풋프린트를 활용하는 개념]({{ '/images/research/thesis-final/fig-1-1.png' | relative_url }})
*그림 1-1. 실내 창고 환경 내 센서 풋프린트 활용 예시.*

창고에는 선반, 벽, 교차 통로, 넓은 빈 공간이 함께 존재합니다. 이러한 공간을 동일한 크기의 셀로 표현하면 두 가지 문제가 생깁니다. 큰 셀은 좁은 통로에서 장애물과 겹쳐 세부 자유공간을 표현하기 어렵고, 작은 셀은 넓은 공간까지 잘게 나누어 셀과 경유점을 불필요하게 늘립니다.

여러 로봇이 작업할 때는 영역 분할도 중요합니다. 로봇별 셀 수가 비슷하더라도 셀 크기와 배치가 다르면 실제 경로 길이가 달라질 수 있습니다. 특정 로봇에 긴 경로가 집중되면 그 로봇이 전체 작업의 병목이 됩니다.

본 연구의 목표는 **센서 관측 범위에 맞는 공간 표현과 로봇별 작업량 균형을 함께 고려하는 것**입니다. 넓고 단순한 자유공간은 큰 셀로 묶고, 장애물 주변과 좁은 통로는 작은 셀로 유지한 뒤, 각 로봇에 연결된 영역을 할당하여 커버리지 경로를 생성합니다.

### 기존 접근과의 관계

논문 제2장에서 다룬 주요 접근은 다음과 같습니다.

| 접근 | 특징 | 본 연구에서 주목한 한계 |
| :--- | :--- | :--- |
| Boustrophedon | 공간을 분해하고 각 영역에서 왕복 경로 생성 | 복잡한 장애물 배치에서는 영역 간 연결을 위한 우회·중복 이동이 증가할 수 있음 |
| 균일 그리드 기반 STC | 셀 그래프의 신장 트리를 따라 커버리지 경로 구성 | 넓은 공간과 좁은 통로를 같은 해상도로 표현 |
| Quadtree | 공간을 계층적으로 분할 | 관측 범위와 셀 사이 구조적 유사성을 함께 고려하는 추가 기준이 필요 |
| Voronoi·MFC 기반 영역 분할 | 시작 위치 또는 그래프 구조를 바탕으로 로봇별 영역 구성 | 공간 표현과 실제 경로 비용에 따라 작업량 불균형이 남을 수 있음 |

## 2. 제안 방법의 전체 구조

![지도 전처리, 적응형 그리드 생성, 영역 할당 및 커버리지 경로 생성의 전체 흐름]({{ '/images/research/thesis-final/fig-3-1.png' | relative_url }})
*그림 3-1. 제안하는 커버리지 경로계획의 전체 구조도.*

제안 방법은 다음 순서로 진행합니다.

1. **지도와 센서 정보 입력**: Gazebo 창고 모델을 2D 레이아웃으로 변환하고 카메라 풋프린트를 계산합니다.
2. **입력 지도 전처리**: 자유공간, 장애물 경계, 장애물 거리 정보를 계산합니다.
3. **CNN 기반 셀 특징 추출**: 사전학습 ResNet18의 특징 맵에서 셀별 임베딩을 구합니다.
4. **적응형 그리드 생성**: 기하학적 조건과 임베딩 유사도에 따라 셀을 병합하고 잔여 셀을 흡수합니다.
5. **영역 할당과 지역 균형화**: 로봇별 연결 영역을 만들고 경계 셀 이동으로 경로 부담을 조정합니다.
6. **경로 생성과 평가**: STC 계열 절차로 순회 경로를 구성하고 경로 길이·균형·회전 수를 평가합니다.

## 3. 센서 풋프린트 정의

### 3.1 지도와 관측 모델

실험에서는 Gazebo의 실내 창고 SDF 모델에 포함된 장애물 위치, 크기와 충돌박스 정보를 추출해 2D 지도를 구성했습니다. 대표 센서는 PX4-Autopilot 드론 모델의 하향 카메라로 설정했습니다.

카메라는 지면을 수직으로 바라보고, 지면은 평면이라고 가정합니다. 이때 카메라가 지면에 투영하는 직사각형 영역을 풋프린트로 정의합니다. 수평 시야각은 폭을, 수직 시야각은 높이를 결정합니다.

<div class="research-figures">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-2.png' | relative_url }}" alt="비행 고도 h와 수평 시야각 HFOV로 풋프린트 폭을 계산하는 기하 구조" loading="lazy" width="389" height="513">
    <figcaption>그림 3-2. 수평 시야각 기반 풋프린트 폭.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-3.png' | relative_url }}" alt="비행 고도 h와 수직 시야각 VFOV로 풋프린트 높이를 계산하는 기하 구조" loading="lazy" width="376" height="494">
    <figcaption>그림 3-3. 수직 시야각 기반 풋프린트 높이.</figcaption>
  </figure>
</div>

논문의 식 (1), (2)는 다음과 같습니다.

```text
Footprint_W = 2 × h × tan(HFOV / 2)
Footprint_H = 2 × h × tan(VFOV / 2)
```

실험 설정은 **h = 5.5 m, HFOV = 82.6°, VFOV = 66°**입니다. 계산된 풋프린트는 기본 셀 크기의 기준으로 사용합니다. 장애물 주변과 좁은 통로는 더 세밀하게 표현하고, 이후 단순한 자유공간을 병합하여 최종 그리드를 구성합니다.

## 4. 지도 전처리와 CNN 기반 특징 추출

### 4.1 세 채널로 공간 구조 표현하기

CNN에는 원본 RGB 지도 대신 장애물 마스크에서 계산한 세 채널을 입력합니다.

| 채널 | 계산·의미 |
| :--- | :--- |
| 자유공간 마스크 F(p) | 작업 가능한 픽셀은 1, 그 외는 0 |
| 장애물 경계 마스크 B(p) | 자유공간 마스크에 팽창·침식을 적용하고 두 결과가 다른 위치를 경계로 표시 |
| 정규화된 장애물 거리 지도 Dnorm(p) | 가장 가까운 장애물까지의 유클리드 거리를 계산하고 0–1 범위로 정규화 |

<div class="research-figures research-figures--three">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-4-a.png' | relative_url }}" alt="자유공간을 흰색으로 표시한 이진 지도" loading="lazy" width="303" height="352">
    <figcaption>(a) 자유공간 마스크</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-4-b.png' | relative_url }}" alt="선반과 외곽 벽의 경계가 강조된 지도" loading="lazy" width="305" height="354">
    <figcaption>(b) 장애물 경계 마스크</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-4-c.png' | relative_url }}" alt="장애물로부터의 정규화된 거리를 색상으로 표현한 지도" loading="lazy" width="353" height="352">
    <figcaption>(c) 정규화된 장애물 거리 지도</figcaption>
  </figure>
</div>

*그림 3-4. 전처리 단계에서 생성한 3채널 지도 표현.*

세 채널은 각각 통과 가능성, 경계 인접성, 장애물과의 여유 거리를 제공합니다. 이를 결합하면 자유공간 비율이 비슷하더라도 넓게 열린 영역과 좁은 통로를 구분하는 데 필요한 정보를 얻을 수 있습니다.

### 4.2 ResNet18을 고정된 특징 추출기로 사용하기

![3채널 지도를 사전학습 ResNet18에 입력하고 특징 맵의 셀별 영역을 평균 풀링하는 구조]({{ '/images/research/thesis-final/fig-3-5.png' | relative_url }})
*그림 3-5. ResNet18을 이용한 셀 단위 지도 임베딩 추출 과정.*

ImageNet 사전학습 ResNet18은 **가중치를 고정한 추론 모드**로 사용하며 파인튜닝하지 않습니다. 마지막 전역 평균 풀링과 완전 연결 분류 계층을 사용하지 않고, 분류기 이전의 합성곱 특징 맵을 이용합니다.

후보 셀에 대응하는 특징 맵 영역에서 벡터들을 평균 풀링하여 셀 임베딩을 계산합니다. 이 임베딩은 셀 내부와 주변의 장애물 배치, 경계 형태와 거리 패턴을 나타냅니다.

CNN의 역할은 후보 셀의 공간 구조가 서로 유사한지 비교하기 위한 정보를 제공하는 것입니다. 최종 병합·흡수 여부는 임베딩과 함께 자유공간 비율, 장애물 거리, 인접성 등 기하학적 조건을 검사해 결정합니다.

## 5. 적응형 그리드 생성: 병합과 잔여 셀 흡수

### 5.1 셀을 평가하는 기준

그리드 생성은 세밀한 균일 그리드에서 자유공간 셀을 후보로 남기는 단계에서 시작합니다. 각 셀에 대해 다음 특성을 계산합니다.

| 특성 | 의미 | 판단에 사용하는 이유 |
| :--- | :--- | :--- |
| 자유공간 비율 ρ | 셀 내부에서 작업 가능한 픽셀의 비율 | 장애물이 많이 포함된 셀의 병합 제한 |
| 경계 밀도 β | 셀 안에 포함된 장애물 경계의 비율 | 복잡한 경계 주변의 세부 표현 유지 |
| 거리 여유 d | 셀 내부 픽셀에서 장애물까지의 평균 거리 | 장애물과 가까운 영역의 과도한 병합 제한 |
| 가로세로 비율 a | max(폭/높이, 높이/폭) | 지나치게 길어진 셀의 생성 제한 |
| 혼합도 m | 2 × min(ρ, 1−ρ) | 자유공간과 장애물이 섞인 영역 구분 |

혼합도는 자유공간과 장애물이 비슷한 비율로 섞일 때 커집니다. 모두 장애물인 셀도 혼합도가 작으므로, 이 값은 자유공간 비율과 함께 검사합니다.

셀 임베딩의 유사도는 유클리드 거리 기반으로 계산합니다. 두 셀의 특징 거리가 작을수록 `유사도 = 1 / (1 + 특징 거리)`가 1에 가까워집니다. 병합 후보 사이의 평균 유사도를 통해 기하학적 조건만으로 구분하기 어려운 구조 차이를 추가로 확인합니다.

### 5.2 셀 병합

같은 부모 영역에 속하는 네 개의 형제 셀을 대상으로, 병합 후의 상위 셀이 기하학적 조건을 만족하는지 먼저 검사합니다. 그다음 후보 셀들의 평균 임베딩 유사도가 임계값 이상인지 확인하여 두 조건을 모두 만족할 때 병합합니다.

<div class="research-figures">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-10-a.png' | relative_url }}" alt="병합 후보인 네 개의 작은 셀" loading="lazy" width="954" height="882">
    <figcaption>(a) 병합 전 후보 셀</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-10-b.png' | relative_url }}" alt="네 개의 후보 셀이 하나의 상위 셀로 병합된 결과" loading="lazy" width="952" height="877">
    <figcaption>(b) 병합 후 후보 셀</figcaption>
  </figure>
</div>

*그림 3-10. 자유공간 영역에서의 셀 병합 예시.*

논문에 제시된 병합 설정에는 최소 거리 여유 **5.0 px**, 최대 경계 밀도 **0.030**, 최대 가로세로 비율 **1**, 최대 혼합도 **0.28**이 포함됩니다. 임베딩 평균 유사도 임계값은 공간 복잡도에 따라 **0.55 또는 0.35**를 적용합니다. 장애물 주변에는 더 엄격한 유사도를 요구하고, 단순한 자유공간에는 완화된 기준을 적용하는 방식입니다.

### 5.3 잔여 셀 흡수

병합 조건을 만족하는 형제 셀이 충분하지 않으면 작은 셀이 독립적으로 남을 수 있습니다. 잔여 셀 흡수는 이러한 작은 셀을 인접한 큰 셀과 통합하는 후처리입니다.

<div class="research-figures">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-11-a.png' | relative_url }}" alt="큰 셀 아래쪽에 잔여 작은 셀이 남은 상태" loading="lazy" width="563" height="548">
    <figcaption>(a) 잔여 작은 셀 흡수 전</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-11-b.png' | relative_url }}" alt="잔여 셀이 인접한 큰 셀과 통합된 결과" loading="lazy" width="563" height="550">
    <figcaption>(b) 잔여 작은 셀 흡수 후</figcaption>
  </figure>
</div>

*그림 3-11. 잔여 작은 셀 흡수 예시.*

흡수 후 통합 영역의 인접성, 자유공간 비율, 경계 밀도, 가로세로 비율, 혼합도를 검사합니다. 여기에 잔여 셀과 대상 셀 사이의 평균 임베딩 유사도 **0.55 이상**을 요구합니다. 따라서 가까운 셀에 무조건 붙이는 방식이 아니라, 통합 후에도 공간 구조를 적절히 표현하는 경우에만 흡수합니다.

![넓은 자유공간은 큰 셀로, 좁은 통로와 장애물 주변은 작은 셀로 표현한 최종 적응형 그리드]({{ '/images/research/thesis-final/fig-3-12.png' | relative_url }})
*그림 3-12. 병합·흡수·최종 검증을 거친 적응형 그리드.*

최종적으로 셀을 노드로, 서로 인접하여 이동 가능한 셀 사이의 관계를 간선으로 갖는 그래프를 구성합니다. 이 그래프가 로봇 영역 할당의 입력이 됩니다.

## 6. 로봇 영역 할당과 지역 균형화

### 6.1 영역 할당이 만족해야 할 조건

로봇별 영역은 다음 조건을 만족하도록 구성합니다.

- 하나의 셀은 하나의 로봇에 배정합니다.
- 전체 커버리지 대상 셀을 빠짐없이 배정합니다.
- 각 로봇의 영역은 자신의 시작 셀을 포함하고 하나의 연결된 영역을 이룹니다.

각 시작 셀에서 영역을 성장시키되, 현재 영역과 인접한 미할당 셀만 후보로 사용합니다. 이렇게 하면 새 셀을 추가할 때도 영역의 연결성을 유지할 수 있습니다.

### 6.2 초기 영역 성장의 비용

적응형 그리드에서는 셀 크기가 다르므로 셀 개수만으로 작업량을 비교하기 어렵습니다. 초기 영역 성장에서는 셀의 면적과 폭·높이에 따른 이동 부담을 반영하는 셀 작업 비용을 누적합니다. 이 값은 최종 경로 생성 전의 **작업량 추정값**입니다.

후보 셀의 종합 할당 비용은 논문의 식 (29)에 따라 네 요소로 구성합니다.

```text
J(i, v) = wγ × 셀 작업 비용
        + wg × 시작 셀에서 후보 셀까지의 그래프 거리
        + wb × 후보 셀 추가 후 평균을 초과하는 작업량
        + wθ × 기존 영역 확장 방향과의 차이
```

그래프 거리는 셀 중심 사이의 유클리드 거리를 간선 비용으로 사용한 최단 경로 거리입니다. 방향성 항은 영역이 한쪽으로 길게 늘어나거나 불규칙하게 퍼지는 것을 완화하기 위해 사용합니다.

먼저 확장 가능한 로봇 중 누적 작업량이 가장 작은 로봇을 선택하고, 그 로봇의 후보 셀 중 종합 비용이 가장 낮은 셀을 배정합니다. 작업량이 같으면 할당 영역 크기, 로봇 인덱스 순으로 선택 순서를 정합니다. 이를 모든 셀이 배정될 때까지 반복합니다.

### 6.3 실제 경로 길이를 이용한 지역 균형화

초기 할당 이후에는 로봇별 임시 STC 경로를 생성해 실제 경로 길이를 계산합니다. 논문의 식 (33), (34)를 풀어 쓰면 다음과 같습니다.

```text
Pᵢ = 로봇 i의 연속된 경로점 사이 유클리드 거리의 합
M  = max(P₁, P₂, …, Pₖ)
```

여기서 M은 가장 긴 로봇 경로의 비용인 Makespan입니다. 초기 영역 성장의 추정 작업량과 달리, 이 단계에서는 **생성된 경로의 길이**를 기준으로 균형을 판단합니다.

가장 긴 경로를 가진 로봇의 경계 셀을 이웃 로봇에 넘기는 후보를 검사하고, 다음 조건을 모두 만족할 때 이동을 허용합니다.

1. 이동할 셀이 로봇의 시작 셀이 아닐 것.
2. 셀을 주고받은 두 로봇의 영역이 모두 연결되어 있을 것.
3. 이동 후 경로를 다시 계산했을 때 전체 Makespan이 감소할 것.

<div class="research-figures research-figures--three">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-13-a.png' | relative_url }}" alt="각 로봇 시작 셀에서 영역이 성장하는 초기 상태" loading="lazy" width="535" height="535">
    <figcaption>(a) 시작 셀 기반 초기 영역 성장</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-13-b.png' | relative_url }}" alt="지역 균형화를 수행하기 전의 로봇별 영역" loading="lazy" width="538" height="538">
    <figcaption>(b) 지역 균형화 전</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-13-c.png' | relative_url }}" alt="경계 셀 이동으로 작업 부담을 조정한 로봇별 영역" loading="lazy" width="526" height="526">
    <figcaption>(c) 지역 균형화 후</figcaption>
  </figure>
</div>

*그림 3-13. 로봇 영역 할당과 지역 균형화 과정.*

## 7. 적응형 셀에서 커버리지 경로 생성

영역 할당을 마친 뒤에는 각 로봇의 셀 집합을 실제로 순회하는 경로로 변환합니다. 본 연구는 STC(Spanning Tree Coverage) 계열의 구성을 사용하되, 크기가 다른 적응형 셀을 연결할 수 있도록 셀 내부와 셀 사이의 경유점을 구성합니다.

### 셀 그래프와 신장 트리

먼저 로봇에 할당된 셀을 노드로 두고 인접 관계를 간선으로 표현합니다. 셀 중심은 위치와 연결 방향을 판단하는 대표점이며, 중심 사이 거리는 간선 길이를 구성하는 데 사용합니다. 로봇별 부분 그래프에서 모든 할당 셀을 잇는 신장 트리를 생성합니다.

<div class="research-figures research-figures--three">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-15-a.png' | relative_url }}" alt="한 로봇에 할당된 적응형 셀 집합" loading="lazy" width="347" height="417">
    <figcaption>(a) 할당된 셀 집합</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-15-b.png' | relative_url }}" alt="할당 셀의 인접 관계를 표현한 부분 그래프" loading="lazy" width="344" height="410">
    <figcaption>(b) 로봇별 부분 그래프</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-15-c.png' | relative_url }}" alt="할당된 모든 셀을 연결하는 신장 트리" loading="lazy" width="346" height="414">
    <figcaption>(c) 신장 트리</figcaption>
  </figure>
</div>

*그림 3-15. 할당 영역에서 부분 그래프와 신장 트리를 생성하는 과정.*

### 셀 사이와 셀 내부의 경유점 연결

신장 트리의 연결을 실제 경로로 옮길 때는 인접한 두 셀의 공유 경계에 가까운 하위 경유점들을 연결합니다. 큰 셀은 중심점만 통과해서는 내부 관측을 충분히 표현할 수 있으므로, 셀 내부의 하위 경유점도 순회하도록 연결합니다.

<div class="research-figures">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-16-a.png' | relative_url }}" alt="인접 셀의 경계에 있는 하위 경유점을 연결한 모습" loading="lazy" width="829" height="700">
    <figcaption>(a) 셀 사이의 경유점 연결</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-3-16-b.png' | relative_url }}" alt="큰 셀 내부를 순회하는 하위 경유점 연결" loading="lazy" width="508" height="431">
    <figcaption>(b) 셀 내부의 경유점 연결</figcaption>
  </figure>
</div>

*그림 3-16. 셀 사이 및 셀 내부의 하위 경유점 연결.*

연결된 경유점들을 하나의 순회 그래프로 구성한 뒤, 로봇 시작 셀에 가장 가까운 하위 경유점에서 출발하도록 방문 순서를 정합니다. 아래 그림은 이러한 절차로 생성한 로봇별 최종 경로입니다.

![적응형 그리드에서 생성된 네 로봇의 최종 커버리지 경로]({{ '/images/research/thesis-final/fig-3-18.png' | relative_url }})
*그림 3-18. 로봇별 최종 커버리지 경로.*

## 8. 실험 환경과 평가 기준

실험에는 장애물 배치와 통로 구조가 서로 다른 **Warehouse, AWS Warehouse, Jetty, Tugbot Depot**의 네 지도를 사용했습니다.

<div class="research-figures">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-1-a.png' | relative_url }}" alt="Warehouse 실험 지도" loading="lazy" width="181" height="281" style="width:auto;max-height:240px;margin-left:auto;margin-right:auto">
    <figcaption>(a) Warehouse</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-1-b.png' | relative_url }}" alt="AWS Warehouse 실험 지도" loading="lazy" width="72" height="106" style="width:auto;max-height:240px;margin-left:auto;margin-right:auto">
    <figcaption>(b) AWS Warehouse</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-1-c.png' | relative_url }}" alt="Jetty 실험 지도" loading="lazy" width="341" height="399" style="width:auto;max-height:240px;margin-left:auto;margin-right:auto">
    <figcaption>(c) Jetty</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-1-d.png' | relative_url }}" alt="Tugbot Depot 실험 지도" loading="lazy" width="182" height="108" style="width:auto;max-height:240px;margin-left:auto;margin-right:auto">
    <figcaption>(d) Tugbot Depot</figcaption>
  </figure>
</div>

*그림 4-1. 실험에 사용한 네 가지 지도.*

로봇 수는 2–6대, 랜덤 시드는 0–4로 설정했습니다. 기본 비교는 네 지도에서 네 방법을 각각 다섯 가지 로봇 수와 다섯 시드로 실행한 **400회 실험**입니다. 아래의 그리드 구성 요소 비교와 LS-MCPP·TMSTC* 비교는 이 기본 비교와 별도로 구분합니다.

| 기본 비교 방법 | 비교 목적 |
| :--- | :--- |
| Proposed | 제안한 적응형 그리드와 영역 할당·균형화 절차 |
| Uniform | 균일 그리드와 적응형 공간 표현의 차이 확인 |
| Voronoi balanced | 동일한 적응형 그리드에서 영역 할당 방식 비교 |
| MFC balanced | 동일한 적응형 그리드에서 영역 할당 방식 비교 |

평가는 셀 수만으로 판단하지 않고 최종 생성 경로를 기준으로 수행했습니다.

| 지표 | 의미 |
| :--- | :--- |
| Makespan | 로봇별 경로 길이 중 최댓값. 가장 긴 경로를 맡은 로봇의 부담 |
| 총 경로 길이 | 모든 로봇의 경로 길이를 합한 값 |
| 작업량 표준편차 | 최종 경로 길이가 로봇 사이에서 얼마나 불균등한지 나타내는 값 |
| 회전 수 | 경로에서 이동 방향이 바뀌는 횟수 |

여기서 Makespan은 **경로 길이를 이용한 비용 지표**이며, 실측 작업 시간(초)이 아닙니다. 경로 길이 그래프는 지도 좌표의 픽셀(px) 단위를 사용합니다. 회전 수도 경로의 특성을 나타내며 실제 비행 안정성을 직접 측정한 값은 아닙니다.

## 9. 실험 결과와 해석

### 9.1. 그리드 구성 요소 비교

논문에서는 제안 방법, Quadtree, 그리고 ResNet 특징 유사도 조건을 제거한 Geometry-only를 비교했습니다. 이후 영역 할당과 경로 생성 절차를 동일하게 적용하여 공간 표현의 영향을 확인했습니다.

<div class="research-figures research-figures--three">
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-2-a.png' | relative_url }}" alt="제안 방법으로 구성한 적응형 그리드" loading="lazy" width="553" height="553">
    <figcaption>(a) Proposed</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-2-b.png' | relative_url }}" alt="Quadtree 방식으로 구성한 그리드" loading="lazy" width="555" height="555">
    <figcaption>(b) Quadtree</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/research/thesis-final/fig-4-2-c.png' | relative_url }}" alt="기하학적 조건만으로 구성한 그리드" loading="lazy" width="612" height="612">
    <figcaption>(c) Geometry-only</figcaption>
  </figure>
</div>

*그림 4-2. 그리드 구성 방식에 따른 공간 표현 비교.*

| 방법 | 셀 수 | 간선 수 | Makespan | 총 경로 길이 | 회전 수 |
| :--- | ---: | ---: | ---: | ---: | ---: |
| Proposed | 288 | 420 | 1842.31 ± 162.48 | 7021.77 ± 221.58 | 459.4 ± 13.0 |
| Quadtree | 530 | 883 | 2396.98 ± 80.03 | 9486.68 ± 383.40 | 843.8 ± 50.0 |
| Geometry-only | 671 | 1147 | 2828.22 ± 46.97 | 11162.08 ± 25.52 | 1071.0 ± 35.0 |

*표 4-1의 그리드 구성 요소 비교 결과.*

이 비교 조건에서 제안 방법은 셀과 간선을 줄이면서 Makespan, 총 경로 길이, 회전 수도 낮췄습니다. 기하학적 조건과 지역 특징을 함께 이용하는 공간 표현이 후속 경로의 복잡도에도 영향을 준다는 결과입니다.

### 9.2. 지도별 Makespan 비교

다음은 논문 표 4-2에서 지도별 평균 Makespan을 발췌한 결과입니다.

| 지도 | Proposed | Uniform | Voronoi balanced | MFC balanced |
| :--- | ---: | ---: | ---: | ---: |
| Warehouse | **1330.0** | 3732.7 | 1420.9 | 1778.0 |
| AWS Warehouse | 966.6 | 3374.9 | **926.4** | 1107.6 |
| Jetty | **2085.3** | 7709.4 | 2161.0 | 2138.6 |
| Tugbot Depot | **1101.1** | 1723.0 | 1213.9 | 1177.8 |

*표 4-2 중 지도별 평균 Makespan. 굵은 값은 각 지도에서 가장 작은 값.*

제안 방법은 네 지도 모두에서 Uniform보다 낮은 Makespan을 보였고, Warehouse·Jetty·Tugbot Depot에서는 비교한 네 방법 중 가장 낮았습니다. 다만 AWS Warehouse에서는 Voronoi balanced가 더 낮았습니다. 지도 구조에 따라 유리한 영역 할당 방식이 달라질 수 있다는 점도 함께 확인할 수 있습니다.

### 9.3. 로봇 수에 따른 지표 변화

![로봇 수에 따른 네 방법의 Makespan 비교 그래프]({{ '/images/research/thesis-final/fig-4-4.png' | relative_url }})
*그림 4-4. 로봇 수에 따른 Makespan 비교.*

로봇 수가 늘어나면 작업을 분담하면서 Makespan이 감소하는 경향을 보입니다. 제안 방법은 Uniform보다 낮은 경로 비용을 유지하며, 적응형 그리드를 사용하는 다른 영역 할당 방법들과도 비교적 낮은 수준을 보입니다.

![로봇 수에 따른 네 방법의 회전 수 비교 그래프]({{ '/images/research/thesis-final/fig-4-5.png' | relative_url }})
*그림 4-5. 로봇 수에 따른 회전 수 비교.*

적응형 공간 표현을 적용한 방법들은 Uniform보다 회전 수가 적었습니다. 그러나 적응형 그리드 방법끼리 비교하면 MFC balanced의 평균 회전 수가 더 낮았습니다. 따라서 Makespan 개선을 곧바로 회전 수의 최적화로 해석할 수는 없습니다.

![로봇 수에 따른 네 방법의 총 경로 길이 비교 그래프]({{ '/images/research/thesis-final/fig-4-6.png' | relative_url }})
*그림 4-6. 로봇 수에 따른 총 경로 길이 비교.*

총 경로 길이에서도 Uniform과 적응형 그리드 방법들 사이의 차이가 나타났습니다. 적응형 그리드 방법 사이의 차이는 상대적으로 작고, MFC balanced는 평균 총 경로 길이에서 유리했습니다. 전체 이동량을 줄이는 목표와 가장 긴 로봇 경로를 줄이는 목표는 구분해서 평가해야 합니다.

![로봇 수에 따른 네 방법의 작업량 표준편차 비교 그래프]({{ '/images/research/thesis-final/fig-4-7.png' | relative_url }})
*그림 4-7. 로봇 수에 따른 작업량 표준편차 비교.*

기본 비교의 평균 결과에서 제안 방법은 로봇별 경로 길이의 편차를 낮게 유지했습니다. 이는 영역의 연결성을 유지하면서 실제 경로 비용을 기준으로 경계 셀을 조정하는 균형화 과정의 목적과 일치합니다.

### 9.4. LS-MCPP·TMSTC*와의 비교

추가 비교에서는 지도, 로봇 수, 시작 위치와 평가 지표를 맞추어 LS-MCPP 및 TMSTC*와 비교했습니다.

| 방법 | 평균 Makespan | 평균 총 경로 길이 | 작업량 표준편차 | 평균 회전 수 |
| :--- | ---: | ---: | ---: | ---: |
| Proposed | **1370.73** | **4584.89** | 53.95 ± 72.17 | 779.88 |
| LS-MCPP | 2123.68 | 7258.00 | 56.02 ± 48.56 | **605.75** |
| TMSTC* | 3921.86 | 13428.42 | **40.97 ± 22.04** | 651.58 |

*표 4-3. 기존 알고리즘과의 비교 결과. 작업량 표준편차 열의 ± 값은 논문의 표기를 유지했습니다.*

표의 평균값을 기준으로 계산하면 제안 방법의 Makespan은 LS-MCPP 대비 약 **35.45%**, TMSTC* 대비 약 **65.05%** 낮았습니다. 평균 총 경로 길이도 각각 약 **36.83%**, **65.86%** 낮았습니다.

반면 회전 수는 두 비교 방법보다 많았으며, 작업량 표준편차는 TMSTC*가 더 낮았습니다. 이 결과는 제안 방법의 장점이 해당 실험에서의 **최장 경로와 전체 이동 거리 감소**에 있음을 보여주며, 모든 지표에서 우수하다는 의미는 아닙니다.

## 10. 연구의 기여와 한계

본 연구는 카메라의 관측 범위를 출발점으로 공간을 표현하고, 기하학적 조건과 CNN 특징을 함께 사용하여 적응형 셀 그래프를 구성했습니다. 이어서 연결된 영역 할당, 실제 경로 비용에 따른 지역 균형화, 셀 내부와 셀 사이를 잇는 경로 생성을 하나의 절차로 연결했습니다.

실험에서는 공간 표현 방식과 영역 할당 방식의 영향을 나누어 비교했습니다. 제안 방법은 실험 조건에서 균일 그리드보다 경로 비용을 줄였으며, 기존 알고리즘과의 비교에서도 Makespan과 총 경로 길이에서 개선을 보였습니다. 동시에 회전 수와 작업량 편차에서는 다른 방법이 유리한 경우도 확인했습니다.

검증 범위는 네 가지 2차원 지도와 제한된 로봇 수·시작 조건의 시뮬레이션입니다. 실제 로봇의 위치 추정 오차, 동역학, 센서 노이즈, 통신 지연은 이 결과만으로 검증되지 않습니다. 후속 연구에서는 지도와 시작 조건을 확대하고, 특징 사용 여부와 기하학적 조건·임계값의 영향을 더 폭넓게 분석하며, 실제 로봇 또는 고충실도 시뮬레이션에서 경로 길이·회전·작업 균형을 함께 평가할 필요가 있습니다.


---

[다음 연구: 전역 경계 재균형 특허 →]({{ site.baseurl }}{% post_url 2026-10-01-global-boundary-rebalancing %}) · [포트폴리오 전체 보기]({{ '/portfolio/' | relative_url }})
