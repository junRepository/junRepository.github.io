---
layout: post
title:  Kubernetes
date:   2023-02-01 13:20
image:  kubernetes.png
tags:   Docker
---

## 01. Kubernetes란
<br/>
- 쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 오픝소스 플랫폼이다.
- 쿠버네티스는 서버에 컨테이너를 분산해서 배치, 교체 혹은 컨테이너가 사용할 비밀번호나 환경 설정을 관리하는 일을 한다.  
- 이러한 컨테이너의 관리를 **컨테이너 오케스트레이션(Container Orchestraint)**이라고 한다.  
- 컨테이너 오케스트레이션 도구 종류
    - 대표적으로 Kubernetes, Docker Swarm, Apach Mesos 이렇게 3가지가 있습니다.
    - 이들 중 가장 많이 쓰이는 도구가 바로 **Kubernetes** 입니다.  
<br/>
<br/>

##### Kubernetes의 구성요소
![](/images/components-of-kubernetes.svg "출처: https://kubernetes.io/docs/concepts/overview/components/"){: width="100%"}  
Kubernetes를 배포하면 cluster가 생성이 된다.  
Kubernetes cluster는 master 역할을 하는 **Control Plane node**와 worker 역할을 하는 **Node**로 구성되어 있다. 

**Control Plane node**는 다수의 worker Node를 관리하고 모니터링하면서 클라이언트로부터 요청을 받게되고 요청이 오면 요청에 맞는 worker Node를 스케줄링해서 해당 node로 전달해주는 역할을 한다.

- 사용자가 보낸 요청을 받는 컨포넌트 이름이 **API server**이다.
- 사용자가 보낸 요청을 키 value로 저장하는 DB(database)가 **etcd**이다.
- control plane에게 명령을 받고 Node에 현재 상태를 Control Plane에 전달하는 컨포넌트는 **kubelet**이다.  

***

## 02. Kubernetes 사용하기  
<br/>

##### YAML
- Kuvernetes Control Plane에 요청을 보낼 때 요청사항을 `.yaml`, `.json` 으로 정해진 형식에 맞게 보내야 한다. 

- YAML

```yaml
# key-value pairs
apiVersion: v1
kind: Pod
metadata:
  name: example
  labels:
    hello: bye
spec:
  containers:
    # list
    - name: busybox
      image: busybox:1.25
      # list
      ports:
      - containerPort: 80
    - name: another-container
      image: curlimages/curl
```
<br/>

- JSON

```json
{
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "example"
  },
  "spec": {
    "containers": [
      {
        "name": "busybox",
        "image": "busybox:1.25"
      }
    ]
  }
}
```
<br/>










<br/>
<br/>
\<출처\>  
[쿠버네티스란 무엇인가?: kubernetes](https://kubernetes.io/ko/docs/concepts/overview/)<br/>
[쿠버네티스 알아보기: SAMSUNG SDS](https://www.samsungsds.com/kr/insights/220222_kubernetes1.html)<br/>
[코드 출처: fastcampus](https://jaeyeon-kim.notion.site/Docker-1-2-be5f796e34ac4903af0c97e59f1eb98e)