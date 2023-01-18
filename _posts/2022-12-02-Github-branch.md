---
layout: post
title:  Github 브랜치
date:   2022-12-02 20:30
image:  
tags:   Github
---

1. 브랜치 만들기  
```bash
#브랜치를 생성한다.
$git branch 브랜치이름
```
<br/>

2. 브랜치 목록 조회
```bash
# 이름을 지정하지 않고 git branch로 입력을 하면 브랜치 목록을 확인할 수 있다
$git branch 
# 원격 브랜치 목록 조회
$git branch -r
# 모든 브랜치 목록 조회
$git branch -a 
```
<br/>

3. 브랜치 전환
```bash
# 브랜치 전환
$git checkout 브랜치이름
# 브랜치 작성과 체크아웃을 한꺼번에 실행
$git checkout -b 브랜치이름
```
<br/>

4. git 특정 브랜치 pull
```bash
$git remote add origin 주소
$git pull origin 브랜치 명 
```
<br/>

5. git 특정 브랜치 clone
```bash
git clone -b branch_name --single-branch 저장소 URL
```
<br/>

6. 브랜치 삭제
```bash
# 로컬에서 브랜치 삭제
$git branch -d 브랜치 이름
# 원격에서 브랜치 삭제
$git push origin --delete 브랜치 명
```