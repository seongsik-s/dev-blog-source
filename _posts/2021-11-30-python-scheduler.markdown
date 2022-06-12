---
layout: post
title: "[python] Apscheduler vs Cron Job"
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

python 개발일을 하다보면 스케줄러 프로그램을 만들일이 생기는데, python에서 스케줄러 라이브러리와 linux에서 제공하는 스케줄링 기능에 대해 알아보고

어떤 상황에 무엇을 사용해야 좋을지 포스팅 해보겠습니다.

<br>

## **APScheduler**

***

`Advanced Python Schedule`의 약자로 python code가 한 번 또는 주기적으로 실행되도록 예약할 수 있는 python libary cron Daemon 이나 windows 작업 스케줄러와 같은 플랫폼별 스케줄러에 대체품으로 사용 

### **scheduling 방식**
- Cron 스타일 스케줄링
- Interval(간격) 기간 실행
- 일회성 지연 실행 

### **같이 사용할 수 있는 시스템**
- 메모리 (default)
- SQLAlchemy
- 몽고DB
- 레디스
- RethinkDB
- 주키퍼
- Flask
- Django 


### **scheduling 선택**

가장 많이 사용되는 2가지를 소개하겠습니다.

- `BlockingScheduler`: 스케줄러가 프로세스에서 실행 중인 유일한 경우에 사용
- `BackgroundScheduler`: 프레임워크를 사용하지 않고 스케줄러가 애플리케이션 내부의 백그라운드에서 실행되기를 원할 때 사용

### **사용방법** 

pip를 사용하여 설치 보통 작업하는 가상환경에 설치

{% highlight console %}
 $ pip install apscheduler
{% endhighlight %}  


### **BlockingScheduler**

```python
    import time
    from apscheduler.schedulers.blocking import BlockingScheduler

    sched = BlockingScheduler()

    # 3초마다 실행
    @sched.scheduled_job('interval', seconds=3, id='test_1')
    def test1():
        print(f'test1 : {time.strftime("%H:%M:%S")}')

    print('job Start')
    sched.start()
    print('job end')
```

```console
    job Start
    test1 : 10:13:44
    test1 : 10:13:47
    test1 : 10:13:50
    test1 : 10:13:53
    test1 : 10:13:56
    ...
```

BlockingScheduler 스케줄러를 사용하게되면 sched.start()에 block이 걸려 해당 프로세스만 계속해서 유지하고 있기 떄문에
print('job end')가 실행되지 않습니다.

<br>

### **BackgroundScheduler**

```python
    import time
    from apscheduler.schedulers.background import BackgroundScheduler

    sched = BackgroundScheduler()

    # 3초마다 실행
    @sched.scheduled_job('interval', seconds=3, id='test_1')
    def test1():
        print(f'test1 : {time.strftime("%H:%M:%S")}')

    print('job Start')
    sched.start()
    print('job end')
```

```console
    job Start
    job end
```

BackgroundScheduler 스케줄러를 사용하게 되면 sched.start() 3초를 기다리지않고 바로 프로그램이 종료됩니다. 백그라운드로 돌아가기 때문입니다.

sched.start() 함수 다음에도 다른 작업을 하기 위해서는 BackgroundScheduler를 사용해야 합니다.


```python
    import time
    from apscheduler.schedulers.background import BackgroundScheduler

    sched = BackgroundScheduler()

    # 3초마다 실행
    @sched.scheduled_job('interval', seconds=3, id='test_1')
    def test1():
        print(f'test1 : {time.strftime("%H:%M:%S")}')

    print('job Start')
    sched.start()
    print('job end')

    while True:
        time.sleep(1)
```

프로그램을 계속해서 유지하기 위해서는 while문 하나만 넣어주면 됩니다.

```python
    job Start
    job end
    test1 : 10:46:23
    test1 : 10:46:26
    test1 : 10:46:29
    test1 : 10:46:32
    test1 : 10:46:35
    ...
```

<br>


## **Crontab**

***

유닉스/리눅스에서 특정 시간에 특정 작업을 하는 데몬을 Cron이라고 합니다. Cron에서 어떤 명령을 언제 수행하도록 만든 리스트를 Crontab 이라고 합니다.
Crontab을 특정 파일을 등록해서 스케줄링 작업한다고 이해한다기 보다, 리눅스 명령어(프로그램)를 스케줄링 한다고 이해하시는게 더 정확한 표현입니다.


### **Crontab 등록, 조회, 삭제**
리눅스 쉘에서 아래의 명령어를 입력하면 Crontab을 등록할 수 있습니다.
```console
    $ crontab -e
```

리눅스 쉘에서 아래의 명령어를 입력하면 동록된 Crontab 리스트를 조회할 수 있습니다.
```console
    $ crontab -l
```

리눅스 쉘에서 아래의 명령어를 입력하면 동록된 Crontab 리스트를 삭제할 수 있습니다.
```console
    $ crontab -d
```

리눅스 쉘에서 아래의 명령어를 입력하면 동록된 Crontab 리스트를 전체 삭제할 수 있습니다.
```console
    $ crontab -r
```


### **Crontab 설정** 
```console
    * 분(0-59)　　　　　　* 시간(0-23)　　　　　　* 일(1-31)　　　　　　* 월(1-12)　　　　　　* 요일(0-6, 일요일=0)

    # 1분 마다 실행
    * * * * * path/test.sh

    # 10분 마다 실행
    */10 * * * * path/test.sh

    # 1시간 마다 실행
    0 * * * * path/test.sh

    # 2시간 마다 실행
    0 */2 * * * path/test.sh

    # 매일 매시간 10분, 20분, 30분에 test.sh 를 실행
    10,20,30 * * * * path/test.sh

    # 06~12시내 1시간 마다 실행
    00 06-12 * * * path/test.sh

    # 매일 15시 0분부터 30분까지 매분 tesh.sh를 실행
    0-30 15 * * * path/tesh.sh

    # 특정 시간 마다 실행 / 매주 월요일 15시 30분 마다 실행
    30 15 * * 1 path/test.sh
```

### **Crontab 주의사항** 
1. 실행 파일 path 설정, cron은 보안문제로 개인 설정 파일을 참조하지 않습니다. 그러므로 실행 프로그램은 /bin/.../... 같은 일반 경로가 있어야 cron에 제대로 반영됩니다.

2. crontab 등록시 한줄에 하나씩 등록해야 합니다.

3. #를 사용하여 주석을 사용할 수 있습니다.

4. crontab이 실제로 실행됐는지 확인할 수 있는 방법이 없기 때문에 로그를 남겨야 합니다. 로그를 남기는 방법은 

```console
    # > path/crontab.log 2>%1  입력한 path에 crontab.log라는 파일이 생성되며 실행될 때마다 log를 저장합니다.
    * * * * * path/test.sh > path/crontab.log 2>%1
```

5. crontab 변경 후에는 반드시 cron을 다시 실행해야 변경 내용이 제대로 반영됩니다.

<br>

## **apscheduler vs cron job**

***

이번 포스트의 핵심이죠, 저와 비슷한 상황의 사용자에게 도움이 되었으면 좋겠습니다.

어떤 기능을 사용할지 고르기전에 어떤 상황인지를 설명드려야겠죠?

* 상황1 : `python script 실행`
* 상황2 : `매일 특정 시간이 되면 실행 (1일 - 1회)`

만약 python의 apscheduler를 매일 특정시간에 구동시켜야한다면, 세션이 종료되더라도 돌아갈 수 있게 만들어야합니다.
예를들어, linux의 nohup과 &(백그라운드)를 사용해서 세션이 종료되더라도 백그라운드로 python script를 돌릴 수 있습니다.
하지만, 하루에 한번만 실행하면 되는 조건에서 불필요한 전력, 메모리, cpu 등의 자원을 소비할 필요가 없다고 생각됩니다.

반대로, apscheduler는 이미 작성된 code내에서 커스텀이 가능합니다.

예를들어 기능마다 실행되야될 시간이 다르다면, 혹은 다른 시스템과의 협업으로 많은 요구사항이 필요하다면 apscheduler를 사용하는게 좋습니다.
```python
    import time
    from apscheduler.schedulers.background import BackgroundScheduler

    sched = BackgroundScheduler()

    # 매일 13시 30분에 실행
    @sched.scheduled_job('cron', hour='13', minute='30', id='job_1')
    def job1():
        print('job1')

    # 매일 15시 50분에 실행
    @sched.scheduled_job('cron', hour='13', minute='50', id='job_2')
    def job2():
        print('job2')

    # 매일 17시 10분에 실행
    @sched.scheduled_job('cron', hour='17', minute='10', id='job_3')
    def job3():
        print('job3')

    while True:
        time.sleep(1)
```
`crontab은 단순히 명령어(프로그램)를 등록하는 것이기 때문에 code내 커스텀이 불가능합니다.`


### **결론**
1. 자원의 관점, 요구사항이 특정 시간에만 스크립트가 실행되어야 한다(요구사항이 단순) -> `crontab`
2. 기능의 관점, 기능별로 스케줄 시간이 달라야 하거나, 요구사항이 복잡하다 -> `apscheduler`

