---
layout: post
title:  Github 파일 올리기
date:   2022-11-20 20:30
image:  
tags:   Github
---
* 처음 올릴 때

```bash
#git bash를 열어준다
#사용자 git의 이름과 이메일을 입력하여 환경설정을 한다.
$git config --global user.name "이름"
$git config --global user.email "email"

#user.name과 user.email 설정이 잘 되었나 확인 
$git config --list

#git 초기화를 한다.
$git init

#파일 추가 
$git add 파일명
#모든 파일 추가
$git add .

#파일의 상태를 확인할 수 있는 명령어
$git status

#깃 히스토리 만들기
$git commit -m "commit 내용"

#리파짓토리로 소스코드를 보내는 곳 
$git remote add origin 자신의 리파짓토리 주소

#제대로 연결이 되었는 지 확인
$git remote -v

#파일 올리기 
$git push origin master

#파일 받기
$git pull origin main --allow-unrelated-histories
```
<br/>
* 기존 파일을 업데이트 할 때  

```bash
$git add .
#또는 
$git add 파일명

#commit 내용
$git commit -m "commit 내용"

#파일 올리기
$git push origin master
```
<br/>

#### Error 해결
* "LF will be replaced by CRLF the next time Git touches it"
```bash
$git config --global core.autocrlf true
```