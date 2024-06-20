# DB ORM과 Config의 활용은 Crawling 파트에서 진행
DB ORM과 Config의 활용은 Crawling 파트에서 진행

# DB ORM
ORM 이란?

=> Object Relational Mapping의 약자로, 객체와 데이터베이스의 관계를 매핑해주는 것

## dbconn_orm.py
```python
# DB Column 데이터 타입 정리
from sqlalchemy import (engine,
                        create_engine,  # DB와 연결할 엔진 생성
                        DateTime,
                        Column,
                        Integer,
                        String,
                        Enum)

from sqlalchemy.ext.declarative import declarative_base # DB구조와 코드상 구조 연결..??
from sqlalchemy.orm import sessionmaker                 # DB와 대화하기 위해 생성해야함..??
from sqlalchemy.sql import func

Base = declarative_base()

class onoffmixtbl(Base):
    __tablename__ = "onoffmixtbl"
    id = Column(Integer, primary_key=True)
    이벤트명 = Column(String)
    유료무료 = Column(String)
    기간 = Column(String)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), onupdate=func.now())


class Mysqldb:
    def __init__(self, db_url):
        self.engine = create_engine(db_url)
        self.session = sessionmaker(bind=self.engine)
        # self.session.configure(bind=self.engine)

```


# config 파일
config.ini 파일 사용하는 이유

=> ini 파일을 통해 config 정보를 한곳에서 관리함으로써 관리 포인트를 줄이고 유지보수 용이성을 높일 수 있다.

=> 예를 들어 DB 접근 시 필요한 IP, PORT, USERNAME, PASSWORD 등을 ini 파일에 저장해두고 이를 꺼내 사용할 수 있다. 


## config.ini
```ini
[DB정보]
user=user
password=1234
url=localhost
db=logtbl
port=3306

```

## conf.py
```python
class Config:
    def __init__(self):
        경로 = os.path.basename('config.ini')
        self.conf_file = 경로
        self.config = configparser.ConfigParser()
        self.config.read(self.conf_file, encoding='UTF-8')
    
    def 디비연결정보(self):
        db_info_tuple = {
            "user": self.config['DB정보']['user'],
            "password": self.config['DB정보']['password'],
            "url": self.config['DB정보']['url'],
            "db": self.config['DB정보']['db']
        }
        return db_info_tuple.values()
```

