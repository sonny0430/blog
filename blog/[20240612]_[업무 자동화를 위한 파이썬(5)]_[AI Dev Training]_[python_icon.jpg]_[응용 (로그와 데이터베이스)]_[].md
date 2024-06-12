# Python 응용

## Log => logg.py
```python
import os
import logging
from contextlib import contextmanager

from db.dbconn import Dbwork

# 이제부터는 함수화보다 클래스화 생각 => 클래스화 / 함수화 : 재사용성 향상
# 한번 잘 만들어두면 평생 사용해도 되는 로그 코드

# 파일로거 클래스화

class Logwork:
    # 생성자
    def __init__(self, name, level, file_name):
        # 인스턴스 변수
        self.logger = logging.getLogger(__name__)
        # 풀경로 = os.path.basename(file_name)
        self.file_name = os.path.basename(file_name)
        self.formatter = logging.Formatter(
            u'%(asctime)s [%(levelname)s] %(message)s'
        )
        self.stream_handler = logging.FileHandler(
            self.file_name, encoding="utf-8"
        )
        self.stream_handler.setFormatter(self.formatter)

        # self.logger.addHandler(logging.StreamHandler())
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


    @contextmanager
    # 제너레이터 함수
    def debug_context(self):
        level = self.logger.level
        try:
            # 로깅 레벨을 변경함
            self.logger.setLevel(logging.DEBUG)
            yield
        finally:
            # 원래 로깅 레벨로 되돌림
            self.logger.setLevel(level)

    def info(self, value):
        self.logger.info("%s (at %s)" % (str(value), self.file_name))

    def error(self, value):
        self.logger.error("%s (at %s)" % (str(value), self.file_name))

```


## DB 연동 (laragon 통한 확인) => dbconn.py

```python

# 외부모듈 pymysql 설치
from helpers.debug import *

import pymysql
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

    def create_insert(self, level, message, created_at):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"INSERT INTO `logdb`.`logtb` (`level`, `message`, `created_at`) VALUES ('{level}', '{message}', '{created_at}');"
        cur.execute(query)
        # print(cur._last_e)
        self.conn.commit()

    def read_select(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = "SELECT * FROM `logdb`.`logtb` LIMIT 1000;"
        result = cur.execute(query)
        pp(result)
        # 보안을 이유로 정보를 많이 주지 않음 (default)

        # .fetchall() : 전부 출력
        # .fetchone() : 한 줄씩 출력
        # .fetchmany(size) : 원하는 만큼 출력
        datas = cur.fetchall()
        for i in datas:
            print(i)

    def update_update(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        # WHERE : 조건 => 필수! / DB 수정은 매우 신중하게 이루어져야 한다.
        query = "UPDATE `logdb`.`logtb` SET `updated_at`='2024-06-12 15:45:00' WHERE  `id`=5;"
        cur.execute(query)
        self.conn.commit()

    def delete_delete_last_row(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"DELETE FROM `logdb`.`logtb` WHERE  `id`=15;"
        cur.execute(query)
        self.conn.commit()

    def close(self):
        self.conn.close()
        pass

```


## runner.py
```python

from helpers.debug import *

from helpers.logg import *
from db.dbconn import *

import datetime

now = datetime.datetime.now()

if __name__ == '__main__':
    # 인스턴스화
    logger01 = Logwork("logger01", "debug", "applog.log")
    # logger02 = Logwork("logger02", "critical")

    logger01.info('프로그램이 작동을 시작했습니다.')

    try:
        db01 = Dbwork("user", "1234", "localhost", "logdb")
        logger01.info("DB 접속에 성공하였습니다.")
        #db01.create_insert("info", '체크', now)
        #db01.read_select()
        #db01.delete_delete()
        db01.update_update()
        db01.delete_delete_last_row()

        db01.close()
        logger01.info('프로그램이 작동이 정상적으로 종료되었습니다.')

    # 192.168.41.120
    # 192.168.0.163
    except Exception as e:
        logger01.error("DB 접속에 문제가 생겼습니다.")


    # DEBUG 로그를 볼 때의 처리를 with 문 블록 안에서 실행함
    # with logger01.debug_context():
    #     logger01.info('inside the block: info log')
    #     logger01.error('inside the block: debug log')

    # logger01.info('after: info log')
    # logger01.error('after: debug log')
```
