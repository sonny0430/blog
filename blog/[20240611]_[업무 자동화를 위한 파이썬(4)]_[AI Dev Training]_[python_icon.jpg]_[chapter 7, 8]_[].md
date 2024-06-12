# chapter 7 (모듈)
## 표준모듈
각 모듈의 주요 함수 일부 정리

### math
수학적 계산을 윟나 다양한 함수와 상수들을 제공

```python
# math.sqrt(x): 제곱근을 구합니다.
# math.pow(x, y): x의 y제곱을 구합니다.
# math.exp(x): e의 x승을 구합니다.
# math.log(x, base): 로그 값을 구합니다. base를 지정하지 않으면 자연로그를 계산합니다.

# math.sin(x), math.cos(x), math.tan(x): 사인, 코사인, 탄젠트 값을 구합니다.
# math.radians(degrees): 각도를 라디안으로 변환합니다.
# math.degrees(radians): 라디안을 각도로 변환합니다.

# math.ceil(x): 올림 값을 구합니다.
# math.floor(x): 내림 값을 구합니다.
# math.trunc(x): 소수점 아래를 버린 값을 구합니다.

# math.factorial(x): x의 팩토리얼을 구합니다.
# math.gcd(a, b): a와 b의 최대공약수를 구합니다.
# math.isqrt(n): n의 제곱근을 구하고 정수 부분을 반환합니다.

# math.pi: 원주율(pi) 값을 반환합니다.
# math.e: 자연상수 e 값을 반환합니다.
```

### random
난수 (랜덤 숫자)를 생성하는 함수를 제공

데이터 분석, 시뮬레이션 등에서 매우 유용하게 활용

```python
# random.random(): 0 이상 1 미만의 실수 난수를 반환합니다.
# random.uniform(a, b): a 이상 b 이하의 실수 난수를 반환합니다.
# random.randint(a, b): a 이상 b 이하의 정수 난수를 반환합니다.

# random.randrange(start, stop, step): start 이상 stop 미만의 정수 난수를 step 간격으로 반환합니다.
# random.choice(sequence): 시퀀스(리스트, 문자열 등)에서 임의의 요소를 반환합니다.

# random.shuffle(sequence): 시퀀스의 요소를 무작위로 섞습니다.
# random.sample(population, k): 인구에서 중복되지 않는 k개의 요소를 무작위로 선택하여 리스트로 반환합니다.
```

### sys
시스템과 관련된 각종 정보들을 제공

 ```python
# sys.path : 파이썬이 설치된 경로 및 라이브러리를의 경로를 반환합니다.
# sys.version : 파이썬의 버전을 출력합니다.
# sys.exit() : 프로그램을 자체적으로 종료합니다.

# sys.argv : 파이썬을 실행시키며 터미널에 함께 입력한 파라미터를 전달받아 활용할 수 있는 기능이다.
# sys.getsizeof() : len()과 달리 메모리의 사이즈를 반환합니다. -->
```

### os
운영체제와 상호작용하여, 파일 및 디렉토리 관리 / 시스템 명령어 실행 / 경로 작업과 관련된 함수 제공

```python
# os.name : 운영체제 이름
# os.getcwd() : 현재 작업 디렉토리 가져오기
# os.chdir() : 작업 디렉토리 변경 

# os.makedirs() : 디렉토리 생성
# os.rmdir() : 디렉토리 삭제
# os.remove() : 파일 삭제
# os.rename() : 파일 및 디렉토리 이름 변경

# os.listdir() : 디렉토리 내 파일 및 서브 디렉토리 리스트

# os.environ : 환경 변수 정보
# os.getenv() / os.putenv() : 환경 변수 값 가져오기 / 설정하기
```

### datetime
날짜와 시간을 다루는 함수를 제공. 

날짜 및 시간의 생성 / 변환 / 포맷팅 수행 가능

```python
# datetime.datetime.now(): 현재 날짜와 시간을 반환합니다.
# datetime.datetime.strftime(format): 날짜와 시간을 문자열로 포맷팅합니다
```

### time
시간과 관련된 기능을 제공

```python
# time.time() : 현재 시간을 초 단위로 반환
# time.sleep() : 주어진 시간동안 프로그램 지연
```

### urllib
웹사이트에서 얻은 데이터를 다루는 패키지


### logging
로그를 남기는 라이브러리

### operator
파이썬에서 수행 가능한 연산을 효율적으로 처리할 수 있는 함수의 모음

#### operator.itemgetter() 메소드
sorted()를 활용한 정렬의 key로 유용하게 쓰인다.

특히 딕셔너리를 정렬할 때 매우 유용하다.


## 외부모듈

### beautifulsoup4 (bs4)
웹 스크래핑 및 데이터 추출을 위한 라이브러리

HTML / XML 등의 마크업 언어로 작성된 웹 페이지 내용을 파싱하고 구문을 분석해서 원하는 데이터를 추출하기 위해 사용

### requests
파이썬으로 HTTP를 호출하는 라이브러리


### 향후 사용하게 될 주요 모듈
#### numpy
데이터 분석에 사용되는 기초 라이브러리

array (행렬 / matrix) 단위의 생성 / 초기화 / 수학 연산에 사용

#### pandas
데이터 조작 / 분석에 매우 유용한 라이브러리

#### matplotlib
데이터 시각화를 위한 라이브러리

#### scikit-learn
대표적인 머신러닝 오픈소스 라이브러리

#### tensorflow
대표적인 딥러닝 오픈소스 라이브러리

#### Diango
파이썬의 웹 프레임워크 중 하나

가장 대표적이지만 무겁고 복잡

#### Flask
파이썬의 웹 프레임워크 중 하나

배우기 쉽고 가볍지만 Diango에 비해 기능이 적음

#### FastAPI
파이썬의 웹 프레임워크 중 하나

비동기 처리를 지원하는 높은 성능과 빠른 속도

비동기 처리로 인한 어려운 난이도


## decorator() (데코레이터)
함수/클래스를 앞 뒤에서 꾸미는 기능 (기능 확장 및 변경)
```python
def test(function):
    def wrapper():
        # 사전적으로
        print("++++")
        function()
        print("++++")
        # 사후적으로
    return wrapper 
        
@test
def hello():
    print("안녕하세요")  
```

# chapter 8 (클래스)
단일 파일에 단일 클래스가 원칙

클래스명은 대문자로 시작한다.

## Class 생성의 3가지 방식
class 클래스
class 클래스()
class 클래스(object)

## 상속
Java : class 클래스명 extends 부모클래스

Python : class 클래스명(부모클래스)

```python
class Character:
    최대속도 = 100
    최대레벨 = 50
    def __init__(self, name, hp, attack, lv):
        self.이름 = name
        self.체력 = hp
        self.공격력 = attack
        self.레벨 = lv
        self.속력 = 1

    def move(self):
        print(f'{self.속력}의 이동속도로 움직였습니다.')

    def attack(self):
        print(f'{self.공격력}의 데미지로 공격했습니다.')


class Mob(Character):
    def __init__(self, name, hp, attack, lv, droprate, dropitem):
        super().__init__(name, hp, attack, lv)
        # super()를 통해 상속하여 하단 4줄 생략 가능
        # self.이름 = name
        # self.체력 = hp
        # self.공격력 = attack
        # self.레벨 = lv
        self.드랍률 = droprate
        self.드랍아이템 = dropitem
        self.속력 = 1

mob1 = Mob('jombi', 100, 30, 1, 0.1, '물고기')
mob1.move()

# 실행 결과 : 1의 이동속도로 움직였습니다.
```


## 객체지향 프로그래밍이란
프로그래밍 : 세상을 코드 안에 담겠다.

=> 엄청난 추상화가 필요.

=> 세상의 모든 것을 객체로 보겠다.

=> 세상에서 일어나는 모든 활동들은 객체들의 상호작용이다.

=> 객체지향 프로그래밍 : 모든 것을 객체로 만들고 객체들 간의 상호작용으로 프로그래밍을 끝낸다.

객체 : 추상적 / 관념적


## 생성자 / 소멸자
생성자 : 객체를 생성할 때 사용하는 함수

소멸자 : 객체를 소멸시킬 때 사용하는 함수

소멸자는 직접 입력하지 않아도 자동으로 실행된다.

```python
class Class:
    # 생성자
    def __init__(self):
        pass
    # 생성자
    def __new__(self):
        pass
    # 소멸자
    def __del__(self):
        pass
```

## 멤버변수 / 메소드 / 인스턴스
멤버변수 : 값 (클래스 멤버 vs 인스턴스 멤버)

메소드 : 기능 (클래스 안에 있는 함수)

인스턴스 : 클래스를 통해 구체화된 객체

```python
class Plant:
    # 생성자
    def __init__(self, name, color):
        # 인스턴스 멤버 변수 (self.변수이름)
        self.name = name + '로봇'
        self.color = color
    
    # 메소드 : 기능
    def 기능인사(self):
        print(f'{self.name} : 무엇을 도와드릴까요?')


    # 클래스 멤버 변수 (클래스 내 공유)
    count = 0

    def __del__(self):
        Plant.count += 1
        print(Plant.count)
```


## 클래스 메소드 / 스태틱 메소드
둘 다 데코레이터 (@)를 사용해서 만든다.

```python
class Plant:
    def __init__(self, name, color):
        self.name = name + '로봇'
        self.color = color

    # cls => class를 의미한다.
    @classmethod
    def 카운트(cls):
        cls.찍어낸횟수 += 1
        print(cls.찍어낸횟수)
    
    @staticmethod
    def 기능미소():
        print('웃는표정')

```

## 겟터 / 셋터
### 겟터
@property

겟터 : 변수 값을 가져온다.

겟은 값을 가져오는 메서드

### 셋터
@겟터함수명.setter

세터 : 변수에 갑을 저장한다.

세터는 값을 밀어넣는 메소드 

## 접근제한자
통상의 접근 제한자 : public / protected / (default) / private

파이썬은 접근 제한자를 갖지 않는다.

파이썬은 언더 스코어로 구분 짓는다.

privaite => __변수명

protected => _변수명

public => 변수명

## 오버로드 / 오버라이드
오버로드(올라탄다) : 상속받은 메소드를 재정의

오버라이드(싣는다) : 같은 이름의 메소드를 여러 개 만드는 것

=> 단 조건 인자 or 리턴을 다르게 한다.

## 추상클래스
정의만 해두고 구체적인 구현은 상속받은 클래스에서 오버라이드를 통해 사용한다.

구체적이지 않은 메소드가 1개 이상이면 추상클래스이다.

## 인터페이스
파이썬에는 존재하지 않는다.

모든 게 다 추상 메소드로 구성되어 있다.

이름 정의만 되어있다.
