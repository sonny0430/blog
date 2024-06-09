# chapter 01
- 파이썬 (Python) : Battery Included (배터리 포함) 언어로, 필요한 것을 조합해서 사용하면 된다.
- 문장 (Statement) : 실행할 수 있는 코드의 최소단위
- 표현식 (Expression) : 어떠한 값을 만들어내는 간단한 코드

- 파이썬의 특징
  - 인터프리터 언어이다. (한 줄씩 해석) <=> 반대 : 컴파일 언어
  - 속도가 느리다.
  - 약타입이다.

- 파이썬 표기법
  - snake_case : 함수명, 변수명 표기법
  - PascalCase : 클래스명 표기법

- 연산자 종류
  - 사칙연산자 / 대입연산자 / 증감연산자
  - 1항연산자 / 2항연산자 / 3항연산자


- 출력 print()
```python
    print("프린트문 연습", end="~~~~")
    print("프린트문 연습", end="\n")
    print(help(print))
    print("","","","", end="\n\n\n", sep=" - - ")
```


# chapter 02
- 메모리
  - 힙 (Heap)
    - 레퍼런스 값 저장 
    - 느림
    - 자유로운 입출 가능
  - 스택 (Stack)
    - 원시 타입 
    - 레퍼런스 주소 저장 
    - 빠름 
    - 크기가 처음부터 정해짐 
    - 후입 선출

- 원시자료형
  - 문자열 : "안녕하세요"
  - 숫자형 : int 1234 / float 23.2334
  - bool형 : True / False
  - 자료형의 확인 : type()
```python
    print(type("홍길동"))
    print(type(1234))
    print(type(1.234))
    print(type(True))
```

- 이스케이핑 (Escaping)
  - 이스케이프 문자 : \n \t \r 등
  - 역슬레시 \ 를 활용해 이스케이핑한다.
```python
    # 적용 예시
    print(r"C:\workspace\first\lec\chapter01.py")
    print("C:\\workspace\\first\\lec\\chapter01.py")
```

- 여러줄 문자열 방법
```python
    a = """
    asdf
    zxcv
"""

    a = " \
    asdf \
    zxcv \
"
```

- 문자열 합 연산자
```python
    print("홍길동은" + str(20) + "학생이다")
```

- 문자열 반복 연산자
```python
    print("인사오십번하기 : " + "안녕하세요" - 50)
```

- 문자열 슬라이싱 []
  - 인덱스(index) 숫자는 0부터 시작
```python
    print("안녕하세요"[0:5])
    print("안녕하세요"[2:3])
    print("안녕하세요"[::2])
```

- 문자열 인덱싱 (Indexing)
  - 문자열 자체가 순서를 세는 것이 가능한 이터러블 객체
  - 인덱스로 접근이 가능한 컨테이너 객체
```python
    print("안녕하세요"[2])
    print("안녕하세요"[-1])
```

- 문자열의 개수
  - len() 사용
```python
    print(len("홍길동은 20살 입니다"))
```
    - [] index 0~11 vs len 개수 12

- 숫자형 type
  - int (정수)
    - signed (부호 O)
    - unsigned (부호 X)
  - float (부동소수점 / 실수)

- 연산자
  - +, -, /, //(몫을 정수로 반환), %(나머지), --(제곱)
  - 연산자 우선순위 (표를 찾아볼 것)


# 별도 .py 파일에 사용자 함수 정의
- helpers 폴더 생성 및 __init__.py 생성 : 디렉토리 자체를 하나의 패키지로 봐줌
- 패키지 : 모듈들이 들어있는 디렉토리
- debug.py에 사용자 함수 정의
```python
# 프린트문 축약
def pp(a):
    """프린트문확장

    Args:
        a (str object): 출력할 수 있는 모든 오브젝트
    """
    print("==>>" ,type(a))
    print("==>>" ,a)
```
- 모듈 가져오는 방법
```python
from helpers.debug import -
```


# 기타
- 개발환경 구축
  - anaconda (가상환경)
  - everything (경로검색 툴)
  - vscode (IDE)

- anaconda를 활용한 가상환경 구축
  - 가상환경 구성 이유 : 진행하는 프로젝트에서 필요로 하는 버전과 패키지만을 사용하기 위함
  - conda prompt 명령
    - conda creat -n name
    - conda env list
    - conda activate name
    - deactivate
  
  - pip vs conda => 수업 중에는 pip 위주로 사용할 것
    - pip install PACKAGENAME : conda에 비해 문제를 덜 일으킨다.
    - pip update PACKAGENAME : 기존에 잘 작동하면 안하는게 맞다.
    - pip list
    - pip freeze -> requirements.txt : Save environment to a text file (패키징)
    - pip install -r requirements.txt : 패키징한 것을 인스톨
        

- vscode 사용
Black Formatter 확장 설치
Auto Docstring 확장 설치

- vscode 단축키
ctrl , : 설정
ctrl j : 터미널
ctrl / : 주석처리
alt shift ↓ : 줄 복사
ctrl x : 줄 삭제
alt shift f : 포맷터 작동

- 용어 정리
"&" 엠퍼센트
"|" 파이프
"`" 백틱
"*" 아스타리스크
";" 세미콜론
":" 콜론
"," 콤마
"." 닷
