
# 문서 데이터

## HTTP

HTTP는 **H**yper**T**ext **T**ransfer **P**rotocol의 약자로 인터넷을 통해 데이터를 주고 받기 위한 서버/클라이언트 모델을 따르는 프로토콜이다. 응용 수준의 규약(프로토콜)으로 TCP/IP 위에서 작동한다. HTTP는 HTML, 이미지, 동영상, 오디오, 텍스트 문서 등의 데이터를 전송할 수 있다.

웹 브라우저(클라이언트)를 통해 웹 사이트(서버)에 접속하여 원하는 정보를 보는 절차는 다음 그림과 같다.

<img src="images/http_client_server.png" style="width: 400px"/>

클라이언트에서 요청(request) 메시지를 보내면 서버에는 요청 정보를 처리하여 응답(response) 메시지를 보낸다. 요청 메시지와 응답 메시지 형식은 다음과 같다.

### 요청(request)

다음은 브라우저가 서버에게 요청하는 메시지의 예제이다.

```python
GET / HTTP/1.1\r\n
Host: www.google.com\r\n
User-Agent: python-requests/2.18.4\r\n
Accept-Encoding: gzip, deflate\r\n
Accept: */*\r\n
Connection: keep-alive\r\n
\r\n
```

요청 메시지는 요청 줄과 헤더, 메시지 몸통으로 구성되어 있다. 요청 줄은 요청 메소드(get, post, put, delete, head, options, trace), 요청 uri, http 버전이 나온다.
```python
GET / HTTP/1.1\r\n
```

#### 요청 메소드

메소드는 서버에게 어떤 종류의 요청인지 알려준다.

| 메소드 | 설명|
| --- | --- |
| GET | 서버에 정보를 요청한다.|
| POST | 폼(데이터)을 서버에 넘겨주기 위해서 사용한다.|
| PUT | 서버의 데이터를 업데이트하기 위해 사용한다.|
| DELETE | 서버 데이터를 삭제하기 위해서 사용한다.|
| HEAD | HTTP 헤더 정보만 요청한다. 해당 자원이 존재하는지 혹은 서버에 문제가 없는지 확인하기 위해서 사용한다.|
| OPTIONS | 웹서버가 지원하는 메소드의 종류를 알아본다.|
| TRACE | 클라이언트의 요청을 그대로 돌려보낸다.|

#### 요청 URI

통합 자원 식별자(Uniform Resource Identifier, URI)는 정보가 서버에 위치해 있는 주소를 가리킨다. 위의 예에서 `/`가 URI이다.

#### 요청 헤더

요청 줄 다음으로 나오는 것은 요청 헤더이다. 헤더는 브라우저 종류, 웹 페이지 언어, 서버 주소 등의 메타 데이터가 나온다. 자세한 것은 [HTTP/1.1 세부 명세](https://tools.ietf.org/html/rfc2616)를 확인한다.

### 응답(response)

브라우저를 통해서 서버에 요청을 하면 서버가 응답을 하면서 메시지를 보낸다. 아래는 응답 메시지 예제이다.

```python
HTTP/1.1 200 OK
'Date': 'Mon, 20 Nov 2017 01:11:02 GMT',
'Expires': '-1',
'Cache-Control':'private, max-age=0',
'Content-Type': 'text/html; charset=EUC-KR',
'P3P': 'CP="This is not a P3P policy! See g.co/p3phelp for more info."',
'Content-Encoding': 'gzip', 'Server': 'gws',
'Content-Length': '5065',
'X-XSS-Protection': '1; mode=block',
'X-Frame-Options': 'SAMEORIGIN',
'Set-Cookie': '1P_JAR=2017-11-20-01; expires=Wed, 20-Dec-2017 01:11:02 GMT; path=/; domain=.google.co.kr, NID=117=qpfOPSGFis-r3WpI7ejpkONxTkZj0W0LjhFgCJSmY3S4rGi2RFBjxHEvB_JvaWFw9OqLdP7TPDp9RxTU0VeVaJ0F5pR7jgcdmIFmL9G_XpGCnuotMM7p3V7yhxU-p3Kf; expires=Tue, 22-May-2018 01:11:02 GMT; path=/; domain=.google.co.kr; HttpOnly'
```

웹 브라우저를 이용하면 ***개발자 도구 메뉴***를 통해서 `http` 요청/응답에 대한 정보를 확인할 수 있다. 크롬인 경우는 `F12`를 눌러 개발자 도구를 열어 `Network` 탭을 클릭해보면 요청, 응답 메시지를 확인할 수 있다.

** 직접하기 **

- 크롬을 이용하여 고려대학교 세종 홈페이지[(http://sejong.korea.ac.kr/kr)](http://sejong.korea.ac.kr/kr)의 요청/응답 메시지를 확인하자.

## 데이터 수집 및 파싱(Parsing)

### 웹 페이지 읽기

[`requests`](http://docs.python-requests.org/en/master/) 모듈을 이용하여 웹 페이지를 읽어 온다. `requests` 모듈은 `http` 요청및 응답을 처리하는 편리한 방법들을 제공한다.

#### 다음(daum) 홈페이지 출력

다음(daum) 홈페이지에 접속해서 HTML 문서를 가져와 화면에 출력하는 예이다.


```python
import requests

resp = requests.get('http://daum.net') # 웹 사이트 접속
 
if (resp.status_code == requests.codes.ok): # 응답이 정상
    html = resp.text # 웹 페이지 읽기
    print(html.split('\n')[0:10]) # 웹 페이지 10줄 출력
```

    ['<!DOCTYPE html>', '<html lang="ko" class="">', '<head>', '<meta charset="utf-8"/>', '<title>Daum</title>', '<meta property="og:url" content="https://www.daum.net/">', '<meta property="og:type" content="website">', '<meta property="og:title" content="Daum">', '<meta property="og:image" content="//i1.daumcdn.net/svc/image/U03/common_icon/5587C4E4012FCD0001">', '<meta property="og:description" content="나의 관심 콘텐츠를 가장 즐겁게 볼 수 있는 Daum">']
    

`requests.get(사이트주소)`은 요청 메시지의 `get` 메소드를 이용하여 사이트 주소의 페이지를 요청한다. `resp.status_code`는 응답 객체의 상태를 나타내는 것으로 정상이면 `200`을 반환한다. `requests.codes.ok`는 정상 코드 `200`을 나타내는 상수이다. `resp.text`은 웹 페이지의 `html` 페이지를 반환한다. 클라이언트의 잘못된 요청에 대해 서버는 여러 가지 에러를 반환할 수 있다.(에러 코드 4xx, 5xx) 이러한 응답에 대해서 `Response.raise_for_status()` 메소드를 이용해 예외를 발생시킬 수 있다.

#### 구글 검색 결과 출력

구글에 접속해서 원하는 단어를 검색하여 출력할 수 있다. 구글에서 검색어를 입력하면 주소창에 `search?q=검색어`와 같은 문자열이 입력되어 있는 것을 확인할 수 있다. 이것을 이용해 다음과 같이 직접 검색어를 사이트 주소와 함께 입력해 주면 검색 결과를 얻을 수 있다.


```python
import requests

resp = requests.get('https://google.co.kr/search?q=인공지능')
if (resp.status_code == requests.codes.ok):
    html = resp.text
    print(html[:100], '...중간 생략...', html[-100:], sep='\n')
```

    <!doctype html><html itemscope="" itemtype="http://schema.org/SearchResultsPage" lang="ko"><head><me
    ...중간 생략...
    "/client_204?&atyp=i&biw="+a+"&bih="+b+"&ei="+google.kEI);}).call(this);})();</script></body></html>
    

인공지능이란 단어를 검색한 결과를 출력한 것이다. 내용이 너무 길어 중간 생략을 했다.

** 직접하기 **

- 다음(daum) 사이트에서 "날씨"를 검색하여 결과를 출력하시오.

### 파싱(Beautiful Soup)

[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 모듈을 이용해서 웹 페이지에서 필요한 정보들을 찾아낼 수 있다. 포털 사이트에서 주요 뉴스 제목을 찾아내거나 검색 사이트에서 원하는 단어를 검색한 결과를 볼 수 있다.

#### 설치

```python
pip install beautifulsoup4 # 또는
conda install -c anaconda beautifulsoup4 # 아나콘다를 이용할 경우
```

#### BeautifulSoup 웹페이지 파싱

웹 문서를 입력받아 bs객체를 만든다. bs 객체를 이용하여 필요한 정보들에 접근해서 원하는 것들을 수집할 수 있다. 원하는 성분으로 접근하는 방법은 여러 가지가 있으나 `select()` 메소드를 이용하는 방법이 있다. `select` 메소드의 인자는 css selector 조합 문자열을 사용한다. css selector에 대한 자세한 설명은 [W3 Schools CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.asp)를 참조한다. 다음은 몇 가지 예를 보여준다.

| Selector | 예제 | 설명 | CSS 버전 |
| ---- | ---- | ----- | ---- | ----- |
|`.class` | `.intro` | `class="intro"`인 모든 성분 선택 | 1 |
|`#id` | `#firstname` | `id="firstname"`인 모든 성분 선택 | 1 |
| `*` | `*` | 모든 성분 선택 | 2 |
|`element` | `p` | `<p>` 성분 모두 선택 | 1 |
|`element, element` | `div, p` | `<div>` 또는 `<p>`를 갖는 모든 성분 선택 | 1 |
|`element element` | `div p` | `<div>` 성분 안에 모든 `<p>` 성분 선택| 1 |
|`element>element` | `div > p` | 부모가 `<div>`인 모든 `<p>` 성분 선택 | 2 |
|`element+element` | `div + p` | `<div>`성분 바로 다음 `<p>` 성분 선택 | 2 |
|`element1~element2` | `p ~ ul`| `<p>` 바로 다음에 있는 `<ul>` 성분들 선택 | 3 |
|`[attribute]` | `[target]` | 속성이 `target`인 모든 성분 선택 | 2 |
|`[attribute=value]` | `[target=_blank]` | 속성이 `target`이고 `target`의 값이 `_blank`인 모든 성분 선택 | 2 |
|`[attribute~=value]` | `[title~=flower]` | `title`속성을 갖고 속성값이 `flower`단어를 포함하는 모든 성분들 선택 | 2 |
|`[attribute` &#124;`=value`] | `[lang` &#124; `=en`] | 속성이 `lang`이고 속성의 값이 `en`으로 시작하는 모든 성분 선택 | 2 |
|`:nth-of-type(n)` | `p:nth-of-type(2)` | `<p>`의 부모의 두번째 `<p>`성분 선택 | 3|

```python
import bs4
 
html = "<html><head><title>제목</title></head><body>...생략...</body></html>"
bs = bs4.BeautifulSoup(html, 'html.parser')
```


```python
import bs4

html = """
<html>

<head>
</head>

<body>
<h1>Welcome to My Homepage</h1>
<div class="intro">
<p>My name is Donald <span id="Lastname">Duck.</span></p>

<p id="my-Address">I live in Duckburg</p>

<p>I have many friends:</p>
</div>

<ul id="Listfriends">
<li>Goofy</li>
<li>Mickey</li>
<li>Daisy</li>
<li>Pluto</li>
</ul>

<p>All my friends are great!<br>
But I really like Daisy!!</p>

<p lang="it" title="Hello beautiful">Ciao bella</p>

<h3>We are all animals!</h3>

<p><b>My latest discoveries have led me to believe that we are all animals:</b></p>

<table>
<thead>
<tr>
<th>Name</th>	<th>Type of Animal</th>
</tr>
</thead>
<tr>
<td>Mickey</td>	<td>Mouse</td>
</tr>
<tr>
<td>Goofey</td>	<td>Dog</td>
</tr>
<tr>
<td>Daisy</td>	<td>Duck</td>
</tr>
<tr>
<td>Pluto</td>	<td>Dog</td>
</tr>
</table>

</body>
</html>
"""

bs = bs4.BeautifulSoup(html, 'html.parser')
bs.select('table')
```




    [<table>
     <thead>
     <tr>
     <th>Name</th> <th>Type of Animal</th>
     </tr>
     </thead>
     <tr>
     <td>Mickey</td> <td>Mouse</td>
     </tr>
     <tr>
     <td>Goofey</td> <td>Dog</td>
     </tr>
     <tr>
     <td>Daisy</td> <td>Duck</td>
     </tr>
     <tr>
     <td>Pluto</td> <td>Dog</td>
     </tr>
     </table>]



다음 html 문서를 이용해서 예제들을 살펴보자.


```python
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
</body>
</html>
"""
```

- 성분들을 찾는다.


```python
soup = bs4.BeautifulSoup(html_doc, 'html.parser')
soup.select('title')
```




    [<title>The Dormouse's story</title>]




```python
soup.select("p:nth-of-type(3)")
```




    [<p class="story">...</p>]



- 성분 밑의 성분 찾기


```python
soup.select("body a")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]




```python
soup.select("html head title")
```




    [<title>The Dormouse's story</title>]



- 성분 바로 밑의 성분 찾기


```python
soup.select("head > title")
```




    [<title>The Dormouse's story</title>]




```python
soup.select("p > a")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]




```python
soup.select("p > a:nth-of-type(2)")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]




```python
soup.select("p > #link1")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]




```python
soup.select("body > a")
```




    []



- 같은 수준의 성분들 찾기


```python
soup.select("#link1 ~ .sister")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]




```python
soup.select("#link1 + .sister")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]



- CSS 클래스에 의한 성분 찾기


```python
soup.select(".sister")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]




```python
soup.select("[class~=sister]")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



- ID에 의한 성분 찾기


```python
soup.select("#link1")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]




```python
soup.select("a#link2")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]



- 속성에 의해 찾기


```python
soup.select('a[href]')
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



- 속성값에 의한 찾기


```python
soup.select('a[href="http://example.com/elsie"]')
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]




```python
soup.select('a[href^="http://example.com/"]')
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]




```python
soup.select('a[href$="tillie"]')
```




    [<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]




```python
soup.select('a[href*=".com/el"]')
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]



#### 네이버 금융 사이트에서 헤드라인 뉴스 제목 발췌


```python
import requests, bs4
 
resp = requests.get('http://finance.naver.com/')
resp.raise_for_status()
 
resp.encoding='euc-kr'
html = resp.text
 
bs = bs4.BeautifulSoup(html, 'html.parser')
print(bs.prettify()[0:100], "\n.\n.\n.\n", bs.prettify()[-100:])

tags = bs.select('div.news_area h2 a') # 헤드라인 뉴스 제목
title = tags[0].getText()
print("헤드라인 제목: ", title)
```

    <html lang="ko">
     <head>
      <title>
       네이버 금융
      </title>
      <meta content="text/html; charset=utf-8" h 
    .
    .
    .
     , 이미지 리플레시 
    jindo.$Fn(mainPageDomReadyFn).attach(document, "domready");
      </script>
     </body>
    </html>
    헤드라인 제목:  원화강세기에는 '경기민감주'
    

** 직접하기 **

- 다음(daum) 사이트의 실시간 이슈 검색어를 추출해 보시오.
- 네이버 사이트에서 [코스피 실시간 지수](http://finance.naver.com/sise/sise_index.nhn?code=KOSPI)를 출력하시오.
- [네이버 환율 사이트](http://info.finance.naver.com/marketindex/?tabSel=exchange#tab_section)에서 엔화 현찰 살때 팔때 환율을 출력하시오. `iframe`으로 연결되어 있어서 사이트 주소를 정확히 입력해야 한다.

## 분석

### pandas
`pandas` 모듈은 데이터를 다루기 편리한 함수들을 제공한다.

#### 생성

- 엑셀 데이터 읽기


```python
import pandas as pd

df_excel = pd.read_excel('http://qrc.depaul.edu/Excel_Files/Presidents.xls')
```

- 웹페이지 표 읽기(행정안전부 지방 물가 정보)

행정안전부 지방물가 정보 사이트 [http://www.mois.go.kr/frt/sub/a02/mulMain/screen.do](http://www.mois.go.kr/frt/sub/a02/mulMain/screen.do)에서 농축산물 전월 평균 가격정보를 가져오자.


```python
import bs4, requests
import pandas as pd

행정 안전부 지방 물가 정보 - 농축산물
site = 'http://www.mois.go.kr/frt/sub/a02/farmProductPriceList/screen.do'

resp = requests.get(site)
html = resp.text
bs = bs4.BeautifulSoup(html, 'html.parser')
iframes = bs.select('iframe')
for iframe in iframes:
    frame_site = iframe.attrs['src']
    dfs = pd.read_html(frame_site, na_values=['-'])
df = dfs[0]
print(df)
```

pd.read_html('site address')을 이용하여 웹 페이지에 있는 표(table)를 pandas DataFrame 리스트로 변환한다.

- 데이터 프레임 내용 출력

df['구분'] # Series 형으로 출력

df[['구분', '쇠고기', '감자']] # DataFrame형 출력

- 값 출력

```
df_excel['Political Party'].value_counts()
```

#### 기본 통계

- 요약

`describe()` 함수를 이용하여 기본적인 통계량을 관찰할 수 있다. `describe(include='all')`을 이용해서 모든 열에 대해서 통계량을 관찰할 수 있다.

df_excel.describe()

df_excel.describe(include='all')

- 열별 평균, 합계
`mean()` 함수를 이용하여 평균을 구할 수 있다. 숫자형에 대해서만 계산한다. `sum()`을 이용하여 열별 합계를 구할 수 있다. 문자열일 경우 각 행의 문자열들을 연결한다.

df.mean() # Series형 반환

df.sum()

`df.ix[행슬라이싱, 열슬라이싱]` 을 이용하여 파이썬 슬라이싱 문법을 사용할 수 있다. 또한 이름으로도 가능한다. `df[:, '쇠고기':'닭고기']`를 이용하여 `쇠고기` 열부터 `닭고기` 열까지를 잘라낼 수 있다.

df.ix[:, 1:].sum()

- 정렬
`sort_values(by=['colname'])`을 이용해서 지정된 열로 데이터프레임을 정렬할 수 있다.

df.sort_values(by=['쇠고기'])

## 시각화

get_ipython().magic('matplotlib inline')
import matplotlib.pyplot as plt
import numpy as np

- line

import numpy as np

df.set_index('구분').plot(kind='line', xticks=np.arange(len(df['구분'])), rot=90)

`xticks=np.arange(16)`는 xtick이 보여질 위치를 지정하는 것이다.

- boxplot

get_ipython().magic('matplotlib inline')
from matplotlib import font_manager, rc

font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/NanumGothicBold.ttf").get_name()
rc('font', family=font_name)

df.boxplot()

- 파이 그래프

get_ipython().magic('matplotlib inline')
df_excel['Political Party'].value_counts().plot(kind="pie")

- 바차트

df_excel['Political Party'].value_counts().plot(kind="bar")

## 참고 사이트

- Beautiful Soup: https://www.crummy.com/software/BeautifulSoup/bs4/doc/#
- requests: http://pythonstudy.xyz/python/article/403-%ED%8C%8C%EC%9D%B4%EC%8D%AC-Web-Scraping
- urllib: https://www.acmicpc.net/blog/view/16
- Python for Data Analysis by Wes McKinney: https://github.com/wesm/pydata-book
- http://www.hanbit.co.kr/channel/category/category_view.html?cms_code=CMS9481416663
- Python for Data Analysis by Wes McKinney: https://github.com/wesm/pydata-book
- Pandas Documentation: http://pandas.pydata.org/pandas-docs/stable/
