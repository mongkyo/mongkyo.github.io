# 추석연휴 python 정리

## 용어 정리 

- 리터럴 literer
--
변하지 않는 고정된 데이터 자체의 표현

- 변수
--
파이썬의 모든 것은 객체(Object)로 이루어져 있다. 
객체는 데이터의 형태를 결정해주는 타입으로, 파이썬에서는 객체 타입을 바꿀 수 없다.
> 변수는 어떠한 객체를 담는 그릇일 뿐이다. 특정한 객체를 가르키고 있는 포인터라고 이해해도 좋다.


- 리스트 컴프리헨션
--

- 리스트 컴프리헨션 

```
[표현식 for 항목 in iterable객체 조건식]
```

- 리스트 컴프리헨션의 중첩

```
for color in colors:
	for fruit in fruits:

[(color, fruit) for color in colors for fruit in fruits]
```

셋 컴프리헨션

```
{표현식 for 표현식 in iterable객체}
```


