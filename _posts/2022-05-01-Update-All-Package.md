---
toc: true
layout: post
description: 가상환경의 패키지를 pip명령으로 전부 업데이트하기
categories: [markdown]
title: "[Python]가상환경 패키지 업데이트"
---

`
import pkg_resources
from subprocess import call

for dist in pkg_resources.working_set:
    call("python -m pip install --upgrade " + dist.project_name, shell=True)
`
