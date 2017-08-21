
# 소개
파이썬 pandas 모듈을 이용하여 데이터를 편리하게 가공할 수 있다. 일차원 데이터를 다루기 위한 Series 형과 이차원 데이터를 다루기 위한 DataFrame형이 있다.

- 시리즈(Series)는 index로 구성된다.
- 데이터프레임(DataFrmae)의 색인은 행을 표현하는 index와 열을 대표하는 columns로 구성된다.

# 시리즈(Series)
일차원 데이터를 다루기 위해서 사용된다.

## 생성

- 파이썬 dict 형을 이용


```python
import pandas as pd

srs = pd.Series({'seoul': 2000, 'cheonan': 3000})
print(srs)
```

    cheonan    3000
    seoul      2000
    dtype: int64
    

dict의 key가 시리즈의 인덱스가 되고 dict의 value는 시리즈의 값이 된다.

# 데이터프레임(DataFrame)
이차원 데이터를 다루기 위해서 사용된다. 여러 개의 열로 구성되며 하나의 열을 구성하는 원소들의 데이터의 형은 동일하다.

## 생성

- 같은 크기의 리스트를 값으로 갖는 사전을 이용해서 만들수 있다.


```python
import pandas as pd
import numpy as np

data = {'city': ['seoul', 'cheonan', 'daejeon', 'busan'],
        'year': [2000, 2011, 2008, 2009],
        'pop': [1.5, 0.3, 0.8, 0.9]}
df = pd.DataFrame(data)
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>pop</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>seoul</td>
      <td>1.5</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>cheonan</td>
      <td>0.3</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>2</th>
      <td>daejeon</td>
      <td>0.8</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>3</th>
      <td>busan</td>
      <td>0.9</td>
      <td>2009</td>
    </tr>
  </tbody>
</table>
</div>



사전의 키 'city', 'year', 'pop'들이 데이터프레임의 열이름이 되고, 사전의 value들인 리스트 ['seoul', 'cheonan', 'daejeon', 'busan'], [2000, 2011, 2008, 2009], [1.5, 0.3, 0.8, 0.9]가 데이터프레임의 열이 된다. 여기서 리스트의 크기는 동일해야 한다.

## 접근
### 행
df[슬라이스]


```python
df[:2]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>pop</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>seoul</td>
      <td>1.5</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>cheonan</td>
      <td>0.3</td>
      <td>2011</td>
    </tr>
  </tbody>
</table>
</div>



주의: df[1]과 같이 스칼라로 접근할 수 없다.

### 열
df[일차원 정수 리스트 또는 np.array]


```python
df[[-1]]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2011</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2008</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2009</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[np.arange(2)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>seoul</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>cheonan</td>
      <td>0.3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>daejeon</td>
      <td>0.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>busan</td>
      <td>0.9</td>
    </tr>
  </tbody>
</table>
</div>



### df.ix[행, 열]
행, 열은 리스트, 일차원 np.array, 행 인덱스, 열 이름등이 올 수 있다.

- 열들의 타입을 알아보기 위해서는 df.dtypes를 치면된다.

## 변경

### replace
- df.replace('원래 문자열', '대체될 문자열')
- df.replace('원래 문자열', numpy.na) # NaN으로 대체된다.

# 참고 문헌
- [Python for Data Analysis by Wes McKinney, 2012](http://oreil.ly/python_for_data_analysis)
