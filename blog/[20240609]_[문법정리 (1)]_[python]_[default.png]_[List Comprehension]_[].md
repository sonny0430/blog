# List Comprehension
- 리스트 컴프리헨션 (List Comprehension)은 한 줄로 간결하게 리스트를 생성하는 문법이다. 
- 컴프리헨션 (Comprehension)문법은 Core가 C로 되어 있으며, 2중 이상으로 사용하지 않는 것을 권고한다.
- 일반 for loop 문을 활용하여 list.append()를 사용하는 것보다 속도가 빠르다.
- if문과 for문을 활용하여 원하는 값을 포함한 리스트를 생성할 때 유용하다.

## for 문
Python 코드:
```python
# 반별 점수가 [국, 영, 수]로 인원만큼 주어집니다. 평균 점수가 80점 이상인 인원이 얼마나 되는지 카운팅하는 solution함수를 완성해주세요.

def solution(data):
    result = [sum(i)//3 for i in data]
        
    return len(list(filter(lambda x: x >= 80, result)))
```
## if 문
Python 코드:
```python
# 캐릭터 이름, 공격력, 방어력, 체력, 마력이 리스트로 ['Licat', 98, 30, 21, 60]와 같이 주어졌을 때 모든 능력치의 합이 350 이상이 되는 캐릭터의 이름을 출력하는 solution함수를 완성해주세요.

def solution(data):        
    result = [i[0] for i in data if sum(i[1:])>=350]  
             
    return sorted(result)
```

## if else 문
Python 코드:
```python
# 주어진 문자열의 숫자를 모두 더하는 solution 함수를 완성해주세요.

def solution(data):
    return sum([int(s) if s.isdigit() == True else 0 for s in data])
```

## 0으로만 채운 리스트
Python 코드:
```python
# 0을 n개 채운 리스트
[0 for _ in range(n)]

# 0을 n개씩 채운 2중 리스트
[[0 for _ in range(n)] for _ in range(n)]
```


글 내부에 사용된 문제 출처 (https://100.pyalgo.co.kr/)