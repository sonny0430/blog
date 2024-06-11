# chapter 02 (자료형)
## parameter vs argument
prameter (매개변수) : 함수 안에서 정의 및 사용에 나열된 변수들

argument (전달인자) : 함수를 호출할 때 전달되는 실제 값

## 원의 둘레와 넓이를 구하는 함수 작성
```python
def circle_area(pi, r):
    return r*r*pi

def circle_circumference(pi, r):
    return 2*r*pi
```

## inch -> cm 변환 함수 작성
```python
def inch_to_cm(inch: int)->int :
    """인치를 입력받아 센치미터로 변환해줌

    Args:
        inch (int): 숫자를 입력받음

    Returns:
        int: 센치미터 출력
    """

    return inch*2.54
```

## 변수
변하는 값 / 변하는 걸 담는 그릇

Mutable

변수를 정의하고 하단에서 재정의를 해도 받아들임

## 상수
고정된 값

Immutable

하단에서 바꿔도 강제되지 않음 (파이썬)

대문자로 표기하는 것이 관례

앞에 언더스코어 _를 붙여서 특수한 값임을 알리기도 함

```python
a = 1
a = 2
print(a)
_PI = 3.141592
_PI = 4.141592
print(_PI)
```

## 연산자
대입연산자 (=) 

복합대입연산자 (+=, -=, *=, /=, %=, **=)

## format-string / f-string
```python
print("계산된 원의 둘레 = {} 입니다.".format(circle_area(pi, r)))
    
print(f"계산된 원의 둘레 = {circle_area(pi, r)} 입니다.")
```

## 형변환
int() / float() / str()

## 메소드 (파괴 vs 비파괴)

### 파괴적 메소드
Deep Copy => 원본이 복사

### 비파괴적 메소드
Swallow Copy => 원본을 참조

## 문자열 관련 메소드
```python
U = 'aBcD'.upper()
L = 'aBcD'.lower()

'   aBcD    '.strip()
'   aBcD    '.rstrip()
'   aBcD    '.lstrip()

"dfsf".find("f") # 인덱스를 리턴
"sdf sf sfs".split("s") # => ["","",""]
```
### split
경로탐색에서 많이 사용

```python
print("C:\workspace\first\lec\chapter01.py".split(".")[-1])
print("C:\workspace\first\lec\chapter01.py".split("\\")[-1].split(".")[0])
```

### is 계열 메소드
True / False 반환

```python
"dsfs".isalnum()
"dsfs".isalpha()
"dsfs".isdigit()
```

### in 연산자
```python
print("하" in "안녕하세요")  
```

## List
```python
list_a = ['홍길동', 20, '학생']
print(f"{list_a[0]}의 나이는 {list_a[1]}이며 직업은 {list_a[2]}이다.")
```

# chapter 03 (조건문)
## 제어문()
### 분기문() / 조건문()
if ()

switch ()

select ()

go to ()

## 반복문()
for ()

while ()

do while ()

for each ()

for in ()

comprehension (컴프리헨션)
    
## and / or / xor
and : 둘 다 True => True

or : 둘 중 하나 이상이 True => True

xor : 둘 중 하나만 True => True

## if 문
 C-Style : if (조건) {}

 Python : if 조건:

 비교연산자 (==, >, <, >=, <=, !=)를 활용

 in, and, or, not 연산자 활용

### if문을 사용한 성적처리 함수 작성
```python
def grade(score):
    if score >= 90:
        return 'A'
    elif score >= 80:
        return 'B'
    elif score >= 70:
        return 'C'
    elif score >= 60:
        return 'D'
    else:
        return 'F'
```

### if문을 사용한 짝수/홀수 판별 함수 작성
```python
def odd_even(num):
    if num % 2 == 1:
        print(f'입력한 숫자는 {num} 입니다. 홀수 입니다.')
    else:
        print(f'입력한 숫자는 {num} 입니다. 짝수 입니다.')
```

## datetime 모듈
```python
def date_time():
    import datetime
    now = datetime.datetime.now()
    print(now.strftime("%Y년-%m월-%d일-%a요일 %H:%M:%S"))
    print(type(now))    
    print(now)
```

## for문
C-Style : for(초기값 : 조건식 : 증감식) {}

Python : for i in range(9):

### for문 예시
```python
fruits = ['apple', 'pear', 'strawberry']
# 단수 in 복수

# for item in items:
for fruit in fruits:
    print(f'내가 좋아하는 {fruit}')
```

### for문을 활용한 구구단 사용자 함수 작성
```python
def mul():
    for i in range(0, 8):
        print(f'{i+2} 단')
        for j in range(0, 9):
            print(f'{i+2} x {j+1} = {(i+2)*(j)+1}')
        print('\n')
```

### enumerate와 for문
enumerate를 활용해 index와 값을 동시 출력

```python
fruits = ['apple', 'pear', 'strawberry']
for idx, fruit in enumerate(fruits):
    print(f'{idx}번째로 내가 좋아하는 과일 {fruit}')
```

## zip() : 길이가 같은 것을 처리할 때 사용
```python
i = [2, 3, 4, 5, 6, 7, 8, 9]
j = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    
for a, b in zip(i, j):
    print(a, b)
```

## itertools 모듈
효율적으로 Iterator(값을 차례대로 꺼낼 수 있는 객체)를 생성해 주는 모듈

count : 시작 값부터 일정 간격의 값을 반환하는 Iterator 생성

accumulate : 누적 결과를 반환하는 Iterator 생성

count : 시작 값부터 일정 간격의 값을 반환하는 Iterator 생성

permutations : Iterable에서 순열을 반환하는 Iterator 생성

combinations : Iterable에서 조합을 반환하는 Iterator 생성

groupby : Iterable에서 연속적인 key와 group을 반환하는 Iterator 생성


# chapter 04 (반복문)
## list 자료구조
길이가 변할 수 있다.

리스트 속 index를 통해 접근한다.

배열 vs 리스트 구분 가능해야한다.

list.append() : 요소 추가

list.pop() : 가장 끝에 있는 것을 제거 (Default)
```python
students = ['Hong', 'Lee', 'King_Se']
students.append('King_Kwang')

students.pop()
students.pop(0)
```

## 2차원 리스트
```python
students2 = [['Hong', 20], ['Lee', 30], ['King_Se', 40]]
students2[1][1]

print(len(students2))
print(len(students2[1]))
```

## for문 / list
빈리스트를 생성 / for문으로 값 채우기

```python
def for_list():
    list_a = []
    for i in range(0, 50):
        list_a.append(i)
    print(list_a)
```

## sorted(list) / reversed(list)
sorted() : 리스트 내부 정렬 / 역정렬

reversed() : 리스트 내부 역정렬

```python
list_a = [1,5,2,4,7]
sorted(list_a)
sorted(list_a, reverse=True)
list(reversed(list_a))
```

## for문 / if문 활용한 짝수 리스트 생성
```python
list_a = []
for i in range(30):
    if i % 2 == 0:
        list_a.append(i)
print(list_a)
```

