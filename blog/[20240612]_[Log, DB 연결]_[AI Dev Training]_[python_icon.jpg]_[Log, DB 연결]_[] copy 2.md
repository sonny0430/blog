# Log Level 정리
Log : 로그는 동작에 대한 수행 과정 및 결과 등을 기록으로 남기는 것

| LOG LEVEL |  VALUE | WHEN TO USE                                                        |
| --------- | ------ | -------------------------------------------------------------------|
| DEBUG     |   10   | (주로 문제 해결을 할 때 필요한) 자세한 정보                           |
| INFO      |   20   | 작업이 정상적으로 작동하고 있다는 확인 메시지                          |
| WARNING   |   30   | 예상하치 못한 일이 발생 / 발생가능한 문제점 명시 / 작업은 정상 진행     |
| ERROR     |   40   | 프로그램이 함수를 실행하지 못할 정도의 심각한 문제                     |
| CRITICAL  |   50   | 프로그램이 동작할 수 없을 정도의 심각한 문제                           |


## logg.py
```python
import os
import logging
from contextlib import contextmanager
from db.dbconn import Dbwork
import pymysql
import pymysql.cursors


# 파일로거 클래스화
class Logwork:
    def __init__(self, name, level, file_name):
        self.logger = logging.getLogger(__name__)

        # 풀경로 = os.path.basename(file_name)
        self.file_name = os.path.basename(file_name)
        self.formatter = logging.Formatter(u'%(asctime)s [%(levelname)s] %(message)s')

        self.stream_handler = logging.FileHandler(self.file_name, encoding="utf-8")
        self.stream_handler.setFormatter(self.formatter)

        self.logger.addHandler(self.stream_handler)

        # 기본값이 INFO 레벨이므로, DEBUG 레벨 로그는 무시됨
        self.logger.setLevel(logging.INFO)

        if level == "debug":
            self.logger.setLevel(logging.DEBUG)

        elif level == "info":
            self.logger.setLevel(logging.INFO)

        elif level == "warning":
            self.logger.setLevel(logging.WARNING)

        elif level == "error":
            self.logger.setLevel(logging.ERROR)

        elif level == "critical":
            self.logger.setLevel(logging.CRITICAL)

    def info(self, value):
        self.logger.info("%s (at %s)" % (str(value), self.file_name))

    def error(self, value):
        self.logger.error("%s (at %s)" % (str(value), self.file_name))

    # 제너레이터 함수
    @contextmanager
    def debug_context(self):
        level = self.logger.level
        try:
            # 로깅 레벨을 변경함
            self.logger.setLevel(logging.DEBUG)
            yield
        finally:
            # 원래 로깅 레벨로 되돌림
            self.logger.setLevel(level)


```

## runner.py
```python
from helpers.logg import *

now = datetime.datetime.now()

if __name__ == '__main__':
    logger01 = Logwork("logger01", "debug", "applog.log")
    logger01.info('프로그램이 작동을 시작했습니다.')
    
    파일명 = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")

    logger01.info('프로그램이 작동이 정상적으로 종료되었습니다.')

```


# DB 연결하기
데이터베이스 (Database, DB) 

=> 데이터의 저장소

데이터베이스 관리 시스템 (Database Managemnet System)

=> 데이터베이스를 운영하고 관리하는 소프트웨어

=> 대부분 테이블로 구성된 관계형 DBMS형태로 사용

SQL (Structured Query Language)

=> 구조화된 질의 언어, 관계형 데이터베이스에서 사용되는 언어


## SQL 언어의 큰 범주
1) DDL (Data Definition Language) 데이터 정의어

2) DML (Data Manipulation Language) 데이터 조작어

3) DCL (Data Control Language) 데이터 제어어


## 사용할 메소드

1) pymysql.Connection()
=> DB에 연결

2) .cursor(pymysql.cursors.DictCursor)

=> SQL문 실행하는 작업환경 제공 (DB와 상호작용)

=> DictCursor를 활용해 데이터 분석에 용이한 딕셔너리로 반환

3) .excute()

=> 쿼리문을 실행

4) .commit()

=> DB에 반영

5) .fetchall() : 전부 출력

6) .fetchone() : 한 줄씩 출력

7) .fetchmany(size) : 원하는 만큼 출력


## 준비사항
Laragon 설치 및 DB와 Table 생성


## dbconn.py
```python
from helpers.debug import *
import pymysql  # MySQL과 연동 및 상호작용하는 라이브러리
import pymysql.cursors


class Dbwork:
    def __init__(self, user, password, host, database):
        self.conn = pymysql.Connection(
            user=user,
            password=password,
            host="127.0.0.1",
            database=database
        )
        print(self.conn)
        print("잘 연결됨")

    def __str__(self):
        return "연결됨"

    def df_insert(self, 단일데이터, created_at):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"INSERT INTO `onoffmix`.`apitbl` (`단일데이터`, `created_at`) VALUES ('{단일데이터}', '{created_at}');"
        cur.execute(query)
        # print(cur._last_e)
        self.conn.commit()


    # 테이블에 데이터행 추가
    def create_insert(self, level, message, created_at):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"INSERT INTO `logdb`.`logtb` (`level`, `message`, `created_at`) VALUES ('{level}', '{message}', '{created_at}');"
        cur.execute(query)
        # print(cur._last_e)
        self.conn.commit()

    # 테이블 데이터 조회
    def read_select(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = "SELECT * FROM `logdb`.`logtb` LIMIT 1000;"
        cur.execute(query)
        # pp(cur.excute(query))
        # 보안을 이유로 정보를 많이 주지 않음 (default)

        datas = cur.fetchall()  # [{}, {}, ...] 형태
        print(datas[-1])
        print(datas[-1]['id'])

    # 조건에 맞는 데이터행 변경
    # WHERE : 조건 => 필수! / DB 수정은 매우 신중하게 이루어져야 한다.
    def update_update(self, updated_at):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"UPDATE `logdb`.`logtb` SET `updated_at`='{updated_at}' WHERE  `id`=5;"
        cur.execute(query)
        self.conn.commit()

    # 조건에 맞는 데이터행 삭제
    def delete_delete(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"DELETE FROM `logdb`.`logtb` WHERE  `id`=12;"
        cur.execute(query)
        self.conn.commit()

    # 마지막 idx 조회 후 해당 데이터행 삭제
    def delete_last_row(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = "SELECT * FROM `logdb`.`logtb` LIMIT 1000;"
        cur.execute(query)

        datas = cur.fetchall()  # [{}, {}, ...] 형태
        query = f"DELETE FROM `logdb`.`logtb` WHERE  `id`={datas[-1]['id']};"
        cur.execute(query)

        self.conn.commit()
        print("한줄 삭제 완료")

    def close(self):
        self.conn.close()
        pass

```

## runner.py
```python
if __name__ == '__main__':
    try:
        db01 = Dbwork("user", "1234", "localhost", "logdb")
        
        db01.create_insert("info", '체크', now)
        db01.read_select()
        db01.delete_delete()

        db01.update_update(now)
        db01.delete_last_row()

        db01.close()

    except Exception as e:
        print(e)

```



# Log와 DB연결
```python
# M R O => Method Resolution Order => __mro__
class Logworkdb(Dbwork, Logwork):
    def __init__(self):
        con_info = {
            "user": "user",
            "password": "1234",
            "host": "localhost",
            "database": "logdb"}

        log_info = ["db로깅", "info", "db로깅앱.log"]

        # 상속
        Dbwork.__init__(self, **con_info)
        Logwork.__init__(self, *log_info)
        # super(클래스명, self).__init__()
        # super().__init__()

    # 오버라이드 (재정의) => # DB에 정보를 INSERT 할 것
    def info(self, level, message, created_at):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"INSERT INTO `logdb`.`logtb` (`level`, `message`, `created_at`) VALUES ('{level}', '{message}', '{created_at}');"
        cur.execute(query)
        # print(cur._last_e)
        self.conn.commit()

    # def error(self, level, message, created_at):
    #     cur = self.conn.cursor(pymysql.cursors.DictCursor)
    #     query = f"INSERT INTO `logdb`.`logtb` (`level`, `message`, `created_at`) VALUES ('{level}', '{message}', '{created_at}');"
    #     cur.execute(query)
    #     # print(cur._last_e)
    #     self.conn.commit()

```

## runner.py
```python
from db.dbconn import *

if __name__ == '__main__':

    try:
        logger02 = Logworkdb()
        logger02.info("info", "DB에 log 남김", now)
        logger02.info("error", "error를 DB에 log 남김", now)

    except Exception as e:
        print(e)


```