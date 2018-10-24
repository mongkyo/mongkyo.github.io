# Python 기본 문법 정리 

## 함수(function)

### 매개변수(parameter)와 인자(argument)의 차이

함수 내부에서 함수에게 전달되어 온 변수는 매개변수라 부르며, 함수를 호출할 때 전달하는 변수는 인자로 부른다. 

```
# 함수를 정의할 때는 매개변수
>> def func(매개변수1, 매개변수2):
	    · · ·

# 함수를 호출할 시에는 인자
>> func(인자1, 인자2)

```

### 리턴값이 없을 경우

함수에서 리턴해 주는 값이 없을 경우, 아무것도 없다는 뜻을 가진 None 객체를 얻는다.

```
>> def None_type_function(something):
       print(something)
	
>> result =  None_type_function('aaa')
>> type(result)
NoneType
```

### 인자 전달 방식

#### 위치 인자(Positional arguments)

매개변수의 순서대로 인자를 전달하여 사용하는 경우

```
>> def student(name, age, gender):
       return {'name': name, 'age': age, 'gender': gender}

>> student('Mongkyo', 28, 'male')
{'name': 'Mongkyo', 'age': 28, 'gender': 'male'}

```

#### 키워드 인자 (keyword arguments)

매개변수의 이름을 지정하여 인자로 전달하여 사용하는 경우

```
>> student(age=31, name='Monkyo', gender='male')
{'name': 'Mongkyo', 'age': 28, 'gender': 'male'}

```

#### 기본 매개변수 값 지정

인자가 제공되지 않을 경우, 기본 매개변수로 사용할 값을 지정할 수 있다.

```
>> def student(name, age, gender, cls='WPS'):
       return {'name': name, 'age': age, 'gender': gender, 'class': cls}
       
>> student('Mongkyo', 28, 'male')
{'name': 'Mongkyo', 'age': 28, 'gender': 'male', 'class': 'WPS}
```

#### 기본 매개변수값의 정의 시점

기본 매개변수 값은 함수가 실행될 때 마다 계산되지 않고, 함수가 정의되는 시점에 계산되어 계속해서 같은 매개변수 값이 사용된다.

```
>> def return_list(value, result=[]):
      result.append(value)
      return result
      
>> return_list('apple')
['apple']

>> return_list('banana')
['banana']

```

따라서 함수가 실행되는 시점에 기본 매개변수 값(result)을 계산하기 위해서는 아래와 같이 바꿔주어야 한다.

```
>> def return_list(value, result=None):
		if result is None:
    		result = []
		result.append(value)
		return result

>> return_list('apple')
['apple']

>> return_list('banana')
['banana']

