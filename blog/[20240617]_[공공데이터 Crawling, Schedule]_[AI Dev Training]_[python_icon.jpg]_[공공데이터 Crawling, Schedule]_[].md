# 공공데이터
공공데이터 포털 사이트 주소

=> https://www.data.go.kr/


## 크롤링을 진행할 데이터

=> 한국산업인력공단 해외취업 통계정보

=> https://www.data.go.kr/iim/api/selectAcountList.do


## apidata.py
이전에 만든 dbconn_orm.py, conf.py의 클래스를 알맞게 활용

```python
# from helpers.debug import *
import requests  # python용 HTTP 라이브러리
import pandas as pd  # 데이터 구조화 / 조작 / 분석
import sys  # System 관련 / 인터프리터와 상호작용

from db.dbconn_orm import Mysqldb
from helpers.conf import Config

import xml
import json

from db.dbconn import Dbwork

import datetime

now = datetime.datetime.now()


# 공공데이터를 활용한 크롤링 / DataFrame 및 DB Insert 확인하기
class Crw:
    def __init__(self, file_name):
        target_base = ("https://api.odcloud.kr/api/15083272/v1/uddi:ffe1fdca-d077-44f6-9f18-dc2e92a7340c?")
        conf = Config()
        serviceKey = conf.공공데이터()

        targets = [target_base + f"page={i + 1}&perPage=10&serviceKey={serviceKey}" for i in range(0, 10)]

        headers = {'User-Agent': ('Mozilla/5.0 (Windows NT 10.0;Win64; x64)\
                    AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98\
                    Safari/537.36')}

        단일데이터 = []

        for idx, target in enumerate(targets):
            if idx == 2:
                break
            res = requests.get(target, headers=headers)    
            jsonres = json.loads(res.text)

            if res.status_code == 200:
                try:
                    for i in jsonres["data"]:
                        단일데이터.append(i)

                except Exception as e:
                    print(e)
                    단일데이터.append("항목없음")
            else:
                print("서버로부터 정상응답을 못 받았습니다.")

        # <데이터 크롤링 및 리스트 저장>

        # #############################################

        # <DataFrame 만들기 / DB 연동 INSERT 진행>

        # pd.DataFrame() : [{},{},{},...] 리스트 딕셔너리 구조를 데이터프레임으로 변환하는 함수
        df = pd.DataFrame([단일데이터[i] for i in range(len(단일데이터))])
        print(df)
        mysql = Mysqldb("mysql+pymysql://{}:{}@{}/{}".format(*conf.디비연결정보()))

        df.to_sql(name="apitbl", index=False, if_exists="append", con=mysql.engine)

```

# Schedule 처리
원하는 시간마다 파이썬 작업을 자동실행하기 위한 라이브러리


## 이벤트에 따른 처리 모듈
이벤트 방식으로 스케줄을 만들어야 한다면 sched 모듈을 사용


## schedule 주기
```python
"""
schedule 주기
every().seconds.do()
every().minutes.do()
every().hour.do()
every().days.do()
every().seconds.do()
every().weeks.do()
every().day.at().do()
every().monday.at().do()
"""

```

## sch.py
```python
import schedule
import time


class Scheduler:
    def __init__(self, name, job):
        # 매 1초마다 job을 수행하세요 / 매일 job을 수행하세요
        schedule.every(1).seconds.do(job)

```

## runner.py
```python
from cafe.apidata import Crw
from helpers.sch import Scheduler

if __name__ == '__main__':

    def job_datakr():  # 공공데이터 크롤러
        crw = Crw("공공데이터")
        print("작업 수행")


    Scheduler("Crawling_Job", job_datakr)

    # 스케줄러
    while True:
        # run_pending() : 스케줄링된 작업들은 이 메소드가 호출될 때 실행된다.
        schedule.run_pending()
        time.sleep(3)

```

