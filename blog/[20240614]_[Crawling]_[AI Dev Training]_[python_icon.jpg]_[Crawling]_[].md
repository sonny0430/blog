# Crawling (크롤링)
Crawling

=> 웹 페이지를 그대로 가져와서 거기서 데이터를 추출해 내는 행위

Crawler

=> 크롤링하는 소프트웨어


## Selector 복사방법
```python
# 개발자도구 => 검색할 요소 선택 (좌상단 메뉴) => 
# 웹사이트에서 selector가 궁금한 곳 클릭 => 선택된 HTML 코드에서 우클릭 => 
# 복사 => selector 복사
```


## Parsing 
```python
# Parsing
# => 문자의 구조를 분석해서 원하는 정보를 얻어내는 것
# => 웹페이지에서 원하는 데이터를 추출해서 다루기 쉬운 형태로 바꾸는 것.
# "JSON / XML / HTML" => "Parser" => "Object / XML Docu / Array"
```


## Header를 태우는 이유
```python
# header를 태우지 않으면, 기계적 크롤링 인식이 되어서 return이 안올 수도 있음
# => response [200] 이 아니면, header를 붙여야한다.
# => https://www.urldecoder.org/ : URL Decode 사이트
# status code sheet : https://javaconceptoftheday.com/http-status-codes-cheat-sheet/
```

## 크롤링을 진행할 사이트
=> https://www.onoffmix.com/event/main/?c=105


## crawling.py
이전에 만든 dbconn_orm.py, conf.py의 클래스 활용

```python
# beautufulsoup4 / requests / pandas / openpyxl / 스케줄러 -> 설치
import requests      # python용 HTTP 라이브러리
from bs4 import BeautifulSoup  # HTML Parsing 라이브러리
import pandas as pd  # 데이터 구조화 / 조작 / 분석
import sys           # System 관련 / 인터프리터와 상호작용

from db.dbconn_orm import Mysqldb
from helpers.conf import Config

# onoffmix 웹사이트 크롤링 / DataFrame 및 DB Insert 확인하기
class Crawler:
    def __init__(self, file_name):
        target_base = "https://onoffmix.com/event/main/?c=105"
        # targets = [target_base for i in range(0, 1)]
        headers = {'User-Agent': ('Mozilla/5.0 (Windows NT 10.0;Win64; x64)\
                    AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98\
                    Safari/537.36')}

        이벤트명 = []
        유료무료 = []
        기간 = []

        # for target in targets:
        res = requests.get(target_base, headers=headers)
        # print(res)

        # status_code (응답코드)가 200이면 => 정상
        # soup.select() -> HTML 문서 구조를 알아야 이해가능    
        if res.status_code == 200:
            try:
                soup = BeautifulSoup(res.text, 'html.parser')
                for i in range(0, 10):
                    이벤트명.append(soup.select(f"#content > div > section.event_main_area > ul > li:nth-child({i + 1}) > article.event_area.event_main > a > div.event_info_area > div.title_area > h5")[0].text.strip())
                    유료무료.append(soup.select(f"#content > div > section.event_main_area > ul > li:nth-child({i + 1}) > article.event_area.event_main > a > div.event_info_area > div.event_info > div.type_info > span.payment_type.free")[0].text)
                    기간.append(soup.select(f"#content > div > section.event_main_area > ul > li:nth-child({i + 1}) > article.event_area.event_main > a > div.event_info_area > div.event_info > div.date")[0].text)

            except Exception as e:
                print(e)

        else:
            print("서버로부터 정상응답을 못 받았습니다.")

        # <데이터 크롤링 및 리스트 저장>

        # ########################################################################
        
        # <DataFrame 만들기 / DB 연동 INSERT 진행>

        # DataFrame => numpy / pandas 배울 때 자세히 다룰 예정
        df = pd.DataFrame(data=zip(이벤트명, 유료무료, 기간), columns=["이벤트명", "유료무료", "기간"])
        print(df)

        conf = Config()
        # mysql = Mysqldb(f'mysql+pymysql://{"user"}:{"1234"}@{"localhost"}/{"onoffmix"}')
        mysql = Mysqldb("mysql+pymysql://{}:{}@{}/{}".format(*conf.디비연결정보()))

        df.to_sql(name="onoffmixtbl", index=False, if_exists="append", con=mysql.engine)
        df.to_excel(r"C:\workspace2\pythonProject\data\crawling_data\{}.xlsx".format(file_name), engine="openpyxl")
        print("엑셀로 반환")
```

## runner.py
```python
from cafe.crawling import Crawler

if __name__ == '__main__':
    파일명 = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")

    crawler = Crawler(파일명)
    print("작업을 수행합니다.")

```

