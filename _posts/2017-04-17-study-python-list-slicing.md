---
layout: post
title: 파이썬 Basic
subtitle: list 슬라이싱
category: study
tags: [basic, python]
---

파이썬에서 리스트의 슬라이싱 syntax는 start:stop[:step]의 형식입니다.
여기서 [:step]은 생략이 가능하다는 의미입니다.

리스트 a에 대해서

1. step을 명시하지 않을 경우<br/>
a[start:end] # start부터 end-1까지의 item<br/>
a[start:] # start부터 리스트 끝까지 item<br/>
a[:end] # 처음부터 end-1까지의 item<br/>
a[:] # 리스트의 모든 item<br/>

2. step을 명시할 경우<br/>
a[start:end:step]# start부터 end-1까지 step만큼 인덱스 증가시키면서<br/>

3. start, end, step이 음수인 경우<br/>
a[-1] # 맨 뒤의 item<br/>
a[-2:] # 맨 뒤에서부터 item2개<br/>
a[:-n] # 맨 뒤의 item n개 빼고 전부<br/>
a[::-1] # reverse 리스트<br/>