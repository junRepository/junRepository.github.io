---
layout: post
title:  Github 파일 삭제
date:   2022-11-30 19:00
image:  
tags:   Github
---
* 파일 삭제

```bash
#원격 저장소와 로컬 저장소에 있는 파일을 삭제
$ git rm 파일 이름

#원격 저장소에 있는 파일 삭제, 로컬 저장소에 있는 파일은 삭제하지 않는다.
$ git rm --cached 파일 이름

#완전히 제거하기 위해 commit를 수행해야 한다.
$ git commit -m "Fixed untracked files"

#원격 저장소에 push
$ git push origin master
```