
# 예외

예외란 프로그램에서 벌어지는 예외적인 상황을 뜻한다. 예를 들면 파일을 읽으려고 할 때 그 파일이 존재하지 않는 경우라던지, 또는 배열의 크기를 넘은 인덱스를 참조하는 경우이다. 이러한 상황을 처리해 주는 것을 예외 처리라고 한다. 파이썬에서는 아무런 처리를 하지 않는 예외에 대하여 자동으로 에러를 일으키며, 에러문을 출력하고 프로그램을 종료한다.

## 에러


```python
사과
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-16-2ef8d2bd498e> in <module>()
    ----> 1 사과
    

    NameError: name '사과' is not defined


`사과`라는 변수가 정의되어 있지 않기 때문에 `NameError` 에러가 난다.


```python
"사과" + 100
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-17-2ac8e5e832c6> in <module>()
    ----> 1 "사과" + 100
    

    TypeError: must be str, not int


문자와 정수를 더할 수 없기 때문에 `TypeError` 에러가 난다.


```python
r = 100
if r < 1000
    print("천보다 작다.")
```


      File "<ipython-input-18-b6f51f6f629c>", line 2
        if r < 1000
                   ^
    SyntaxError: invalid syntax
    


`if` 문 끝에 콜론 `:`을 붙이지 않기 때문에 `SyntaxError` 에러가 발생했다.


```python
리스트 = ['ㄱ', 'ㄴ', 'ㄷ']
리스트[3]
```


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-19-6d133e5e254c> in <module>()
          1 리스트 = ['ㄱ', 'ㄴ', 'ㄷ']
    ----> 2 리스트[3]
    

    IndexError: list index out of range


리스트의 크기가 3개이지만 인덱스는 0부터 시작하기 때문에 3번째 인덱스에 해당하는 성분이 존재하지 않기 때문에 `IndexError` 에러가 발생한다. 파이썬 [내장 예외](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)는 다음과 같은 클래스 구조로 되어 있다.

```python
BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
           +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- ResourceWarning
```

| 클래스 이름 | 설명 |
| --- | --- |
| `Exception` | 모든 내장 예외의 기본이 되는 클래스로 사용자 정의 예외를 작성하고자 하며 이 클래스를 상속받아 구현해야 한다. |
| `NameError` | 지역, 전역 이름공간 중에서 유효하지 않은 이름을 접근하는 경우 발생한다. |
| `OSError` | 시스템 관련 에러이다. |
| `SyntaxError` | 구문 오류로 발생하는 예외이다 |
| `TypeError` | 부적절한 타입의 객체에 값을 할당하는 경우 발생하는 예외이다. |

## 예외 처리

예외가 발생할 가능성이 있는 문장들을 `try` 문을 이용해서 예외를 처리할 수 있다. `try` 문은 일반적으로 다음과 같이 사용할 수 있다. 예외 발생이 가능한 문장을 `try` 절에 작성하고 예외 발생시 처리를 할 수 있는 문장을 `except` 절에 작성한다. 예외가 발생하지 않으면 `else` 절이 시행이 되고 `finally` 절은 예외 발생과 상관없이 항상 실행이 된다.

```python
try:
    <예외 발생 가능 문장>
except <예외>:
    <예외 처리 문장>
except (예외1, <예외2>, ..., <예외n>):
    <예외 처리 문장>
except 예외 as 변수:
    <예외 처리 문장>
else:
    <예외가 발생하지 않은 경우에 실행되는 문장>
finally:
    <예외 발생과 상관없이 항상 수행되는 문장>
```

- `except`를 이용하여 발생하는 예외의 종류와 상관없이 모두 같은 처리를 수행할 수 있다.


```python
try:
    2/0
except:
    print("예외 종류 상관없이 처리한다.")
```

    예외 종류 상관없이 처리한다.
    

- 여러 개의 `except` 문장을 순차적으로 검사한다.


```python
try:
    1/0
except ZeroDivisionError:
    print("0으로 나누는 에러.")
except TypeError:
    print("타입이 같아야 합니다.")
except:
    print("에러가 발생했지만 어떤 종류인지 알 수 없다.")
```

    0으로 나누는 에러.
    

발생한 예외가 `ZeroDivisionError`인지 검사하고 일치하면 해당 `except`절을 수행하고, 다르면 `TypeError` 인지를 검사한다. 모두 아닌 경우 `except` 절을 통하여 예외를 처리한다.

- 예외 인스턴스 전달

내장 예외가 발생하는 경우 단순히 예외 발생 여부뿐만 아니라, 추가적인 정보도 예외 인스턴스 객체의 인자에 전달된다. 이 정보를 이용하기 위해서는 예외 클래스의 인스턴스 객체를 변수로 할당하여 사용하면 된다. 다음 예제는 `as` 구문으로 예외 인스턴스 객체의 추가적인 정보를 출력한다.


```python
try:
    1 + "a"
except ZeroDivisionError:
    print("0으로 나누는 에러.")
except TypeError as e:
    print("타입이 같아야 합니다.", e.args[0])
except:
    print("에러가 발생했지만 어떤 종류인지 알 수 없다.")
```

    타입이 같아야 합니다. unsupported operand type(s) for +: 'int' and 'str'
    


- 다른 예외들을 함께 처리

예외의 종류는 다르지만 예외 처리하는 부분은 같게 할 수 있다. 이런 경우 다음과 같이 에외를 튜플로 묶어서 처리하면 된다.


```python
try:
    1/0
except (ZeroDivisionError, OverflowError, FloatingPointError):
    print("수와 관련된 에러.")
except TypeError:
    print("타입이 같아야 합니다.")
except:
    print("에러가 발생했지만 어떤 종류인지 알 수 없다.")
```

    수와 관련된 에러.
    

- 부모 클래스 이용

예외 클래스 계층 구조에서 부모 클래스를 `except` 구문으로 에러를 처리하면 자식 클래스도 같은 에러를 처리할 수 있다.


```python
try:
    1/0
except ArithmeticError:
    print("수와 관련된 에러.")
except TypeError:
    print("타입이 같아야 합니다.")
except:
    print("에러가 발생했지만 어떤 종류인지 알 수 없다.")
```

`ZeroDivisionError, OverflowError, FloatingPointError`에 관한 에러가 발생하면 이들의 부모 클래스인 `ArithmeticError`에 의해서 처리할 수 있다.

## 예외 처리 순서

여러 가지 `try` 문의 예외 처리 순서를 좀더 알아보자.

### `try ... except` 문


```python
try:
    print("예외 발생 전")
    1 / 0
    print("예외 발생 후")
except:
    print("예외 발생")
```

    예외 발생 전
    예외 발생
    

### `try ... except ... finally` 문

`try` 절에서 예외가 발생하지 않으면 `finally` 절의 모든 문장을 실행하고 `try` 문을 끝낸다. `try` 절에서 예외가 발생하면 `try` 절의 나머지 문장을 건너 띄고 `except` 절에 나열된 예외와 발생된 예외가 일치하는 것을 찾는다. 일치하는 예외를 찾으면 그 `except` 절 문장을 실행한 후 `finally` 절을 실행한 후 끝낸다. 일치하는 예외를 찾지 못하면 `finally` 절을 수행한 후 다시 같은 예외를 발생시킨다.


```python
try:
    print("예외 발생 전")
    x = 2/0
    print("예외 발생 후")
    x = "2" + 2
except ZeroDivisionError:
    print("0으로 나누는 에러 발생.")
except Exception as e:
    print(e)
    print("예외 문장")
finally:
    print("파이널리 문장.")
```

    예외 발생 전
    0으로 나누는 에러 발생.
    파이널리 문장.
    

** 직접하기 **

- 위 코드에서 6줄과 8줄을 바꾸면 어떤 결과가 출력되는가? 이유를 설명하시오.

### `try ... finally` 문

`try` 안에서 예외가 발생하면 `finally` 절로 제어가 넘어가고 `finally` 절이 모두 수행되면 동일 예외가 다시 발생된다. `try` 절에서 예외가 발생되지 않으면 `try` 절의 모든 문장을 수행하고 `finally` 절의 모든 문장을 실행한다.


```python
try:
    print("예외 발생 전")
    x = 2/0
    print("예외 발생 후")
finally:
    print("파이널리 문장.")
```

    예외 발생 전
    파이널리 문장.
    


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-7-9a23210ec287> in <module>()
          1 try:
          2     print("예외 발생 전")
    ----> 3     x = 2/0
          4     print("예외 발생 후")
          5 finally:
    

    ZeroDivisionError: division by zero


- 3줄에서 `ZeroDivisionError`가 발생하면 다음 문장부터는 수행하지 않고 `finally` 절로 이동하여 `finally` 절의 모든 문장을 수행한다. 그리고 다시 `ZeroDivisionError` 예외를 발생시키므로 에러문이 출력이 된다.

## 예외 발생시키기

`raise` 문을 이용하여 예외를 발생시킬 수 있다. 다음과 같은 3가지 형식으로 사용할 수 있다.

- `raise [Exception]`: 해당 예외를 발생한다.
- `raise [Exception(arg)]`: 예외 발생시 관련 인자(arg)를 전달한다.
- `raise`: 가장 가까이에 발생된 예외를 그대로 다시 발생시킨다.


```python
raise Exception("내가 예외를 발생시켰다.")
```


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    <ipython-input-10-ba47a616c93d> in <module>()
    ----> 1 raise Exception("내가 예외를 발생시켰다.")
    

    Exception: 내가 예외를 발생시켰다.



```python
raise
```


    ---------------------------------------------------------------------------

    RuntimeError                              Traceback (most recent call last)

    <ipython-input-11-26814ed17a01> in <module>()
    ----> 1 raise
    

    RuntimeError: No active exception to reraise



```python
try:
    raise
except Exception as e:
    print("예외 발생.", e)
```

    예외 발생. No active exception to reraise
    

다음과 같이 `NameError`를 발생시키고 그 예외를 처리한다.


```python
try:
    raise NameError
except NameError:
    print("이름 예외가 발생했습니다.")
```

    이름 예외가 발생했습니다.
    

- `raise`를 이용하여 가장 가까운 예외를 다시 발생시킨다.


```python
try:
    1/0
except Exception as e:
    print("예외 발생:", e)
    raise
```

    예외 발생: division by zero
    


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-38-9667373e7d06> in <module>()
          1 try:
    ----> 2     1/0
          3 except Exception as e:
          4     print("예외 발생:", e)
          5     raise
    

    ZeroDivisionError: division by zero



```python
try:
    1/0
except Exception as e:
    print("예외 발생:", e)
    1 + "ㅁ"
    10 / 0
    raise
```

    예외 발생: division by zero
    


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-51-196be53aec0c> in <module>()
          1 try:
    ----> 2     1/0
          3 except Exception as e:
    

    ZeroDivisionError: division by zero

    
    During handling of the above exception, another exception occurred:
    

    TypeError                                 Traceback (most recent call last)

    <ipython-input-51-196be53aec0c> in <module>()
          3 except Exception as e:
          4     print("예외 발생:", e)
    ----> 5     1 + "ㅁ"
          6     10 / 0
          7     raise
    

    TypeError: unsupported operand type(s) for +: 'int' and 'str'


## 사용자 정의 예외

사용자가 예외를 정의해서 자신만의 예외를 사용할 수 있다. 사용자 정의 예외 클래스를 만들기 위해서는 `Exception` 클래스를 상속받아 만든다.


```python
class 내예외(Exception):
    def __init__(self, 매개):
        self.매개 = 매개
        
    def 출력(self):
        print("내예외 출력 함수():", self.매개)

try:
    raise 내예외("내 예외를 발생시킵니다.")
except 내예외 as e:
    print("에러:", e)
    e.출력()
    print(e.args)
except ZeroDivisionError as e:
    print("에러의 매개:", e.args[0])
except:
    print("어떤 예외인지 모르는 예외 발생.")
```

    에러: 내 예외를 발생시킵니다.
    내예외 출력 함수(): 내 예외를 발생시킵니다.
    ('내 예외를 발생시킵니다.',)
    

** 직접하기 **

- 위 `내예외` 클래스 `__init__` 메소드에 매개변수를 추가하여 실행시켜보자.
- `내예외` 클래스를 `Exception` 클래스로부터 상속받지 않으면 어떻게 되는지 살펴보자.

## 파일 입출력

파일 입출력을 위해 파일을 열고 사용하려면` file` 클래스의 객체를 생성한 후` read` , `readline`, `write`와 같은 메소드들을 활용하면 된다. 파일을 열때 파일을 읽는 모드와 쓰는 모드로 따로 지정해 줄 수 있다. 파일을 읽거나 쓰는 일을 모두 마친 후에는, `close` 메소드를 호출하여 파이썬에게 그 파일을 다 사용했다는 것을 알려 주어야 한다.

### open, close

`open(파일이름, 모드)`를 이용하면 `파일이름`이라는 파일을 `모드`라는 형태로 만들어 파일 객체를 반환한다.

| 모드 | 설명 |
| --- | --- |
| `r` | 읽기 모드|
| `w` | 쓰기 모드|
| `a` | 추가 모드|


```python
파 = open('새로운파일.txt', 'w')
파.close()
```

파일을 쓰기 모드로 열게 되면 해당 파일이 이미 존재할 경우 원래 있던 내용이 모두 사라지고, 해당 파일이 존재하지 않으면 새로운 파일이 생성된다. 위의 예에서는 디렉터리에 파일이 없는 상태에서 `새로누파일.txt`를 쓰기 모드인 `'w'`로 열었기 때문에 `새로운파일.txt`라는 이름의 새로운 파일이 현재 디렉터리에 생성되는 것이다.

만약 `새로운파일.txt`라는 파일을 `C:\work`라는 디렉터리에 만들고 싶다면 다음과 같이 작성해야 한다.


```python
파 = open("C:/work/새로운파일.txt", 'w')
파.close()
```

위의 예에서 `파.close()`는 열려 있는 파일 객체를 닫아 주는 역할을 한다. 사실 이 문장은 생략해도 된다. 프로그램을 종료할 때 파이썬 프로그램이 열려 있는 파일의 객체를 자동으로 닫아주기 때문이다. 하지만 `close()`를 사용해서 열려 있는 파일을 직접 닫아 주는 것이 좋다. 쓰기모드로 열었던 파일을 닫지 않고 다시 사용하려고 하면 오류가 발생하기 때문이다.

### 파일에 쓰기

`write` 메소드를 이용해 파일에 쓸 수 있다.


```python
파 = open("새로운파일.txt", 'w')
파.write("안녕하세요.")
파.close()

파 = open("새로운파일.txt", 'r')
일 = 파.read()
print(일)
파.close()
```

    안녕하세요.
    

### 파일에 추가하기

파일을 열 때 모드를 `a`로 하면 추가모드가 된다.


```python
파 = open("새로운파일.txt", 'a')
파.write("추가했습니다.")
파.close()

파 = open("새로운파일.txt", 'r')
일 = 파.read()
print(일)
파.close()
```

    안녕하세요.추가했습니다.
    


```python
청포도 = """
내 고장 칠월은 
청포도가 익어 가는 시절. 

이 마을 전설이 주저리주저리 열리고 
먼 데 하늘이 꿈꾸며 알알이 들어와 박혀,

하늘 밑 푸른 바다가 가슴을 열고 
흰 돛 단 배가 곱게 밀려서 오면, 

내가 바라는 손님은 고달픈 몸으로 
청포(靑袍)를 입고 찾아온다고 했으니, 

내 그를 맞아 이 포도를 따 먹으면 
두 손은 함뿍 적셔도 좋으련,

아이야, 우리 식탁엔 은쟁반에 
하이얀 모시 수건을 마련해 두렴.
"""

# 쓰기위해 파일 열기
파 = open('청포도.txt', 'w')
# 파일에 쓰기
파.write(청포도)
# 파일 닫기
파.close()

#  모드가 설정되어 있지 않으면 기본은 읽기('r')모드
파 = open('청포도.txt')
while True:
    줄 = 파.readline()
    # 0이면 파일의 끝
    if len(줄) == 0:
        break
    # '줄'은 개행 문자를 포함하고 있다.
    print(줄)
# 파일 닫기
파.close()
```

    
    
    내 고장 칠월은 
    
    청포도가 익어 가는 시절. 
    
    
    
    이 마을 전설이 주저리주저리 열리고 
    
    먼 데 하늘이 꿈꾸며 알알이 들어와 박혀,
    
    
    
    하늘 밑 푸른 바다가 가슴을 열고 
    
    흰 돛 단 배가 곱게 밀려서 오면, 
    
    
    
    내가 바라는 손님은 고달픈 몸으로 
    
    청포(靑袍)를 입고 찾아온다고 했으니, 
    
    
    
    내 그를 맞아 이 포도를 따 먹으면 
    
    두 손은 함뿍 적셔도 좋으련,
    
    
    
    아이야, 우리 식탁엔 은쟁반에 
    
    하이얀 모시 수건을 마련해 두렴.
    
    

** 직접하기 **

- 위에서 제목 "청포도"와 저자 "이육사"를 함께 저장하는 프로그램을 작성하시오.

## `with` 문

파일 입출력을 위해서는 `open`과 `close`를 반복해야 한다.

```python
파 = open("짧은인생.txt", 'w')
파.write("인생은 짧다. 그래서 파이썬이 필요한다.")
파.close()
```

이러한 과정을 자동으로 처리할 수 있게 한 것이 `with`문이다.

```python
with open("청포도.txt") as 파:
    for 줄 in 파:
        print(줄)
```

`with`는 [문맥 관리자(context manager)](https://docs.python.org/3/reference/datamodel.html#context-managers)와 관련되어 더 많은 작업을 할 수 있다.


```python
with open("청포도.txt") as 파:
    for 줄 in 파:
        print(줄)
```

    
    
    내 고장 칠월은 
    
    청포도가 익어 가는 시절. 
    
    
    
    이 마을 전설이 주저리주저리 열리고 
    
    먼 데 하늘이 꿈꾸며 알알이 들어와 박혀,
    
    
    
    하늘 밑 푸른 바다가 가슴을 열고 
    
    흰 돛 단 배가 곱게 밀려서 오면, 
    
    
    
    내가 바라는 손님은 고달픈 몸으로 
    
    청포(靑袍)를 입고 찾아온다고 했으니, 
    
    
    
    내 그를 맞아 이 포도를 따 먹으면 
    
    두 손은 함뿍 적셔도 좋으련,
    
    
    
    아이야, 우리 식탁엔 은쟁반에 
    
    하이얀 모시 수건을 마련해 두렴.
    
    
