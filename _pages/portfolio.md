---
layout: page
title: Portfolio
permalink: /portfolio/
description: "남준우의 학력, 활동, 기술, 논문, 특허, 석사 연구 및 프로젝트 경험을 소개합니다."
research: true
---
## 남준우 · Junwoo Nam

<img class="portfolio-profile" src="{{ '/images/portfolio/profile.png' | relative_url }}" alt="남준우 프로필 사진" width="459" height="575">

**가능성을 현실로, 혁신을 일상으로 바꾸는 남준우입니다.**

## Profile

### Education

| 기간             | 학력                                                     |
| :--------------- | :------------------------------------------------------- |
| 2017.03–2024.02 | 대전대학교 경영학과 주전공 · 컴퓨터공학과 복수전공 졸업 |
| 2024.03–2026.08 | 동국대학교 대학원 컴퓨터·AI학과 졸업                    |

### Activities

| 기간             | 활동                                                                               |
| :--------------- | :--------------------------------------------------------------------------------- |
| 2018.04–2019.12 | 육군 병장 만기 전역 · 대형차량 운전병                                             |
| 2021.09          | 컴퓨터공학과 복수전공 시작                                                         |
| 2022.05–2022.12 | NAVER Connect 「소프트웨어야 놀자」 멘토 · 초·중학생 대상 자율주행 교육 재능기부 |
| 2023.02          | CDS 빅데이터 경진대회 최우수상 · 대전대학교, 연세대학교 등 11개 대학 참여         |

### Skills

| 분야                 | 기술                                       |
| :------------------- | :----------------------------------------- |
| Programming          | Python, C++                                |
| Robotics             | ROS 2, PX4, Gazebo, Isaac Sim              |
| AI / Data Processing | PyTorch, TensorFlow, OpenCV, NumPy, Pandas |
| Tools                | Git, Linux, Docker                         |

### Publications

1. **2D Layout-based Drone Warehouse Monitoring System**
   Junwoo Nam, Hangyeol Lee, Sungwon Cho, Minhee Jo, Pavel Stasa, Jin-Woo Jung · **ISIS 2025**  
   -> 2D 레이아웃 기반 경로계획을 이용한 실시간 드론 모니터링 시스템 연구.
2. **An Embedded Real-Time System for Human Trajectory Prediction Using ROS 2**
   Jihong Jeong, Junwoo Nam, Hangyeol Lee, Jongtae Rhee, Pavel Stasa, Jin-Woo Jung · **ICUFN 2026**  
   -> 임베디드 환경에서 전방 하향 시점 영상을 통해 사람의 이동 경로를 실시간으로 예측하는 ROS 2 기반 시스템 연구.
3. **Voice-Controlled Navigation for TurtleBot3 using ROS Noetic and Speech Recognition**
   Mohammad Afif Kasno, Norhidayah Mohamad Yatim, Tae-Won Kang, Jo Min Hee, Junwoo Nam, Jin-Woo Jung · **ICTC 2024**  
   -> ROS 및 음성 인식을 활용한 음성 명령 기반 TurtleBot3 실시간 자율주행 제어 시스템 연구.
4. **직전 프레임을 활용한 감정 단계 인식 모델의 성능 향상을 위한 후처리 알고리즘 연구**
   Minhee Jo, Hangyeol Lee, Junwoo Nam, Jin-Woo Jung · **한국지능시스템학회 2024 추계학술대회**  
   -> 이전 프레임의 감정을 참고하여 갑작스러운 감정 인식 오류를 줄이고 탐지 안정성을 높이는 연구.

### Patents

| 발명 명칭                                                             | 발명자         | 출원 정보       |
| :-------------------------------------------------------------------- | :------------- | :-------------- |
| 전역 경계 재균형을 이용한 다중 로봇 커버리지 경로 계획 장치 및 방법   | 남준우, 정진우 | 10-2026-0049001 |
| 센서 및 환경 정보에 기반한 다중 로봇의 적응형 커버리지 경로 계획 방법 | 남준우, 정진우 | 출원 진행 중    |

---



## Thesis

### 실내 환경에서의 카메라 탐지 영역을 고려한 적응형 그리드 기반 다수 로봇 커버리지 경로계획 연구

**1. 연구 목적**

복잡한 실내 환경에서 모든 영역을 효율적으로 커버하는 경로를 생성하는 연구입니다. 카메라가 실제로 촬영하는 영역과 실내 공간 구조를 반영하여 중복 이동을 줄이고 전체 작업 영역을 커버하는 것을 목표로 했습니다.

**2. 기존 연구의 한계**

- 장애물이 많은 창고에서는 경로 중복과 관측 누락이 발생합니다.
- 균일 그리드는 넓은 공간도 잘게 나누어 불필요한 셀과 이동 경로를 생성합니다.

<div class="portfolio-figures">
  <figure>
    <img src="{{ '/images/portfolio/boustrophedon-path.png' | relative_url }}" alt="창고 지도 위의 Boustrophedon 커버리지 경로 예시" loading="lazy" width="619" height="783">
    <figcaption>Boustrophedon 경로 예시 · PPT 4쪽</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/portfolio/stc-path.png' | relative_url }}" alt="창고 지도 위의 두 STC 경로와 관측 영역 예시" loading="lazy" width="1085" height="1086">
    <figcaption>STC 경로 예시 · PPT 4쪽</figcaption>
  </figure>
</div>

![풋프린트 크기를 기준으로 창고 공간을 나눈 균일 그리드 예시]({{ '/images/portfolio/footprint-grid.png' | relative_url }}){: .research-figure-narrow }
*풋프린트 기반 그리드 예시 · PPT 4쪽.*

**3. 연구 방법**

공간에 따라 경로의 세밀함을 조절하는 적응형 방식을 제안했습니다. 넓은 공간은 병합하고, 장애물 주변과 좁은 통로는 세밀하게 유지합니다.

<div class="portfolio-figures">
  <figure>
    <img src="{{ '/images/portfolio/uniform-grid.png' | relative_url }}" alt="전체 공간을 같은 크기의 셀로 나눈 기존 균일 그리드" loading="lazy" width="768" height="767">
    <figcaption>기존 균일 그리드 · PPT 4쪽</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/portfolio/adaptive-grid.png' | relative_url }}" alt="공간 구조에 따라 셀 크기를 조정한 제안 적응형 그리드" loading="lazy" width="734" height="736">
    <figcaption>제안한 적응형 그리드 · PPT 4쪽</figcaption>
  </figure>
</div>

1. **카메라 관측 영역 계산** — 카메라 화각과 비행 고도로 풋프린트를 계산하고, Gazebo 창고 모델에서 2D 지도를 추출합니다.
2. **AI 모델 기반 공간 특징 추출** — 자유공간, 장애물 경계, 거리 지도를 입력으로 사전학습 ResNet18을 이용해 셀별 특징을 추출합니다.
3. **적응형 그리드 생성** — 인접 셀의 병합과 작은 셀의 흡수로 넓은 공간을 단순화하고 좁은 통로를 유지합니다.
4. **영역 분할과 경로 생성** — 커버 비용에 따라 로봇별 연결 영역을 할당하고, 지역 균형화 후 Hybrid STC 경로를 생성합니다.

![석사 연구 전체 프로세스]({{ '/images/portfolio/thesis-pipeline.png' | relative_url }})
*센서 풋프린트 계산부터 로봇별 경로 생성까지의 전체 프로세스 · PPT 5쪽.*

![자유공간, 장애물 경계, 거리 지도를 ResNet18에 입력해 셀 임베딩을 추출하는 구조]({{ '/images/portfolio/resnet-features.png' | relative_url }})
*ResNet 기반 특징 추출 · PPT 5쪽.*

**4. 실험 결과**

| 비교 방법 | 최대 경로 길이 | 전체 경로 길이 | 경로 부담 표준편차 | 회전 수 |
| :-------- | -------------: | -------------: | -----------------: | ------: |
| Proposed  |        1370.73 |        4584.89 |     53.95 ± 72.17 |  779.88 |
| LS-MCPP   |        2123.68 |        7258.00 |     56.02 ± 48.56 |  605.75 |
| TMSTC*    |        3921.86 |       13428.42 |     40.97 ± 22.04 |  651.58 |

*최대 경로 길이(Makespan)는 로봇별 경로 중 가장 긴 경로입니다.*

- 최대 경로 길이: **LS-MCPP 대비 35.45%, TMSTC* 대비 65.05% 감소**.
- 전체 경로 길이: **LS-MCPP 대비 36.83%, TMSTC* 대비 65.86% 감소**.
- 회전 수는 두 비교 방법보다 높았으며, 경로 부담 표준편차는 TMSTC*가 가장 낮았습니다.

[석사 연구 상세 글]({{ site.baseurl }}{% post_url 2026-10-01-adaptive-grid-coverage %})

---



## Project

### 1. 산업 환경을 위한 AI 기반 스마트 안전 관리 시스템

**2024.03–2026.08 · 한국·체코 국제공동 R&D · 산업통상자원부 · 동국대학교**

**연구 개요**

드론과 이동로봇의 영상 및 IoT 센서 데이터를 AI로 분석하여 작업자와 물류 장비의 위험 상황을 탐지하고, 충돌 위험 예측과 경보를 지원하는 산업현장 통합 안전관리 플랫폼 개발 과제입니다.

**담당 범위: 드론 경로계획, 비행 실행, 영상 촬영 및 서버 전송**

이글아이 시스템 연구에서는 창고 내 사각지대를 줄이고, 작업자와 물류 장비가 이동하는 공간을 지속적으로 모니터링하는 것을 목표로 했습니다. 드론 촬영 영상을 창고 지도와 연결하여 작업 현장을 확인하도록 했습니다.

![창고 지도와 로봇 영상을 이글아이 시스템에 연결하는 운영 개념]({{ '/images/portfolio/eagle-eye-overview.png' | relative_url }})
*이글아이 시스템 운영 개념 · PPT 7쪽.*

**드론 기반 창고 모니터링 구현**

- 2D 지도 경유점을 실제 이동 거리와 회전각으로 변환했습니다.
- 촬영 지점에서 랙 사이 통로로 회전한 뒤 영상을 전송했습니다.
- 현장 실증으로 비행, 통로 촬영 및 서버 전송의 정상 작동을 확인했습니다.

![창고 지도와 드론 및 현장 영상을 함께 표시하는 이글아이 모니터링 화면]({{ '/images/portfolio/eagle-eye-monitoring.png' | relative_url }})
*드론 기반 창고 모니터링 화면 · PPT 8쪽.*

**다수 로봇 적용을 위한 작업영역 분할**

- 단일 드론 모니터링을 다수 로봇으로 확장하기 위한 작업영역 분할과 로봇 간 작업량 균형을 연구했습니다.
- 초기 영역 배정으로 특정 로봇에 작업이 집중되는 문제를 줄이기 위해 전역 경계 재균형 방법을 설계했습니다.
- 여러 경계 구역을 함께 재배정하면서 영역 연결성을 유지하고, 로봇별 경로 부담의 균형을 조정했습니다.

**성과: 국제학술대회 논문 2건, 특허 출원 1건**

- ISIS 2025 주저자 — *2D Layout-based Drone Warehouse Monitoring System*.
- ICUFN 2026 공동저자 — *An Embedded Real-Time System for Human Trajectory Prediction Using ROS 2*.
- 특허 출원 — *전역 경계 재균형을 이용한 다중 로봇 커버리지 경로 계획 장치 및 방법*.

[드론 모니터링 상세 글]({{ site.baseurl }}{% post_url 2026-10-01-drone-warehouse-monitoring %}) · [전역 경계 재균형 상세 글]({{ site.baseurl }}{% post_url 2026-10-01-global-boundary-rebalancing %})

### 2. 차세대 지능형 CCTV 솔루션 기술 상용화

**2025.04–2026.08 · R&D · 과학기술정보통신부 · 동국대학교**

**연구 개요**

유치장과 요양시설에서 위험 상황을 신속하게 파악하고 대응할 수 있도록, 감정 상태 분석과 쓰러짐·자해·폭행 등 이상행동 탐지 기능을 통합한 지능형 CCTV 솔루션 상용화 과제입니다.

**담당 범위: 감정 및 단계 인식 시스템 개발**

여러 인물의 감정 범주와 강도를 실시간으로 분석하여 관리자가 대상자의 상태 변화를 확인하도록 지원하고, 조도 변화와 프레임별 예측 흔들림을 줄이는 것을 목표로 했습니다.

![영상 전처리, 얼굴 탐지와 추적, 감정 및 강도 인식, TC와 EMA 후처리로 이어지는 시스템 구조]({{ '/images/portfolio/emotion-recognition.png' | relative_url }})
*감정 및 단계 인식 시스템 구조 · PPT 9쪽.*

**개발 내용**

- **얼굴 탐지와 다중 인물 추적** — RetinaFace로 얼굴을 검출하고 인물별 ID를 유지하며 추적했습니다. 얼굴 영역 정규화와 비동기 처리로 입력을 안정화했습니다.
- **감정 범주와 강도 동시 예측** — EfficientNet-B2 기반 멀티태스크 학습 모델을 적용했습니다. 부정 2단계, 중립, 긍정 2단계로 상태 변화를 표현했습니다.
- **프레임 간 결과 안정화** — 전이 제약(TC)으로 급격한 단계 변화를 제한하고, 지수 이동 평균(EMA)으로 예측 확률의 흔들림을 완화했습니다.

**평가 결과**

최대 **4명 동시 분석**, 약 **30 FPS** 처리를 확인했습니다. 남양주 북부경찰서 내 빈 유치장에서 거리, 각도, 이동 및 조도 조건을 반영한 영상 평가를 수행했습니다.

| 평가 조건 | 정확도 | F1-score |
| :-------- | -----: | -------: |
| 고조도    |    82% |    0.802 |
| 저조도    |    77% |    0.757 |

### 3. RGB+D 영상 기반 고건축물/유물의 고해상도 3D 모델링 및 이를 활용한 서비스 개발

**2025.12–2026.08 · 한국·체코 국제공동 R&D · 과학기술정보통신부 · 동국대학교**

**연구 개요**

RGB 영상과 LiDAR의 깊이 정보를 결합하여 고건축물과 유물을 고해상도 3D 모델로 복원하고, 웹 기반 실시간 시각화와 AI 해설·상호작용 서비스를 연계하는 디지털 문화유산 연구 과제입니다.

**연구 목적**

- **정밀한 디지털 복원** — 구조적 특징과 공간좌표를 추출하여 문화유산의 형상과 세부 구조를 복원합니다.
- **고품질 복원 데이터 확보** — 색상과 깊이 정보의 정합으로 3D 모델링에 활용할 영상을 생성합니다.
- **문화유산 활용 확대** — 웹에서 탐색·체험 가능한 콘텐츠로 교육·보존·관광 서비스를 지원합니다.

**담당 연구 계획: 특징점 추출 기술 개발 · RGB–LiDAR 융합 고품질 이미지 생성**

PPT에 기재된 담당 연구 계획은 다음과 같습니다.

1. **특징점 추출 기술 개발** — 모서리·곡선·꺾임 등 구조적 특징점을 자동 검출하고, 중복 특징점 제거와 좌표 정확도 개선을 수행합니다. 촬영 각도와 조명 변화에 강인한 매칭 기술을 개발하여 최소한의 특징점으로 원형에 가까운 복원을 지향합니다.
2. **RGB–LiDAR 융합 이미지 생성** — RGB 카메라와 LiDAR의 정밀 캘리브레이션, 센서 동기화, 포인트 클라우드의 영상 정합을 수행합니다. 이미지 투영으로 색상과 깊이를 결합한 RGB+D를 생성하여 구조 정보를 포함하는 고품질 3D 복원 입력을 확보합니다.

![두 이미지에서 SuperPoint 특징점과 기술자를 추출해 대응점을 찾는 구조]({{ '/images/portfolio/feature-matching.png' | relative_url }}){: .research-figure-narrow }
*특징점 추출과 대응점 매칭 개념 · PPT 12쪽.*

![RGB 영상과 LiDAR 포인트 클라우드를 캘리브레이션하고 영상에 투영해 RGBD를 생성하는 과정]({{ '/images/portfolio/rgb-lidar-fusion.png' | relative_url }})
*RGB–LiDAR 좌표계 정합 및 RGBD 이미지 생성 프로세스 · PPT 12쪽.*

---
