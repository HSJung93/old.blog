---
title: "Network-Time-Protocol"
date: 2021-11-05T14:41:00+09:00
categories:
  - 메모
tags:
  - Network-Time-Protocol
header:
  teaser: /assets/images/slipBox.jpg
---

네트워크 타임 프로토콜(NTP)는 인터넷상의 시간을 정확하게 유지시켜주기 위한 통신망 시간 규약이다. 기본적으로 Straum이라는 계층 구조로 구현된다. Straum 0는 GPS나 세슘 원자 시계 등으로 시간을 구하는 장비 자체를 의미한다. Straum 1은 GPS나 세슘 원자 시계에서 직접 시간을 동기화하는 서버이다. Straum 2부터는 트리구조를 형성한다. Straum 2 에서 동기화를 받은 Straum 3 서버에서 같이 운영하는 서버들을 peer로 동기화시킨다.

우리나라에서는 kr.pool.ntp.org 등에서 NTP 서버가 운영되고 있다. `yum install ntp`로 ntp를 설치한 후 서버 주소를 /etc/ntp.conf에 저장하여 설정할 수 있다.
