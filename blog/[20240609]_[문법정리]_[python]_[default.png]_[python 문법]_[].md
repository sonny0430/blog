# List Comprehension
- 리스트 컴프리헨션 (List Comprehension)은 한 줄로 간결하게 리스트를 생성하는 문법이다. 
- 컴프리헨션 (Comprehension)문법은 Core가 C로 되어 있으며, 2중 이상으로 사용하지 않는 것을 권고한다.
- 일반 for loop 문을 활용하여 list.append()를 사용하는 것보다 속도가 빠르다.
- if문과 for문을 활용하여 원하는 값을 포함한 리스트를 생성할 때 유용하다.
- if문의 위치는 if만 사용시 for문 보다 우측에, if~else를 사용시 for문보다 좌측에 사용한다.

## for 문
Python 코드:
```python
# 반별 점수가 [국, 영, 수]로 인원만큼 주어집니다. 평균 점수가 80점 이상인 인원이 얼마나 되는지 카운팅하는 solution함수를 완성해주세요.

def solution(data):
    result = [sum(i)//3 for i in data]
        
    return len(list(filter(lambda x: x >= 80, result)))

solution([[98, 92, 85], [95, 32, 51], [98, 98, 51]])
```
## if 문
Python 코드:
```python
# 캐릭터 이름, 공격력, 방어력, 체력, 마력이 리스트로 ['Licat', 98, 30, 21, 60]와 같이 주어졌을 때 모든 능력치의 합이 350 이상이 되는 캐릭터의 이름을 출력하는 solution함수를 완성해주세요.

def solution(data):        
    result = [i[0] for i in data if sum(i[1:])>=350]  
             
    return sorted(result)

solution([['Licat', 98, 92, 85, 97], ['Mura', 95, 32, 51, 30], ['Binky', 98, 98, 51, 32]])
```

## if else 문
Python 코드:
```python
# 주어진 문자열의 숫자를 모두 더하는 solution 함수를 완성해주세요.

def solution(data):
    return sum([int(s) if s.isdigit() == True else 0 for s in data])

solution('1hel2lo3')
```

## 0으로만 채운 리스트
Python 코드:
```python
# 0을 n개 채운 리스트
[0 for _ in range(n)]

# 0을 n개씩 채운 2중 리스트
[[0 for _ in range(n)] for _ in range(n)]]
```



# 딕셔너리의 sorted 정렬
## key와 value를 이용한 sorted 다중 조건 적용

```python
# items()를 이용해, 튜플로 받은 뒤 x[1] (value), x[0] (key)를 이용한 다중조건 정렬
# 다중조건은 lambda 내부에 튜플 형태로 작성한다. (조건, 조건)
def solution(data):
    temp = sorted(data[1].items(), key = lambda x: (x[1], x[0]))
    return [temp[i][0] for i in range(len(temp))]

```


# map / filter 차이



글 내부에 사용된 문제 출처 (https://100.pyalgo.co.kr/)