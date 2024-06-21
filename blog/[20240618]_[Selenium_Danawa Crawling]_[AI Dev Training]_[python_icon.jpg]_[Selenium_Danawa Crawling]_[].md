# Selenium
selenium 이란?

=> 웹 애플리케이션 테스트 자동화를 위해 개발된 오픈 소스 프레임워크

=> 웹 브라우저를 제어하여 웹 페이지 상에서 동작하는 테스트를 자동화 가능

=>  파이썬에서는 Selenium을 사용하여 웹 스크래핑, UI 테스트, 웹 테스트 자동화 등 다양한 작업을 수행 가능


## Selenium 특징
=> 속도가 느리지만, 동적인 사이트도 크롤링이 가능


## 진행할 동적크롤링 과정
```python
"""
1. 다나와 사이트 접속
2. 맥북 검색
3. 리뷰 많은 순 정렬
4. 최상단 제품 접속
5. 의견/리뷰 클릭
6. 각 리뷰 크롤링
"""
```

## 크롬 드라이버
```python
"""
본인의 크롬 버전을 확인하고
버전에 맞는 드라이버 다운로드

# https://googlechromelabs.github.io/chrome-for-testing/#stable
"""
```

## Selenium 활용한 2가지 방식
```python
"""
### 2가지 방식
1. 이미 떠 있는 브라우저 제어 
2. 신규로 브라우저 실행 후 제어 (else)
# 기존에 실행중이던 브라우저는 디버그모드로 실행되었어야함
# 디버그 모드로 실행된 포트를 확인해야함 (중요!)
# => 수동으로 브라우저 작동시키기 (subprocess 활용)
"""
```


## sele.py
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service

from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait


import subprocess
from urllib.parse import quote_plus
import os
import signal

import time

CHROME_DRIVER = r"C:\workspace2\pythonProject\cafe\driver\chromedriver.exe"

class User:
    # self.chrome_options.add_argument("--headless") # 브라우저가 화면에 나타나지 않음
    # self.chrome_options.add_argument("--no-sandbox") # 
    # self.chrome_options.add_argument("--disable-dev-shm-usage")
    
    def __init__(self, mode="new"):
        self.chrome_driver = CHROME_DRIVER
        self.chrome_options = Options()
        self.mode = mode
        
        if self.mode == "new":
            self.browser = webdriver.Chrome(service=Service(self.chrome_driver), options=self.chrome_options)
            self.browser.maximize_window()
            
        else:
            pass
 

    # find_element() : 필요한 요소를 찾아서 특정
    # send_keys() : 검색할 문자열 전송
    
    def page_main(self, page):
        self.browser.get(page)
        
        
    def search_box_click(self):
        self.browser.find_element(By.XPATH, '//*[@id="AKCSearch"]').click()
        print("검색창 클릭완료")
        
            
    def keyword_send(self, keyword):
        self.browser.find_element(By.XPATH, '//*[@id="AKCSearch"]').send_keys(keyword)
        print("keyword 입력완료")
        
        
    def search_click(self):
        self.browser.find_element(By.XPATH, '//*[@id="srchFRM_TOP"]/fieldset/div[1]/button[2]').click()
        print("검색완료")
        
        
    def object_click(self, xpath):
        self.browser.find_element(By.XPATH, xpath).click()
        print("객체 클릭 성공")
        
        

    def review_points(self):
        points = []
        for i in range(1, 11):
            points.append(self.browser.find_element(By.XPATH, f'//*[@id="danawa-prodBlog-companyReview-content-list"]/ul/li[{i}]/div[1]/span[1]').text)
        print("리뷰별점을 불러왔습니다.")
        return points
    
    
    def review_dates(self):
        dates = []
        for i in range(1, 11):
            dates.append(self.browser.find_element(By.XPATH, f'//*[@id="danawa-prodBlog-companyReview-content-list"]/ul/li[{i}]/div[1]/span[5]').text)
        print("리뷰날짜를 불러왔습니다.")
        return dates


    def review_titles(self):
        titles = []
        for i in range(10):
            titles.append(self.browser.find_element(By.XPATH, f'//*[@id="danawa-prodBlog-companyReview-content-wrap-{i}"]/div/div[1]/p').text)
        print("리뷰제목을 불러왔습니다.")
        return titles


    def review_contexts(self):
        contexts = []
        for i in range(10):
            cont = (self.browser.find_element(By.XPATH, f'//*[@id="danawa-prodBlog-companyReview-content-wrap-{i}"]').text)
            contexts.append(cont[(cont.find('\n'))+1:])
        print("리뷰내용을 불러왔습니다.")
        return contexts
    
    
    
    def paging(self, full_xpath):
        self.browser.find_element(By.XPATH, full_xpath).click()
        print("페이징되었습니다.")
        
        
        
    def new_tab_activate(self, idx):
        self.browser.switch_to.window(self.browser.window_handles[idx])
        print("새로운 탭 중심 활성화")
        
    def page_down(self, cnt):
        for _ in range(cnt):
            self.browser.find_element(By.CSS_SELECTOR, 'body').send_keys(Keys.PAGE_DOWN)
    
                      
    def browser_close(self):
        self.browser.close()    
    
    
```


## runner.py
```python
from cafe.sele import User
import time
import pandas as pd


def selenium_run():
    user = User()
    user.page_main("https://www.danawa.com/")    
    time.sleep(3)
        
    user.search_box_click()
    time.sleep(3)
    
    user.keyword_send("맥북")
    time.sleep(3)
    
    user.search_click()
    time.sleep(3)
    
    user.object_click('//*[@id="opinionDESC"]/a')
    time.sleep(3)
    
    user.object_click('//*[@id="productItem12660491"]/div/div[2]/p/a')
    time.sleep(3)
    
    user.new_tab_activate(1)
    time.sleep(3)
    
    user.object_click('//*[@id="bookmark_cm_opinion_item"]/a')
    time.sleep(3)
    
    user.object_click('//*[@id="danawa-prodBlog-productOpinion-button-tab-companyReview"]')
    time.sleep(3)

    상품명 = []
    별점 = []
    작성날짜 = []
    상품평제목 = []
    상품평내용 = []
    for p in range(1, 4):
        try:
            별점.extend(user.review_points())
            time.sleep(3)
            
            작성날짜.extend(user.review_dates())
            time.sleep(3)
                       
            상품평제목.extend(user.review_titles())
            time.sleep(3)
            
            상품평내용.extend(user.review_contexts())
            time.sleep(3)

            user.paging(f'/html/body/div[2]/div[5]/div[2]/div[4]/div[4]/div/div[3]/div[2]/div[3]/div[2]/div[5]/div/div/div/a[{p}]')
            time.sleep(5)


        except Exception as e:
            print(f'문제 발생 / {p}페이지')

    
    상품명 = ["맥북"] * len(상품평제목)

    df = pd.DataFrame(data=zip(상품명, 별점, 작성날짜, 상품평제목, 상품평내용), columns=["product", "point", "date", "title", "context"])
    print(df)
    df.to_excel(r"C:\workspace2\pythonProject\data\crawling_data\danawa_data0.xlsx", engine="openpyxl")

    
if __name__ == '__main__':
    selenium_run()
    
```