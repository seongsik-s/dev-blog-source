---
layout: post
title: "[python] Windows 스케줄러 기능으로 파이썬 스크립트 작업 등록하기"
tags: [Python, Windows, Linux, Cron, Crontab, Job Scheduler]
categories: [Python]
banner:
  image: /assets/images/banners/python.png
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 3.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
---

오늘은 Python 스크립트를 windows 작업 스케줄러에 등록하는 방법에 대해 포스팅 해보겠습니다~

이전에 Linux cron과 python apscheduler에 대해 포스팅을 하고나서 windows 에서 제공하는 작업 스케줄러에 대해 포스팅 해야겠다고 생각했습니다.

Linux cron과 apscheduler에 대해 궁굼하신 분들은 밑의 링크를 눌러세요!  

[[python] Apscheduler vs Cron Job](/posts/python-scheduler/)

<br>

## **1. 윈도우 작업 스케줄러 실행**  

***

![image](https://user-images.githubusercontent.com/52439201/144750739-bc0c0c0e-6b9a-4949-abb5-9bee1690c63b.png)

<br>

## **2. 윈도우 작업 등록**

***

* 오른쪽 상단에 `[작업 만들기]`를 클릭

    ![image](https://user-images.githubusercontent.com/52439201/144750828-c91f5fbf-1fe5-459d-9dc2-e6a862667757.png)  

<br>

* 이름에는 작업 등록에 사용될 이름을 기입
* 관리자 권한으로 실행하는 경우가 많으므로, `가장 높은 수준의 권한`으로 실행을 체크 (필요없으면 체크를 안하셔도 됩니다)  

    ![image](https://user-images.githubusercontent.com/52439201/144751009-b7cb4a39-00d5-48ce-812d-20786d10b6fa.png)

<br>

* `[트리거] 탭`을 누르시면 작업 주기를 설정할 수 있는 창이 뜹니다.  
* 설정에서 한 번 (최초 1회), 매일, 매주, 매월 중 본인에 맞는 주기를 선택
* 시작에서 작업 시작시간을 선택 
* 매주 월~금 오후 11:25:55 시간에 파이썬 스크립트를 돌린다고 가정하고 선택  

    ![image](https://user-images.githubusercontent.com/52439201/144751231-0e3886bd-eab0-4665-83b2-01cb5e4d33b8.png)

<br>

* `[동작] 탭`을 누르시면 실행시킬 프로그램/스크립트를 등록할 수 있습니다
* 프로그램/스크립트에는 python이 깔려있는 경로 `설치경로\python.exe`를 등록해줍니다(ex. D:\anaconda3\python.exe)
* python으로 실행해야 하므로 `python.exe`를 등록해야 합니다.
* 만약, python 설치경로를 모르신다면 cmd -> `where python`을 입력하면 위치를 알 수 있습니다
* 저같은 경우에는 가상환경을 따로 만들었고 해당 경로의 `python.exe`를 입력  
* 인수 추가(옵션)에는 실행시킬 python 스크립트의 `절대 경로`를 입력
* 시작 위치(옵션)에는 python 스크립트의 `폴더 경로`만 입력

    ![image](https://user-images.githubusercontent.com/52439201/144751629-b9993beb-00f5-444f-81b5-2714fdcf0bbf.png)

<br>

* `[조건] 탭`을 누르시면 작업 실행 여부를 결정할 수 있는 트리거들을 설정할 수 있습니다
* 만약 데스크탑이면 크게 바뀔 건 없지만, 노트북이라면 전원 > [컴퓨터의 AC 전원이 켜져 있는 경우에만 작업 시작] 을 해제
* 노트북은 배터리로 전원을 키기때문에 작업이 등록되어있어도 실행이 안되는 경우가 생깁니다.
* 작업 실행 여부에 영향을 미치는 조건을 배제시켜 무조건 돌아가게 만들 수 있습니다.  

    ![image](https://user-images.githubusercontent.com/52439201/144751951-72524e40-ba5b-4701-8f52-473030ad0c15.png)

<br>

* `[설정] 탭`을 누르시면 작업의 동작에 영향을 주는 추가 설정을 할 수 있습니다
* 작업 스케줄러가 제대로 동작 안했을 때 후 처리를 어떻게 할 것인지 설정
* 후 처리 요구사항이 있지 않다면, 건들지 않고 확인 

    ![image](https://user-images.githubusercontent.com/52439201/144752129-4e044048-7376-4601-845b-7ac1374a4a00.png)

<br>

## 3. **마무리**  

***

* linux의 cron과 비슷하게 OS 플랫폼에서 제공하는 기능이다보니 간편하게 스케줄러 작업을 등록할 수 있었습니다. 
* 앞으로 Linux, windows 환경에서 스케줄러 작업을 해야할 상황이 생기면 요긴하게 사용할 것 같습니다.