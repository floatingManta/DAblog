---
toc: true
layout: post
description: VSCode의 VSIX파일을 다운로드 받아야 하는데, 이전 버전을 다운로드 받기 Download VSC Extension Previous version VXIS file
categories: [markdown]
title: VSCode 확장 이전 버전 VSIX 파일 다운로드 받기 
---

# Visual Studio Code Extension의 이전 버전 VSIX파일 다운로드

## 목적

VSCode의 확장을 오프라인으로 설치하려면, 스토어에서 VSIX파일을 다운받아야 한다.  
이전 버전을 설치하려면, VSCode에서 확장을 오른클릭하여 나오는 다른 버전 설치하기를 누른다.  

그런데 이전 버전을 오프라인 설치하려면?  
스토어의 다운로드 기능은 항상 최신 버전 VSIX만 제공한다.  

## 해결

https://stackoverflow.com/questions/69398500/vscode-download-older-version-of-an-extension 
관련 stackoverflow 글

https://www.vsixhub.com/  
vsix 이전 버전 보관 사이트

## 상세 상황
Offline환경에서 VSCode의 IntelliSense가 작동하지 않는다.  
Setting.json을 수정하여 언어를 pylance로 바꾸고 ExtraPaths는 이미 추가해 본 상황  
최후의 수단으로 Jupyter 확장을 업데이트 시도  
그런데 다운받은 VSIX가 VSCode 1.66.1과 호환되지 않는다는 에러 발생  
그럼 설치 가능한 이전 버전을 찾아야하기 때문에 본 문서의 해결책 탐색
