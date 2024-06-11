# chapter 04 (반복문)
## list vs dictionary
list : index로 접근

dictionary : key로 접근하면 key에 연결된 value를 반환

## dictionary 자료구조
### 특징
key로 접근하여 연결된 value를 반환한다.

### 형태
```python
dic = {
  "key0" : "value0",
  "key1" : "value1",
  "key2" : "value2",
  "key3" : "value3",       
}
```
### dictionary 관련 함수
keys() => key만 모아서 출력

values() => values만 모아서 출력

items() => key와 value를 묶어서 표현 / (tuple) 형태

### dictionary 생성 / 추가 및 수정 / 삭제
```python
students = [
  {
  "이름": "홍길동",
  "나이": "20",
  "직업": "학생"
  },
  {
  "이름": "이순신",
  "나이": "30",
  "직업": "장군"
  },
  {
  "이름": "세종대왕",
  "나이": "40",
  "직업": "왕"
  }
]

# 없는 key에 의한 error / .get() 을 이용한 에러 모면
students[0]["취미"] # 없는 key => error! 
students[0].get("취미") # 없는 key => None 반환

# key / value 추가 및 수정
students[0]["취미"] = "코딩"
students[1]["취미"] = "C코딩"

# 삭제 : 여러 방식 가능 (del, pop())
# key / value가 함께 삭제 됨
students[0].pop("취미")
del students[0]["이름"]
```

### dictionary와 if문 / for문 / comprehention
```python
students = [
  {
  "이름": "홍길동",
  "나이": "20",
  "직업": "학생"
  },
  {
  "이름": "이순신",
  "나이": "30",
  "직업": "장군"
  }
]

key = "이름"
# if key in dictionary
if key in students[1]:
    print(students[1]["이름"])
else:
    print("없는 키를 입력하셨습니다.")

# for문 : key들을 순회하며, value를 반환한다.
for k in students[1]:
    print(students[1][k])

# 컴프리헨션
print([students[1][k] for k in students[1]])
```

### dictionary 관련 함수 사용 예시
keys() / values() / items()

```python
students = [
  {
  "이름": "홍길동",
  "나이": "20",
  "직업": "학생"
  },
  {
  "이름": "이순신",
  "나이": "30",
  "직업": "장군"
  }
]
print(students[1].keys())
print(students[1].values())
print(students[1].items())

for k in students[1].keys():
    print(f'아이템은 {k}입니다.')

for k in students[1].values():
    print(f'아이템은 {k}입니다.')

for k in students[1].items():
    print(f'아이템은 {k}입니다.')

```

## range() 함수
범위 자료형으로 지정된 범위의 연속된 숫자롤 생성한다.

range(start, stop, step)

```python
range(0, 10, 2)
```

## while 반복문
### 특징
언제 끝날 지를, 시작할 때는 모를 때 사용한다.

초기값이 while문 밖에 존재 => 전역변수

for문 안의 i는 지역변수

whlie문 안에서 조건을 바꿔주어야 한다.

break를 사용한 즉시 탈출이 가능하다.

continue를 사용해 하단 내용을 생략 가능하다. 이것이 반복문 탈출은 아니다.

### while문 예시
```python
i = 0
while (i < 10):
    i += 1
    if i == 3:
        print(f'continue 생략')
        continue
    elif i == 7:
        break
    else:
        print(f'{i}')
```

## list 관련 함수 정리
max() : 최대값

min() : 최솟값

sum() : 합계

sorted() : 정렬

enumerate() : index와 value를 함께 출력

''join() : ''를 기준으로 list 요소 병합

split('') : ''를 기준으로 list 요소 분할 

reversed() : 역정렬, 메모리 효율을 위해 주소값으로 반환

## dictionary 관련 함수 정리
keys() : key만 모아서 출력

values() : value만 모아서 출력

items() : key와 value를 묶어서 출력


## list comprehension (리스트 컴프리헨션)
특징 : 가독성 + 속도 + 리스트 내포

```python
list_comprehesion = [i for i in range(10) if i%2 == 1]

list_comprehesion = [i if i%2 == 1 else 0 for i in range(10)]
```

## iterator()
iterable 객체 : 반복시킬 수 있는 객체

iterator : 반복시킬 수 있는 객체를 만들어내는 것 / next()로 꺼낼 수 있는 것

```python
num = [1,2,3]
r_num = reversed(num)
print(r_num)
print(next(r_num))  
print(next(r_num)) 
print(next(r_num))   
```

## other type (tuple / set)
### 튜플 (tuple)
값을 변경불가

### 셋 (set)
중복값을 허용하지 않음


# chapter 05 (함수)
시그니처 : 함수 앞단  (ex) func(a,b,c,d)

## 함수관련 용어
- 인자 / 인수 / 매개변수

- 파라미터 / 아규먼트

- 가인수 / 실인수

정의 ≒ 파라미터 ≒ 가인수
호출 ≒ 아규먼트 ≒ 실인수

## 특징
return이 무조건 존재한다.

return을 명시하지 않으면 None이 반환한다.

함수 호출시 인자 개수가 맞아야한다.

default 값을 주면 개수가 안맞아도 된다.

```python
def func_made(a, b, c = 2):
  pass
```

## 가변매개변수 / 가변위치인자 / 가변키워드인자
파라미터 내부 순서 중요

```python
def variable_argument(g, c=2, *a, **b):
    pass
    # *a
    # 가변매개변수
    # 함수에 전달된 값의 길이를 모른다.
    
    # 가변위치인자 => 리스트 튜플
    # 가변위치매개변수
    # 관례적 이름 : *args
    

    # **b
    # 가변키워드인자 => 딕셔너리
    # 가변키워드매개변수
    # 관례적 이름 : **kwargs
```

### 가변위치인자 예시
```python
def ex_var_arg(*args):
    result = []
    for arg in args:
        result.append(arg)
    return result

a_list = [1,2,3,4,5]
print(ex_var_arg(*a_list))
```

### 가변키워드인자 예시
```python
def ex_var_arg2(**kwargs):
    result = {}
    for k, v in kwargs.items():
        result[k] = v
        print(f'키는 {k} 값은 {v}')
    return result

dic = {
    "name" : "홍길동",
    "age" : 20
  }
print(ex_var_arg2(**dic))
```

## 재귀함수
### 특징
함수 내부에서 자기자신을 호출한다.

상황에 따라 기하급수적으로 반복을 많이한다.

재귀함수를 사용하는 것을 추천하지 않는다.

적재적소에 사용하면 그래도 괜찮다.

메모화 / 메모이제이션을 함께 사용하면 좋다.

메모이제이션 (memorization) : 처리를 계속 수행하지 않고 메모된 값을 저장해두었다가 돌려줘서 속도를 빠르게 하는 기법


### 팩토리얼 값 구하기

#### for문 활용방법
```python
def for_factorial(num):
    output = 1
    
    for i in range(1, num+1):
        output *= i        
    return output
```

#### 재귀함수 활용방법
```python
def recursion_factorial(num):
    if num == 0:
        return 1
    else:
        return num * recursion_factorial(num-1)
```

## 람다함수 (lambda function)
특징 : 무명함수 / 익명함수 / 람다함수 => 한번만 사용하겠다.

### map()
첫번째 인자로 함수를 받아들인다.

함수를 통과시켜서 새로운 객체를 만들어낸다.
```python
print(list(map(lambda x: x ** 2, [1,2,3,4,5])))
print(list(map(lambda x: x > 2, [1,2,3,4,5]))) 
```

### filter()
필터를 통과한 True만 모아서 새로운 객체를 생성한다.
```python
print(list(filter(lambda x: x > 2, [1,2,3,4,5]))) 
```

## file control
with 없이 open / close 방식이 있지만 잘 사용하지 않는다.

with 문을 주로 사용한다.

with 문을 사용시 직접 close()를 하지 않아도 된다.

```python
# 전체를 한번에 출력
with open(r"C:\workspace\first\lec\chapter05.py", encoding="utf-8") as file:
    file.read()

# 한 줄씩 차례로 출력
with open(r"C:\workspace\first\lec\chapter05.py", encoding="utf-8") as file:
    for _ in range(9):
        print(file.readline()) 
```

## generator
iterator를 직접 만들어낸다.

generator를 만들어내는 함수 내부에 yield를 사용하면 generator 함수가 된다.

일반함수와 달리 호출에도 함수 내부 코드가 실행되지 않는다.

```python
def generator():
    print("함수가 호출됨")
    yield '월'
    print('월 호출됨')
    yield '화'
    print('화 호출됨')
    yield '수'
    print('수 호출됨')

for i in generator():
    print(i)
```

## dictionary에서의 value 최소값 구하기
```python
def list_func_keyword_arg():
    book = [{
            "책제목" : "혼공파이썬",
            "가격" : 18000
        },
        {
            "책제목" : "혼공파이썬2",
            "가격" : 20000
        }
    ]
    
    def price(books):
        return books["가격"]
    
    print(min(book, key = price))
    print(min(book, key=lambda book : book["가격"])["가격"])
```


## steak / heap
### steak
기본 자료형 : 숫자 / 문자열 / 불

가볍고 정형화 되어있다.

빠르게 찾을 수 있다.

속도가 빠르다.

### heap
참조 자료형 (객체 자료형) : 나머지 전부

무겁고 정형화 되어있지 않다.

속도가 느리다

### list / dictionary
실제 데이터 => heap에 저장

메모리 주소 => steak에 저장


# chapter 6 (예외처리)
오류 / 에러 / 에러

구문오류 / 런타임오류 / ...

파이썬이 사전정의해둔 Exception 클래스를 상속해서 사용한다.

try except 구문을 사용한다.

```python
# try except 구문
# finally : 예외처리와 상관없이 무조건 실행하는 구문
try:
  import 없는모듈
except Exception as e:
  print(f'{e}')
except ZeroDivisionError as e:
  print(f'{e}')
except PermissionError as e:
  print(f'{e}')
finally:
  print(f'finally')
```
