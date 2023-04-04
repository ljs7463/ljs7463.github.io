---
layout: single
title: "[파이썬 클린코드] 코드 문서화(Documentation)"
categories : Python
tags : 클린코드, 홀로성장하기
---

> INDEX
1. 서론 : 문서화와 클린코드의 중요성
2. 본론 : 코드 문서화 방법(어노테이션 VS docstring)
3. 결론 : 나는 둘다~

# 1. 서론 : 문서화와 클린코드의 중요성

주니어 데이터분석가로서 내가 오늘 만든 코드는 그림에 비유했을때 스케치 정도라고 생각한다. 그저 빠르게 뼈대만을 그려 어떻게든 방향을 잡는것...

하지만, 내가 고심해서 만든 코드들은 결국 서비스에 연결이 되어야 하고, 그러기 위해서는 사람들과 공유를 통해 리펙토링과 유지보수의 단계를 거쳐 색을 입혀나가는 단계가 필요하다.

누군가의 도움을 받기에는 사내에서 서비스 준비로 시간도 부족하기 때문에 혼자서라도 나중을 대비하여 유지보수가 쉽게 코드를 만들어야하고, 문서화역시 해야합니다..ㅠㅠㅠ 시간이 정말 많이 필요함을 느끼고 있습니다. 
분석가로서 개발적인 부분을 강화하기란 쉽지 않은것 같아요.. 

누군가 왜 회사의 나중을 위해서 이렇게 까지(자발적 야근과 주말 공부) 하냐고 물어보실수도 있습니다.
그냥...서비스를 성공시켜서 회사를 키우고 이로인한 보상이 저의 성장과 더불어 금전적인 보상까지 이루어지리라 믿어 의심치 않아서...라고 답변하고싶네요 ㅎㅎ( 다시 본론으로 아니..서론으로 넘어가겠습니다.)

저와 같은 상황에서 만약에 코드리뷰가 제대로 되지않은상태로 서비스에 나간다면, 분명히 해결해야 할 부분(리펙토링을 통한 코드(API통신등)효율화, 문서화)인것을 인지하고 있기때문에 결국에는 "기술부채"로서 남아있게 되는것입니다.

**기술부채**또한 부채의 성격으로서 늘어날수록, 누적될수록 겉잡을 수 없게 되어버립니다. 그리고 이러한 기술부채들이 종속성을 갖는다면..서비스의 돌발변수로 되어 돌아옵니다.

따라서 문서화를 통한 만든 모듈들의 기능을 정확히 해두고, 어떤 파라미터를 입력받아 어떤 출력물을 내보낼지, 그리고 이때의 파라미터 타입은 문자열인지, 데이터프레임 인지 와 같이 미리 만들어 두지 않는다면, 추후에 본인이 만든 함수들을 알아보지 못하게 되는 경우가 생깁니다. 

또한 유지보수를 위해 공유할 수 있는 코드로 만들기 위한 클린코드는 매우중요하다고 생각합니다.

# 2. 어노테이션 VS docstring

코드를 문서화 하는 방법중에 어노테이션(Annotation)과 docstring이라는 방법 두가지를 책 초반부에 다루고 있어서, 이를 비교해보고 정리해보고자 합니다.

#### 어노테이션?
어노테이션의 기본 아이디어는 코드 사용자에게 함수 인자로 어떤 값이 와야하는지 힌트를 주는것입니다. 

예를 들어 경도와 위도 좌표를 입력받아서 POINT(경도,위도를 통한 하나의 점)객체를 반환하는 함수가 있다고 가정해 보겠습니다. 
```python
def locate(latitude: float, longirude : float) -> Point:
	"""맵에서 좌표에 해당하는 객체를 검색한다."""
```

위의 함수를 보면 파라미터에 float라는 타입을 지정해준것과 ->Point 라는 반환 객체의 형태 또한 지정해둔것을 알 수 있습니다. 

하지만, 이것을 통해서 파이썬이 타입을 검사하거나 강제하지는 않습니다. 즉, 다른타입을 넣어도 작동을 하긴합니다. 

> 그렇다면? 특정타입을 강제하는 방법이 있을까?
찾아보니 "mypy", "pytype"라는 타입체크 라이브러리가 있다고 하고, 책에서 추후에 다룬다고하니 그때 같이 다루어보겠습니다.

결국 파이썬은 동적 언어이기때문에 타입을 고정하는 방법보다는 필요한경우 체크하는 라이브러리를 사용하거나, 어노테이션을 통해 적어두는 정도라고 보면될것같네요

**어노테이션**을 사용함으로서 얻을 수 있는 이점중에 @dataclass데코레이터를 사용하여 클래스를 간결하게 하는 점도 있습니다.

>#### @dataclass 란?
정답부터 말하면 파이썬에서 클래스로 작업시 코드를 줄여주는 표준라이브러리 라고 한다.
예를들어 아래와 같은 dictionary가 있다고 예를 들어보자,
```python
user = {'name':'Steve',
		'last_name':'Jobs'
        }
print(user['name'])
```
이렇게 dictionary를 사용하는 대신에
```python
user = Person(name = 'Steve', last_name = 'Jobs')
print(user.name)
```
이렇게 클래스를 사용할 수 있다. 하지만, 클래스는 사전에 정의가 필요하고 그렇게 하기위해서는 __init__이라는 생성자가 필요하게 된다.
즉, 다음과 같이 사전정의가 필요하다.
```python
class Person:
	def __init__(self, name, last_name):
    	self.name = name
        self.last_name = last_name
user = Person('Steve', 'Jobs')
```
이렇게 하게되면 dict형태를 사용할때보다 길어지는데 이는 생성자에서 변수를 초기화를 시켜야하기 때문이다.
이때 만약에 생성자가 없어도 된다면 얼마나 좋을까?? 그래서 나온게 "dataclass'이다.
(내용 출처 : <https://www.youtube.com/watch?v=VY7akCnhQ9o&t=171s>)
dataclass로 위에서 사용한 클래스를 줄이면 아래와 같다.
```python
from dataclasses import dataclass
@dataclass
class Person:
	name : str
    last_name : str
user = Person('Steve','Jobs')
```
이렇게 작성한다면 파이썬은 자동으로 __init__메서드를 생성해준다.
또한, dataclass를 사용하면 인스턴스 생성방법을 제어할 수 있다.
예를들어 위의 Person클래스의 경우 Person클래스가 많아지면 헷갈리게 된다.
아래와 같이 말이다.
```python
Person('Steve', 'Jobs', 'M', 10, 4, 2)
```
이런경우는 보통 아래와 같이 키워드 인수를 사용하는것을 알것이다.
```python
Person(
		name = 'Steve',
        last_name = 'Jobs',
        gender = 'M',
        billions = 10,
        childre = 4,
        companies = 2
        )
```
즉, dataclass를 사용해서 키워드 인수로만 클래스를 강제할 수 있는데 다음과 같다.
```python
from dataclasses import dataclass
@dataclass(kw_only = True)
class Person:
	name : str
    last_name : str
```
또 다른 기능으로 Person.name = 'Jake' 의 코드를 통해 name을 바꿀수 있는데 이를 바꾸지못하게 고정시킬 수 있는데 방법은 아래와 같다.
```python
from dataclasses import dataclass
@dataclass(kw_only = True, frozen = True)
class Person:
	name : str
    last_name : str
```

#### docstring?

docstring은 쉽게 말해보면 소스코드 내에 포함된 문서(documention)이라고 볼 수 있다. 여기서 확실히 할 것은 "코멘트"가 아닌 "문서" 라느 점이다.

내가 작성한 모듈들을 다른 사람에게 전달할때 docstring으로 어떻게 동작하는지, 그리고 어떤 기능인지, 어떤인자를 받아서 어떤 형태로 return이 되는지를 적어두고 쉽게 확인할 수 있게 만드는 습관이 좋은것같다.

또한, 파이썬은 동적인 데이터 타입을 갖기때문에 docstring을 통한 문서화를 통해 함수 내 입출력을 설명할 수 있다.

docstring의 예시를 간단하게 만들어보았다.

```python
def my_function(firstNum : int, secondNum : int):
	"""두개의 숫자를 입력받아 곱해주는 함수이다.
    """
    return firstNum * secondNum
    
help(my_function)

>>> 
Help on function my_function in module __main__:
my_function(firstNum: int, secondNum: int)
    두개의 숫자를 입력받아 곱해주는 함수이다.
```
예시를 보면 함수 내에 """ """사이에 적어둔 글자들이 보일것이다 이것을 docstring이라고 하며 이렇게 함수나 클래스에 대한 설명을 적어주는 것이다.
자세할 수록 좋다고 생각한다.


# 3. 나는 둘다~
그렇다면 docstring과 어노테이션중에 어떤것을 써야할까??
정답은 둘 다 쓰면좋다고 한다. docstring과 어노테이션은 서로 보완적인 관계에 있다고 보면 될 것 같다.

어노테이션을 통해 타입을 지정해주었지만, 실제 모듈 내부에서 받은 인지가 무엇을 뜻하고 로직이 돌아가는게 어떠한 의미를 가지고 있는지를 docstring을 통해서 기입해 준다면 시간이 지난후에도 유지보수에 많은 도움이 될 것 같다.