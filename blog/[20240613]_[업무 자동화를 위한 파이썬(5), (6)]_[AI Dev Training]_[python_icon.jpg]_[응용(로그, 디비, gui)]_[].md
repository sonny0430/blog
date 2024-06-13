# Python 응용
## Project 디렉토리
### runner.py
```python
from helpers.debug import *
from helpers.logg import *
from db.dbconn import *
import datetime
from ui.ui2 import Gui
from helpers.conf import Config
import sys

now = datetime.datetime.now()

if __name__ == '__main__':
    # GUI를 활용하는 클래스 불러오기
    # gui = Gui()

    # config.ini 파일을 읽어오는 클래스 불러오기
    # config = Config()
    # print(config.앱의이름())
    # sys.exit()

    logger01 = Logwork("logger01", "debug", "applog.log")
    logger01.info('프로그램이 작동을 시작했습니다.')

    try:
        db01 = Dbwork("user", "1234", "localhost", "logdb")
        logger02 = Logworkdb()

        logger01.info("DB 접속에 성공하였습니다.")
        # db01.create_insert("info", '체크', now)
        # db01.read_select()
        # db01.delete_delete()

        # db01.update_update(now)
        # db01.delete_last_row()

        # logger02.info("info", "DB에 log 남김", now)
        # logger02.info("error", "error를 DB에 log 남김", now)

        # logg.py에서 오버라이드 없이, 바로 메소드를 상속받아서 사용할 수도 있다.
        # logger02.create_insert(" info", "메소드 상속 테스트", now)


        db01.close()
        logger01.info('프로그램이 작동이 정상적으로 종료되었습니다.')

    except Exception as e:
        logger01.error("DB 접속에 문제가 생겼습니다.")


    # DEBUG 로그를 볼 때의 처리를 with 문 블록 안에서 실행함
    # with logger01.debug_context():
    #     logger01.info('inside the block: info log')
    #     logger01.error('inside the block: debug log')

    # logger01.info('after: info log')
    # logger01.error('after: debug log')

# DB
# primary 할당
# 이론적으로 NULL 허용은 안하는게 좋지만
# 문제를 덜 일으키기 위해 실습상 허용
# DB 사용자 관리자 => 호스트 % 설정 => 어디에서나 접근 가능

# pysimplegui 설치 => GUI 포장
# GUI 프레임워크인 Tkinter와 PyQT를 사용을 도와주는 패키지
# 4.60.5 => 구버전 => 무료

# auto-py-to-exe 설치
# 터미널 => auto-py-to-exe

# pip freeze > requirements.txt
# pip install -r requirements.txt

# DB작업 => ORM 중요
```

### helpers 디렉토리
1. __init__.py

2. debug.py

3. logg.py
```python
import os
import logging
from contextlib import contextmanager

from db.dbconn import Dbwork

import pymysql
import pymysql.cursors

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


# M R O => Method Resolution Order => __mro__
class Logworkdb(Dbwork, Logwork):
    def __init__(self):
        con_info = {
            "user" : "user",
            "password" : "1234",
            "host" : "localhost",
            "database" : "logdb"
        }

        log_info = ["db로깅", "info", "db로깅앱.log"]

        # super(클래스명, self).__init__()
        # super().__init__()

        Dbwork.__init__(self, **con_info)
        Logwork.__init__(self, *log_info)

    # 오버라이드 (재정의)
    def info(self, level, message, created_at):
        # DB에 정보를 인서트할 것
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"INSERT INTO `logdb`.`logtb` (`level`, `message`, `created_at`) VALUES ('{level}', '{message}', '{created_at}');"
        cur.execute(query)
        # print(cur._last_e)
        self.conn.commit()



    def error(self, level, message, created_at):
        # DB에 정보를 인서트할 것
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"INSERT INTO `logdb`.`logtb` (`level`, `message`, `created_at`) VALUES ('{level}', '{message}', '{created_at}');"
        cur.execute(query)
        # print(cur._last_e)
        self.conn.commit()

# JSON / XML / YAML / TOML

# DB를 직접 설치 => installer
# wapm => 아파치, php, mysql
# lamp
# 라라곤 bitnami xampp

```

4. conf.py
```python
import configparser
import os

class Config:
    def __init__(self):
        경로 = os.path.basename('config.ini')
        self.conf_file = 경로
        self.config = configparser.ConfigParser()
        self.config.read(self.conf_file, encoding='UTF-8')

        앱의이름 = self.config['앱']['이름']



    def 앱의이름(self):
        return self.config['앱']['이름']

```

### db 디렉토리
1. __init__.py

2. dbconn.py
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
        print("잘 연결됨")

    def __str__(self):
        return "연결됨"
        # 비접속성공여부(self):


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
        # pp(result)
        # 보안을 이유로 정보를 많이 주지 않음 (default)

        # .fetchall() : 전부 출력
        # .fetchone() : 한 줄씩 출력
        # .fetchmany(size) : 원하는 만큼 출력
        datas = cur.fetchall() # [{}, {}, ...] 형태
        # for i in datas:
        #     pp(i)
        print(datas[-1])
        print(datas[-1]['id'])

    def update_update(self, updated_at):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        # WHERE : 조건 => 필수! / DB 수정은 매우 신중하게 이루어져야 한다.
        query = f"UPDATE `logdb`.`logtb` SET `updated_at`='{updated_at}' WHERE  `id`=5;"
        cur.execute(query)
        self.conn.commit()

    def delete_delete(self):
        cur = self.conn.cursor(pymysql.cursors.DictCursor)
        query = f"DELETE FROM `logdb`.`logtb` WHERE  `id`=12;"
        cur.execute(query)
        self.conn.commit()

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
3. dbconn_orm.py
```python
# ORM => sqlalchemy 많이 사용
# django orm

# SQLAlchemy 설치

# 바다코끼리 연산자 (:=)
# 변수 := 값
# 표현식의 결과를 변수에 할당하고 동시에 반환

# https://docs.sqlalchemy.org/en/20/orm/quickstart.html

from typing import List
from typing import Optional

from sqlalchemy import ForeignKey
from sqlalchemy import String, Integer, TIMESTAMP

from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column
from sqlalchemy.orm import relationship

from sqlalchemy import Column


class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "user_account"
    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(30))
    fullname: Mapped[Optional[str]] = mapped_column(String(30))
    addresses: Mapped[List["Address"]] = relationship(
        back_populates="user", cascade="all, delete-orphan"
    )
    def __repr__(self) -> str:
        return f"User(id={self.id!r}, name={self.name!r}, fullname={self.fullname!r})"

class Address(Base):
    __tablename__ = "address"
    id: Mapped[int] = mapped_column(primary_key=True)
    email_address: Mapped[str] = mapped_column(String(30))
    user_id: Mapped[int] = mapped_column(ForeignKey("user_account.id"))
    user: Mapped["User"] = relationship(back_populates="addresses")
    def __repr__(self) -> str:
        return f"Address(id={self.id!r}, email_address={self.email_address!r})"


from sqlalchemy import create_engine

engine = create_engine("mysql+pymysql://user:1234@localhost/logdb", echo=True)
print(engine)

Base.metadata.create_all(engine)


from sqlalchemy.orm import Session

with Session(engine) as session:
    spongebob = User(
        name="spongebob",
        fullname="Spongebob Squarepants",
        addresses=[Address(email_address="spongebob@sqlalchemy.org")],
    )
    sandy = User(
        name="sandy",
        fullname="Sandy Cheeks",
        addresses=[
            Address(email_address="sandy@sqlalchemy.org"),
            Address(email_address="sandy@squirrelpower.org"),
        ],
    )
    patrick = User(name="patrick", fullname="Patrick Star")
    session.add_all([spongebob, sandy, patrick])
    session.commit()



    from sqlalchemy import select

    session = Session(engine)

    stmt = select(User).where(User.name.in_(["spongebob", "sandy"]))

    for user in session.scalars(stmt):
        print(user)

# stmt = (
#     select(Address)
#     .join(Address.user)
#     .where(User.name == "sandy")
#     .where(Address.email_address == "sandy@sqlalchemy.org")
# )
# sandy_address = session.scalars(stmt).one()
# sandy_address

# with Session(engine) as session:
#     stmt = select(User).where(User.name == "patrick")
#     patrick = session.scalars(stmt).one()
#     patrick.addresses.append(Address(email_address="patrickstar@sqlalchemy.org"))
#     sandy_address.email_address = "sandy_cheeks@sqlalchemy.org"
#
#     session.commit()
#
#     sandy = session.get(User, 2)
#     sandy.addresses.remove(sandy_address)
#     session.flush()
#     session.commit()

# class Logtable(Base):
#     __tablename__ = "logtb"
#     id = Column(Integer())
#     level = Column(String())
#     message = Column(String())
#     created_at = Column(TIMESTAMP())
#     updated_at = Column(TIMESTAMP())
#
#     def __repr__(self) -> str:
#         return f"Logtable(id={self.id}, level={self.level}, message={self.message})"

```

### ui 디렉토리
1. __init__.py

2. ui.py
```python
#!/usr/bin/env python

import PySimpleGUI as sg
import sys

class Gui:
    def __init__(self):
        pass

    def make_window(self, theme):
        sg.theme(theme)
        menu_def = [['파일', ['종료']],
                    ['도움말', ['&소개']]]
        right_click_menu_def = [[], ['편집', 'Versions', 'Nothing', 'More Nothing', 'Exit']]
        graph_right_click_menu_def = [[], ['Erase', 'Draw Line', 'Draw', ['Circle', 'Rectangle', 'Image'], 'Exit']]

        # Table Data
        # 앱에서 날씨정보를 json => 리스트 객체로 바꿔서 사용
        data = [["John", 10], ["Jen", 5]]
        headings = ["Name", "Score"]

        input_layout = [

            # [sg.Menu(menu_def, key='-MENU-')],
            [sg.Text('파이썬 GUI 프로그래밍')],
            [sg.Input(key='-INPUT-')],
            [sg.Slider(orientation='h', key='-SKIDER-'),
             sg.Image(data=sg.DEFAULT_BASE64_LOADING_GIF, enable_events=True, key='-GIF-IMAGE-'), ],
            [sg.Checkbox('Checkbox', default=True, k='-CB-')],
            [sg.Radio('Radio1', "RadioDemo", default=True, size=(10, 1), k='-R1-'),
             sg.Radio('Radio2', "RadioDemo", default=True, size=(10, 1), k='-R2-')],
            [sg.Combo(values=('Combo 1', 'Combo 2', 'Combo 3'), default_value='Combo 1', readonly=False, k='-COMBO-'),
             sg.OptionMenu(values=('Option 1', 'Option 2', 'Option 3'), k='-OPTION MENU-'), ],
            [sg.Spin([i for i in range(1, 11)], initial_value=10, k='-SPIN-'), sg.Text('Spin')],
            [sg.Multiline(
                'Demo of a Multi-Line Text Element!\nLine 2\nLine 3\nLine 4\nLine 5\nLine 6\nLine 7\nYou get the point.',
                size=(45, 5), expand_x=True, expand_y=True, k='-MLINE-')],
            [sg.Button('Button'), sg.Button('Popup'), sg.Button(image_data=sg.DEFAULT_BASE64_ICON, key='-LOGO-')]]

        asthetic_layout = [[sg.T('Anything that you would use for asthetics is in this tab!')],
                           [sg.Image(data=sg.DEFAULT_BASE64_ICON, k='-IMAGE-')],
                           [sg.ProgressBar(100, orientation='h', size=(20, 20), key='-PROGRESS BAR-'),
                            sg.Button('Test Progress bar')]]

        logging_layout = [[sg.Text("Anything printed will display here!")],
                          [sg.Multiline(size=(60, 15), font='Courier 8', expand_x=True, expand_y=True, write_only=True,
                                        reroute_stdout=True, reroute_stderr=True, echo_stdout_stderr=True, autoscroll=True,
                                        auto_refresh=True)]
                          # [sg.Output(size=(60,15), font='Courier 8', expand_x=True, expand_y=True)]
                          ]

        graphing_layout = [[sg.Text("Anything you would use to graph will display here!")],
                           [sg.Graph((200, 200), (0, 0), (200, 200), background_color="black", key='-GRAPH-',
                                     enable_events=True,
                                     right_click_menu=graph_right_click_menu_def)],
                           [sg.T('Click anywhere on graph to draw a circle')],
                           [sg.Table(values=data, headings=headings, max_col_width=25,
                                     background_color='black',
                                     auto_size_columns=True,
                                     display_row_numbers=True,
                                     justification='right',
                                     num_rows=2,
                                     alternating_row_color='black',
                                     key='-TABLE-',
                                     row_height=25)]]

        popup_layout = [[sg.Text("Popup Testing")],
                        [sg.Button("Open Folder")],
                        [sg.Button("Open File")]]

        theme_layout = [[sg.Text("See how elements look under different themes by choosing a different theme here!")],
                        [sg.Listbox(values=sg.theme_list(),
                                    size=(20, 12),
                                    key='-THEME LISTBOX-',
                                    enable_events=True)],
                        [sg.Button("Set Theme")]]

        layout = [[sg.MenubarCustom(menu_def, key='-MENU-', font='Courier 15', tearoff=True)],
                  [sg.Text('Demo Of (Almost) All Elements', size=(38, 1), justification='center', font=("Helvetica", 16),
                           relief=sg.RELIEF_RIDGE, k='-TEXT HEADING-', enable_events=True)]]
        layout += [[sg.TabGroup([[sg.Tab('Input Elements', input_layout),
                                  sg.Tab('Asthetic Elements', asthetic_layout),
                                  sg.Tab('Graphing', graphing_layout),
                                  sg.Tab('Popups', popup_layout),
                                  sg.Tab('Theming', theme_layout),
                                  sg.Tab('Output', logging_layout)]], key='-TAB GROUP-', expand_x=True, expand_y=True),

                    ]]
        layout[-1].append(sg.Sizegrip())
        window = sg.Window('All Elements Demo', layout, right_click_menu=right_click_menu_def,
                           right_click_menu_tearoff=True, grab_anywhere=True, resizable=True, margins=(0, 0),
                           use_custom_titlebar=True, finalize=True, keep_on_top=True)
        window.set_min_size(window.size)
        return window


    def main(self):
        window = self.make_window(sg.theme())

        # This is an Event Loop
        while True:
            event, values = window.read(timeout=100)
            # keep an animation running so show things are happening
            if event not in (sg.TIMEOUT_EVENT, sg.WIN_CLOSED):
                print('============ Event = ', event, ' ==============')
                print('-------- Values Dictionary (key=value) --------')
                for key in values:
                    print(key, ' = ', values[key])
            if event in (None, 'Exit'):
                print("[LOG] Clicked Exit!")
                break

            window['-GIF-IMAGE-'].update_animation(sg.DEFAULT_BASE64_LOADING_GIF, time_between_frames=100)
            if event == 'About':
                print("[LOG] Clicked About!")
                sg.popup('PySimpleGUI Demo All Elements',
                         'Right click anywhere to see right click menu',
                         'Visit each of the tabs to see available elements',
                         'Output of event and values can be see in Output tab',
                         'The event and values dictionary is printed after every event', keep_on_top=True)
            elif event == 'Popup':
                print("[LOG] Clicked Popup Button!")
                sg.popup("You pressed a button!", keep_on_top=True)
                print("[LOG] Dismissing Popup!")
            elif event == 'Test Progress bar':
                print("[LOG] Clicked Test Progress Bar!")
                progress_bar = window['-PROGRESS BAR-']
                for i in range(100):
                    print("[LOG] Updating progress bar by 1 step (" + str(i) + ")")
                    progress_bar.update(current_count=i + 1)
                print("[LOG] Progress bar complete!")
            elif event == "-GRAPH-":
                graph = window['-GRAPH-']  # type: sg.Graph
                graph.draw_circle(values['-GRAPH-'], fill_color='yellow', radius=20)
                print("[LOG] Circle drawn at: " + str(values['-GRAPH-']))
            elif event == "Open Folder":
                print("[LOG] Clicked Open Folder!")
                folder_or_file = sg.popup_get_folder('Choose your folder', keep_on_top=True)
                sg.popup("You chose: " + str(folder_or_file), keep_on_top=True)
                print("[LOG] User chose folder: " + str(folder_or_file))
            elif event == "Open File":
                print("[LOG] Clicked Open File!")
                folder_or_file = sg.popup_get_file('Choose your file', keep_on_top=True)
                sg.popup("You chose: " + str(folder_or_file), keep_on_top=True)
                print("[LOG] User chose file: " + str(folder_or_file))
            elif event == "Set Theme":
                print("[LOG] Clicked Set Theme!")
                theme_chosen = values['-THEME LISTBOX-'][0]
                print("[LOG] User Chose Theme: " + str(theme_chosen))
                window.close()
                window = self.make_window(theme_chosen)
            elif event == 'Edit Me':
                sg.execute_editor(__file__)
            elif event == 'Versions':
                sg.popup_scrolled(__file__, sg.get_versions(), keep_on_top=True, non_blocking=True)

        window.close()
        # exit(0)
        # exit()
        # sys.exit()
```
