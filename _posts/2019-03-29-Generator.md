---
title: "python-Generator"
categories: 
  - python
tags:
    - python
    - Generator
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---

# Generator

## Generator의 기본 개념

제너레이터(Generator)는 특별한(?) 종류의 이터레이터(Iterator)이다. 제너레이터는 이터레이터에서 사용했던 iter()나 next() 메서드로 클래스를 작성하는 방식을 피하고, 함수 내부에 `yield` 키워드만 사용해주면 된다. 제너레이터 함수를 호출하면 제너레이터 객체가 반환되고, 제너레이터 객체에서 `__next__` 메서드 또는 next 함수를 호출하면 yield까지 실행한 뒤 yield에 지정한 값이 반환값으로 나온다. 제너레이터의 특징을 간단히 말하면 다음과 같다. 

- 모든 제너레이터는 이터레이터이다. (반대는 성립하지 않는다)
- 모든 제너레이터는 게으른 팩토리이다. (값을 그때 그때 생성한다)

아래의 코드는 제너레이터를 사용하여 yield안에서 함수를 호출하는 코드이다.

```python
def fruit_generator(fruits):
    for i in fruits:
        yield i.upper()

fruits = ['apple', 'banana', 'grape']

for i in fruits_generator(fruits):
    print(i)

>>> APPLE
>>> BANANA
>>> GRAPE
```

이터레이터에서는 `__next__` 메서드 안에 return을 사용해서 직접 값을 반환 했지만, 제너레이터에서는 yield에 지정한 값이 `__next__`메서드(next함수)의 반환값으로 나온다. 또한 이터레이터에서는 raise를 통해 StopIteration을 발생시켰지만, 제너레이터에서는 자동으로 StopIteration을 발생한다.

제너레이터에서 yield 는 return 과 달리 값을 내놓은 후에도 함수 흐름이 종료되지 않고 잠시 중단될 뿐이다. 제너레이터 객체는 생성되고 나서 바로 실행되지 않는다. 대신 외부로부터 호출이 되면 그 순간 실행이 된다. 그 실행은 next()메서드를 사용하는데 함수의 yield를 만날때까지 실행한 후 멈춘다. 이 멈춘 순간 대기 상태로 들어간다. 그리고 다시 호출이 되면 다음번 yield를 만날때까지 실행된다.

![generator](/assets/deploy/generator.png)


## Generator send()

```python
def gen():
    print('gen start')
    data1 = yield 1 # send를 통해 받을 수 있다.
    print(f'gen[1] : {data1}')
    data2 = yield 2
    print(f'gen[2] : {data2}')
    return 'done'
    
>>> g = gen()
>>> first = g.send(None)
gen start
>>> first
1  # yield 1을 통해 넣은 값

>>> second = g.send('hello')
gen[1] : hello
>>> second
2  # yield 2를 통해 넣은 값

```

위의 코드는 gen()이라는 함수를 만든 것이다. 안에 기능은 단순히 print문을 호출하는데, 여기서 yield를 data값으로 받아서 출력한다. 이때 `yield 1`과 `yield 2`를 통해 first, second에 값이 들어간 것도 확인 가능하다. send()를 사용하면 yield를 만나는 시점에서 값을 전달하여 함수를 호출 할 수 있다. 
send()는 `__next__()`와 거의 동일한데, 차이가 있다면 이 시점에서 코루틴 내부로 값을 밀어넣을 수 있다. 

> 코루틴은 함수 사이의 권한을 넘기는 것을 말한다. 위의 제너레이터도 코루틴이다.(이터레이터는 코루틴이 아님)

## yield from

`yield from`에는 반복 가능한 객체, 이터레이터 객체, 제너레이터 객체를 지정해준다. yield from은 아래의 형식으로 함수 내부에 사용해주면 된다.

```python
yield from 반복 가능 객체
yield from 이터레이터 객체
yield from 제너레이터 객체
S
# 예제 코드
def num_gen():
    x = [1, 2, 3]
	yield from x  # 리스트의 요소들을 순회하며 전달

for i in num_gen():
    print(i)

>>> 1
>>> 2
>>> 3
```

위의 예제 코드를 작성하면 리스트에 들어있는 값을 한개씩 차례대로 전달하는 것을 보여준다. 즉 next()함수를 세번 호출할 수 있다. (호출이 끝나면 당연히 StopIterator Error발생)





