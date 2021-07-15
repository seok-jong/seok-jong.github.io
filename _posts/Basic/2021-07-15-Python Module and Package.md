---
title: Python Module and Package

"date": "2021/7/15 14:43"

"categories": 
- basic
tags:
- basic
- python
"toc": true
"toc_sticky": true
---



# Python Module and Package



* **모듈(module)** : 각종 변수, 함수, 클래스를 담고 있는 파일 
  - 특정 기능을 .py 파일 단위로 작성
* **패키지(package)** : 여러 모듈을 묶은 것
  - 특정 기능과 관련된 여러 모듈을 묶은 것( 패키지는 모듈에 네임스페이스를 제공 )
* **파이썬 표준 라이브러리**(Python Standard Library) : 파이썬에서 기본으로 설치된 모듈과 패키지, 내장 함수를 묶은 것.



## 1. import  module

모듈은 `import`를 통해 가져올 수 있다.

ex) 

- import 모듈
- import 모듈1, 모듈2
- 모듈.변수
- 모듈.함수()
- 모듈.클래스()

### import as 

모듈을 가져와 내부의 함수나 변수를 사용할 때, 모듈의 이름을 앞에 입력해 줘야한다는 불편함이 있다. (네임스페이스 지정) 모듈을 불러올 때, `as`를 이용하여 모듈의 이름을 지정해 주면 지정해준 이름을 모듈의 이름을 대신하여 쓸 수 있다. 

```python
>>> import math as m    # math 모듈을 가져오면서 이름을 m으로 지정
>>> m.sqrt(4.0)         

```



### from import 

모듈의 전체 기능을 필요로 하는 것이 아니라 일부분만 사용할 것이라면 `from module import func`를 이용하여 일부분만 가져와 사용할 수 있다. 

```python
>>> from math import pi    # math 모듈에서 변수 pi만 가져옴
>>> pi                     # pi를 바로 사용하여 원주율 출력
3.141592653589793

>>> from math import sqrt    # math 모듈에서 sqrt 함수만 가져옴
>>> sqrt(4.0)                # sqrt 함수를 바로 사용
2.0


```

위와 같은 방식으로 가져올 변수(함수)를 입력하면 사용시에 모듈의 이름을 입력해 주지 않아도 된다. 

cf) 

```python
from math import * # 모듈의 모든 변수, 함수, 클래스를 가져온다. 
from math import sqrt as s
from math import pi as p, sqrt as s

>>> s(4.0)
>>> 2.0
```



---



## 2. import package

패키지는 특정 기능과 관련된 여러 모듈을 묶은 것인데, 패키지에 들어있는 모듈도 import를 사용하여 가져온다. 

ex) 

- import 패키지.모듈
- import 패키지.모듈1, 패키지.모듈2
- 패키지.모듈.변수
- 패키지.모듈.함수()
- 패키지.모듈.클래스()

```python
>>> import urllib.request
>>> response = urllib.request.urlopen('http://www.google.co.kr')
>>> response.status
200
```

위와 같은 방법으로 패키지.모듈.함수()의 형태로 사용



cf) 

```python
import urllib.request as r # 패키지.모듈을 r이라는 이름으로 저장 
from urllib.request import Request, urlopen # 여러개의 함수,클래스 가져온다. 
from urllib.request import * # 패키지.모듈안의 모든 변수, 함수, 패키지를 가져온다. 
from urllib.request import Request as r, urlopen as u # 각각의 함수를 가져온 뒤, 이름을 저장해 준다. 
```



---



## 3. Python Package 

 파이썬 표준 라이브러리(PSL) 이외에도 파이썬 패키지 인덱스(PyPI)를 통해 다양한 패키지를 사용할 수 있다. 
명령어를 입력하면 원하는 패키지를 설치할 수 있다. 

### 3-1) pip 설치 

pip는 파이썬 패키지 인덱스의 패키지 관리 명령어이며 Windows용 파이썬에는 기본 내장되어 있다. 

```
# ubuntu , macOS 설치 
$ curl -O https://bootstrap.pypa.io/get-pip.py
$ sudo python3 get-pip.py #ubuntu 설치 
```

### 3-2) pip install 

- pip install 패키지

cmd 또는 터미널에서 사용한다. 

```
#windows
C:\Users>pip install requests

#ubuntu macOS
$ sudo pip install requests
```



### 3-3) import package 

- import 패키지

```python
>>> import requests                                # pip로 설치한 requests 패키지를 가져옴
>>> r = requests.get('http://www.google.co.kr')    # requests.get 함수 사용
```



**CF)** 

```
pip search 패키지: 패키지 검색

pip install 패키지==버전: 특정 버전의 패키지를 설치(예: pip install requests==2.9.0)

pip list 또는 pip freeze: 패키지 목록 출력

pip uninstall 패키지: 패키지 삭제
```



---



## 4. module Exploration

매번 비슷한 클래스와 함수를 작성하여 코드가 불필요하게 길어진다거나 중복되는 경우를 줄이기 위해 공통되는 부분을 빼내서 모듈과 패키지로 만든다. 이우에는 코드를 다시 작성할 필요 없이 모듈과 패키지를 불러와서 사용하면된다. 

- 모듈(module) : 변수, 함수, 클래스 등을 모아 놓은 스크립트 파일  
- 패키지(package) : 여러 모듈을 묶은 것 

모듈은 비교적 간단한 기능을 담고있고 , 패키지는 코드가 많고 복잡할 때 사용한다. 

### 4-1)  module  만들고 사용하기 

모듈로 사용할 square2.py파일을 만든다. 

```python
# 파일명 : square2.py

base = 2

def square(n):
    return base ** n 
```



만들 모듈을 불러올 main.py파일을 만든다. 

```python
# 파일명 : main.py

import square2

print(square2.base)
print(square2.square(10))
________________________________
>>> 2
>>> 1024
```



---

다음으로 모듈에 클래스를 작성하고 사용해본다.
모듈로 사용할  person.py파일을 만든다. 

```python
class Person:
    def __init__(self,name, age, address):
        self.name = name
        self.age = age
        self.address = address
     
    def greeting(self):
        print("안녕하세요. 저는 {0}입니다.".format(self.name))
```



위에서 만든 person.py 모듈을 사용할 main.py파일을 만든다. 

```python
import person  # import로 person 모듈을 가져옴

# 모듈.클래스()로 person 모듈의 클래스 사용
maria = person.Person('마리아', 20, '서울시 서초구 반포동')
maria.greeting()

_______________________________________________________
>>> 안녕하세요. 저는 마리아입니다.
```





### 4-2) module 시작점 

github에 있는 python 코드들을 보다보면 아래와 같은 코드를 자주 볼 수 있다. 

```python
if __name__ == '__main__':
    코드
```

이 코드는 현재 스크립트 파일이 모든 모듈 파일들을 고려할때, 가장 먼저 시작되어야 하는 시작 파일이 맞는지 검사하기 위해 사용된다. 

이제부터 그 원리를 알아본다. 

먼저 모듈로 사용할 hello.py 파일을 생성해 준다. 

```python
#  파일명 : hello.py
print('hello 모듈 시작')
print('hello.py __name__:', __name__)    # __name__ 변수 출력
print('hello 모듈 끝')
```

위에서 작성한 모듈을 이용할 main.py파일을 만든다. 

```python 
import hello    # hello 모듈을 가져옴
 
print('main.py __name__:', __name__)    # __name__ 변수 출력
```

이제 main.py파일을 실행하여 결과를 살펴보자. 

```python 
hello 모듈 시작
hello.py __name__: hello
hello 모듈 끝
main.py __name__: __main__
```

결과를 분석해 보자면 main.py파일에서 hello.py를 불러온 시점에 hello.py가 실행된다. 
hello.py의 name은 hello라고 출력된다. 
hello.py의 코드가 끝나고 main.py의 name을 보면 main이라고 출력된 것을 확인할 수 있다. 

그럼 hello.py를 직접 실행시킨 결과를 보자. 

```
# python hello.py
hello 모듈 시작
hello.py __name__: __main__
hello 모듈 끝
```

hello.py만 실행시켰을 경우 name이 main으로 출력되는 것을 확인할 수 있다. 
따라서 **직접 실행시킨 스크립트 파일의 name은 main이라고 출력되고 모듈로써 불려진 파일의 name은 해당 파일의 이름으로 출력된다.** 

---



``` python
def add(a, b):
    return a + b
 
def mul(a, b):
    return a * b
 
if __name__ == '__main__':    # 프로그램의 시작점일 때만 아래 코드 실행
    print(add(10, 20))
    print(mul(10, 20))
```



위와 같이 calc.py라는 파일을 만들면 이 파일이 모듈로써 불려지면 name이 일치하지 않기 때문에 if문 아래는 실행되지 않는다. 이러한 경우에는 단순한 모듈로써 add, mul함수만 이용할 수 있다. 하지만 이 파일이 단독으로 직접 실행되게 되면 name이 main으로 일치하기 때문에 if문 아래 코드가 실행된다. 

이러한 방식으로 모듈 파일을 시작파일로 이용할 수도 있고 모듈로도 이용할 수 있다.





---



## 5. package Exploration 

이번에는 패키지를 만들어 본다. 
모듈은 스크립트 파일 하나이지만 패키지는 이러한 모듈이 모인 폴더(디렉터리)로 구성되어 있다. 
( 일반적으로 패키지 파일은 pkg라고 함 )



### 5-1) package 만들고 사용하기



![image](https://user-images.githubusercontent.com/77032455/125731573-1fb3d23f-ca29-4823-a9a6-235e6b967b1f.png)



위와 같은 directory 구조로 .py 파일을 만든다.  (init 파일이 없어도 해당 폴더는 패키지로 인식되고 내용 또한 없어도 된다. 하지만 하위 버전에도 호환되도록 init파일을 작성하는 것을 권장 )

```python
# calcpkg/operation.py
def add(a, b):
    return a + b
 
def mul(a, b):
    return a * b

# calcpkg/geometry.py
def triangle_area(base, height):
    return base * height / 2
 
def rectangle_area(width, height):
    return width * height

```



위와 같이 패키지 폴더 속에 모듈을 만들었다. 이제 이 패키지를 이용할 main.py파일을 만들어 보자.

```python
#main.py

import calcpkg.operation    # calcpkg 패키지의 operation 모듈을 가져옴
import calcpkg.geometry     # calcpkg 패키지의 geometry 모듈을 가져옴
 
print(calcpkg.operation.add(10, 20))    # operation 모듈의 add 함수 사용
print(calcpkg.operation.mul(10, 20))    # operation 모듈의 mul 함수 사용
 
print(calcpkg.geometry.triangle_area(30, 40))    # geometry 모듈의 triangle_area 함수 사용
print(calcpkg.geometry.rectangle_area(30, 40))   # geometry 모듈의 rectangle_area 함수 사용

---
30
200
600.0
1200
```



`import 패키지.모듈`의 형식으로 패키지 속 모듈을 가져와 이용하였다. 

cd) 패키지의 모듈에서는 name 변수에 **패키지.모듈**의 형태로 입력된다. 



### 5-2) from import 응용하기 

package 안의 `init.py`파일의 내용을 아래와 같이 바꾼다.

```python
# __init__.py
from . import operation    # 현재 패키지에서 operation 모듈을 가져옴
from . import geometry     # 현재 패키지에서 geometry 모듈을 가져옴
```

`.`의 의미는 현재 디렉토리를 의미한다. 즉 calcpkg 폴더내에서 operation, geometry 모듈을 가져온다는 내용이다. 

위와 같이 초기화 파일을 작성해 주면 아래와 같이 모듈을 이용할 수 있다. 

```python
import calcpkg    # calcpkg 패키지만 가져옴
 
print(calcpkg.operation.add(10, 20))    # operation 모듈의 add 함수 사용
print(calcpkg.operation.mul(10, 20))    # operation 모듈의 mul 함수 사용
 
print(calcpkg.geometry.triangle_area(30, 40))    # geometry 모듈의 triangle_area 함수 사용
print(calcpkg.geometry.rectangle_area(30, 40))   # geometry 모듈의 rectangle_area 함수 사용
```

import를 통해 package를 불러오면 자동으로 init 파일이 실행이 되고 이 과정에서 모듈을 불러오게 된다. 

또한 init 함수를 아래와 같이 작성하면 다른 형태로 모듈을 이용할 수 있다. 

```python
# __init__.py
from .operation import *    # 현재 패키지의 operation 모듈에서 모든 변수, 함수, 클래스를 가져옴
from .geometry import *     # 현재 패키지의 geometry 모듈에서 모든 변수, 함수, 클래스를 가져옴
```

```python
# main.py
import calcpkg    # calcpkg 패키지만 가져옴
 
print(calcpkg.add(10, 20))   # 패키지.함수 형식으로 operation 모듈의 add 함수 사용
print(calcpkg.mul(10, 20))   # 패키지.함수 형식으로 operation 모듈의 mul 함수 사용
 
print(calcpkg.triangle_area(30, 40)) # 패키지.함수 형식으로 geometry 모듈의 triangle_area 함수 사용
print(calcpkg.rectangle_area(30, 40))# 패키지.함수 형식으로 geometry 모듈의 rectangle_area 함수 사용
```



## Information

1. all로 필요한 것만 공개하기
   `import *`을 사용하면 모듈의 모든 것을 공개해 버리게 된다. 이때, 공개할 모듈,변수,함수,클래스를 all을 통해 리스트로 지정해 주면 된다.

   ```python
   # __init__.py
   __all__ = ['add', 'triangle_area']    # calcpkg 패키지에서 add, triangle_area 함수만 공개
    
   from .operation import *    # 현재 패키지의 operation 모듈에서 모든 변수, 함수, 클래스를 가져옴
   from .geometry import *     # 현재 패키지의 geometry 모듈에서 모든 변수, 함수, 클래스를 가져옴
   
   # main.py
   from calcpkg import *    # calcpkg 패키지의 모든 변수, 함수, 클래스를 가져옴
    
   print(add(10, 20))    # add 함수는 공개되어 있으므로 사용할 수 있음
   print(mul(10, 20))    # 에러: mul 함수는 공개되어 있지 않으므로 사용할 수 없음
    
   print(triangle_area(30, 40))    # triangle_area 함수는 공개되어 있으므로 사용할 수 있음
   print(rectangle_area(30, 40))   # 에러: rectangle_area 함수는 공개되어 있으므로 사용할 수 있음
   ```

    ```
    30
    Traceback (most recent call last):
      File "C:\project\main.py", line 4, in <module>
        print(mul(10, 20))    # 에러: mul 함수는 공개되어 있지 않으므로 사용할 수 없음
    NameError: name 'mul' is not defined
    ```

   

   

2. 하위 패키지 사용하기 


   ![image](https://user-images.githubusercontent.com/77032455/125733993-7e821510-d672-438d-8789-fd67e05b0293.png)

   위 이미지 처럼 패키지 안에 패키지가 있는 형태라면 `import calcpkg.operation.element`로 불러와서 
   `calcpkg.operation.element.add(10, 20)`의 형태로 이용하여야 한다. 
   또한 init을 작성하여 하위 패키지의 모든 모듈을 가져오게 할 수도 있다. 

   ```python
   #calcpkg/__init__.py
   from .operation.element import *
   from .operation.logic import *
   from .geometry.shape import *
   from .geometry.vector import *
   ```

   이렇게 하면 calcpkg.add(10, 20), calcpkg.triangle_area(30, 40) 또는, add(10, 20), triangle_area(30, 40)처럼 사용할 수 있다.

   또한 하위 패키지 안에서 옆에 있는 패키지의 요소를 가져와서 사용하려면 `..` 을 사용해야 한다. 
   `..`은 상위폴더라는 의미이다. 

   ```python
   # calcpkg/geometry/shape.py
   from ..operation import element             # from ..operation.element import mul로도 가능
    
   def triangle_area(base, height):
       return element.mul(base, height) / 2    # mul(base, height)로도 가능
    
   def rectangle_area(width, height):
       return element.mul(width, height)       # mul(width, height)로도 가능
   ```

   
   

3. 모듈과 독스트링
   **모듈**의 독스트링은 모듈 파일의 첫 줄에 """ 또는 ''' 를 이용하여 문자열을 넣는다. 
   **패키지**의 독스트링은 init파일의 첫 줄에 넣는다. 

   모듈과 독스트링을 출력하려면 모듈 또는 패키지의 `doc`를 출력하면 된다 .

   ```
   모듈.__doc__
   패키지.__doc__
   ```

   

## Reference 

https://dojang.io/mod/page/view.php?id=2441

https://dojang.io/mod/page/view.php?id=2447















