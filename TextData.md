
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

웹 문서를 입력받아 bs객체를 만든다. bs 객체를 이용하여 필요한 정보들에 접근해서 원하는 것들을 수집할 수 있다. 원하는 성분으로 접근하는 방법은 여러 가지가 있으나 `select()` 메소드를 이용하는 방법이 있다. `select` 메소드의 인자는 [CSS(Cascading Style Sheets)](https://www.w3schools.com/css/default.asp) selector 조합 문자열을 사용한다. css selector에 대한 자세한 설명은 [W3 Schools CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.asp)를 참조한다. 다음은 몇 가지 예를 보여준다.

html 성분(element 또는 tag)은 다음과 같은 형식으로 이루어져 있다.

```html
<tag_or_element attribute="value">text</tag_or_element>
```

다음은 html 예제의 일부이다.

```html
<div class="intro"> <!-- div는 성분, class는 속성, "intro"는 class 속성값이다.-->
<p>My name is Donald <span id="Lastname">Duck.</span></p>

<p id="my-Address">I live in Duckburg</p>

<p>I have many friends:</p>
</div>
```

| Selector | 예제 | 설명 | CSS 버전 |
| ---- | ---- | ----- | ---- | ----- |
|`.class` | `.intro` | `class="intro"`인 모든 성분 선택 | 1 |
|`#id` | `#firstname` | `id="firstname"`인 모든 성분 선택 | 1 |
| `*` | `*` | 모든 성분 선택 | 2 |
| `element *` | `*` | `div` 안에 있는(자손) 모든 성분 선택. 중복하면서 선택된다. | 2 |
|`element` | `p` | `<p>` 성분 모두 선택 | 1 |
|`element, element` | `div, p` | `<div>` 또는 `<p>`를 갖는 모든 성분 선택. 중복을 허락하지 않는다. | 1 |
|`element element` | `div p` | `<div>` 성분 안에(자식) 모든 `<p>` 성분 선택| 1 |
|`element > element` | `div > p` | 부모가 `<div>`인 모든 `<p>` 성분 선택 | 2 |
|`element + element` | `div + p` | `<div>`와 형제이며 `<div>` 바로 아래쪽에 붙어 있는 `<p>` 성분 선택 | 2 |
|`element1 ~ element2` | `p ~ ul`| `<p>` 와 형제이며 `<p>` 아래쪽에 있는 모든 `<ul>` 성분들 선택 | 3 |
|`[attribute]` | `[target]` | 속성이 `target`인 모든 성분 선택 | 2 |
|`[attribute=value]` | `[target=_blank]` | 속성이 `target`이고 `target`의 값이 `_blank`인 모든 성분 선택 | 2 |
|`[attribute~=value]` | `[title~=flower]` | `title`속성을 갖고 속성값이 `flower`를 포함하는 모든 성분들 선택 | 2 |
|`[attribute`&#124;`=value`] | `[lang`&#124;`=en`] | 속성이 `lang`이고 속성의 값이 `en` 또는 `en-`로 시작하는 모든 성분 선택 | 2 |
|`:nth-of-type(n)` | `p:nth-of-type(2)` | `<p>`의 부모 아래에 있는 두번째 `<p>`성분 선택 | 3|

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

** 성분들을 찾는다. **


```python
soup = bs4.BeautifulSoup(html_doc, 'html.parser')
soup.select('title')
```




    [<title>The Dormouse's story</title>]



성분이 `title`인 것을 모두 찾는다.


```python
soup.select("p:nth-of-type(3)")
```




    [<p class="story">...</p>]



`p`의 부모의 자손중 3번째 `p`를 찾는다.

**  성분 밑의 성분 찾기 **


```python
soup.select("body a")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



`body`의 자손 중 `a` 성분을 모두 찾는다.


```python
soup.select("html head title")
```




    [<title>The Dormouse's story</title>]



`html` 자손으로 `head` 자손 중 `title` 성분을 모두 찾는다.

**성분 바로 밑의 성분 찾기**


```python
soup.select("head > title")
```




    [<title>The Dormouse's story</title>]



`head` 성분의 자식 중 `title` 성분을 모두 찾는다.


```python
soup.select("p > a")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



`p`의 자식 중 `a`인 성분 모두 찾는다.


```python
soup.select("p > a:nth-of-type(2)")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]



`p`의 자식 중의 `a` 성분들 중에서 2번째 성분을 찾는다.


```python
soup.select("p > #link1")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]



`p`의 자식 중 `id`가 `link1`인 성분을 찾는다.


```python
soup.select("body > a")
```




    []



`body` 자식 중 `a` 성분을 찾지만 없으므로 빈 리스트가 된다.

**같은 수준의 성분들 찾기**


```python
soup.select("#link1 ~ .sister")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



`id`가 `link1`인 형제들 중 `class` 값이 `sister`인 모든 성분 찾는다.


```python
soup.select("#link1 + .sister")
```




    [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]



`id`가 `link1`인 형제들 중 `id`가 `link1`인 성분 바라 아래 붙어있는 `class` 값이 `sister`인 성분을 찾는다.

**CSS 클래스에 의한 성분 찾기**


```python
soup.select(".sister")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



클래스 값이 `sister`인 성분들 모두 찾는다.


```python
soup.select("[class~=sister]")
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



클래스 **속성값**이 `sister`를 포함하는 모든 성분을 찾는다.

**ID에 의한 성분 찾기**


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

### 셀레늄(Selenium)

[Selenium](http://www.seleniumhq.org)은 웹 브라우저의 기능을 하도록 하는 모듈이다. 브라우저를 직접 실행하지 않고 selenium 메소드들을 이용해서 웹 브라우저 기능을 대신할 수 있게 한다. Selenium은 Selenium 2(Selenium WebDriver), Selenium 1(Selenium RC), Selenium IDE, Selenium-Grid 툴로 이루어 졌다. 우리가 사용하는 것은 Selenium 2(Selenium WebDriver)이다. 이것은 프로그래밍 언어(Java, C#, Python, Javascript등)에 맞는 인터페이스를 제공하여 프로그래밍을 이용하여 사용하기 편리하다. [Selenium 2](http://www.seleniumhq.org/docs/03_webdriver.jsp)를 이용하기 위해서는 웹 브라우저에 맞는 드라이버를 다운로드 해야 한다. 여기서는 웹 브라우저가 없이도 사용할 수 있는 [PhantomJS](http://phantomjs.org/quick-start.html)를 이용한다. 파이썬에서 사용하는 selenium에 대한 문서는 [http://selenium-python.readthedocs.io/index.html](http://selenium-python.readthedocs.io/index.html)을 참고한다. 더 자세한 사용법은 [Selenium 파이썬 웹드라이버 API](http://selenium-python.readthedocs.io/api.html)를 참조하자.

#### 드라이버 다운로드

`phantomjs` 드라이버를 인터넷으로부터 다운받아 작업 디렉토리 아래 `drivers` 폴더에 넣는다. 다운로드하는데 약간의 시간이 걸린다.


```python
import urllib.request
import os

directory = 'drivers'
if not os.path.exists(directory):
    os.makedirs(directory)

url = 'https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-windows.zip'

fpath = directory + '/' + 'phantomjs-2.1.1-windows.zip'

if not os.path.exists(fpath):
    urllib.request.urlretrieve(url, fpath)
```

#### 압축해제

다운받은 파일을 압축해제한다.


```python
import zipfile
zip_ref = zipfile.ZipFile(fpath, 'r')
zip_ref.extractall(directory)
zip_ref.close()
```

#### 압축해제된 경로 연결


```python
filename = os.path.split(url)[1] # 파일 이름 추출
file_ext = os.path.splitext(filename) # 파일이름과 확장자로 분리

phantom_path = directory + '/' + file_ext[0] + '/bin/phantomjs.exe'
```

#### 간단한 사용법

먼저 driver를 설정한다. 드라이버는 웹 브라우저에 해당하는 것이라고 생각할 수 있다. 드라이버는 브라우저의 종류에 따라 설치되어 있어야 한다. 위에서 PhantomJS 드라이버를 설치했다. 드라이버 연결할 때는 드라이버의 위치를 알려주는 방법과 운영체제의 경로에 있으면 된다.


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

chrome_path = 'drivers/chromedriver.exe'

os.path.exists(chrome_path)
driver = webdriver.Chrome(chrome_path)
driver.get("http://www.python.org")
assert "Python" in driver.title
elem = driver.find_element_by_name("q")
elem.clear()
elem.send_keys("pycon")
elem.send_keys(Keys.RETURN)
assert "No results found." not in driver.page_source
driver.close()
```

#### 사이트에서 원하는 자료 가져오기

셀레늄을 이용해서 [행정안전부 지방물가정보](http://www.mois.go.kr/frt/sub/a02/farmProductPriceList/screen.do) 사이트에 있는 2017년 10월 농축산물 평균가격을 가져와보자. 페이지 소스 보기를 하면 웹 페이지 상에 보이던 표가 보이지 않는 것을 알 수 있다. 이것은 표를 보여주는 부분이 [`iframe`](https://www.w3schools.com/tags/tag_iframe.asp)으로 처리되었기 때문이다. `iframe`은 inline frame으로 다른 위치에 있는 웹 페이지를 현재 위치에 보이게 하는 것이다. 따라서 `iframe` 위치로 이동하는 것이 필요하다.


```python
from selenium import webdriver
from selenium.webdriver.support.ui import Select
import pandas as pd

# PhantomJS 드라이버 설정
driver = webdriver.PhantomJS(executable_path=phantom_path)

# 사이트에서 웹 문서 수집
site = 'http://www.mois.go.kr/frt/sub/a02/farmProductPriceList/screen.do'
driver.get(site)

# iframe 으로 이동
iframe = driver.find_element_by_css_selector('iframe')
driver.switch_to.frame(iframe)

# 2017년 10월 클릭
elem = driver.find_element_by_id('year')
select = Select(elem)
select.select_by_value("2017")
elem = driver.find_element_by_id('month')
select = Select(elem)
select.select_by_value("10")

driver.find_element_by_id('srch').click()
html = driver.page_source

driver.close()

bs = bs4.BeautifulSoup(html, 'html.parser')
tables = bs.select('div > table > tbody')
rows = tables[0].find_all('tr')
for row in rows:
    cols = row.find_all('td')
    cols = [ele.text.strip() for ele in cols]
    print(cols)
```

    ['서울', '9,864', '2,619', '6,353', '2,281', '3,330', '1,769', '3,622', '3,329', '11,153', '48,638']
    ['부산', '9,382', '2,370', '5,962', '3,497', '5,046', '1,961', '2,929', '3,329', '9,601', '43,952']
    ['대구', '9,428', '2,455', '6,423', '2,447', '4,345', '1,691', '3,428', '3,071', '9,373', '41,517']
    ['인천', '8,691', '2,438', '5,652', '2,179', '3,797', '1,622', '3,761', '2,783', '9,317', '42,322']
    ['광주', '10,433', '2,447', '5,219', '2,346', '4,240', '2,061', '3,730', '3,382', '9,868', '43,047']
    ['대전', '8,013', '2,599', '5,091', '3,169', '2,905', '1,589', '3,876', '4,131', '9,329', '42,780']
    ['울산', '9,160', '2,196', '6,485', '2,587', '4,608', '1,935', '3,581', '3,957', '10,523', '42,880']
    ['경기', '9,627', '2,389', '5,917', '2,542', '4,263', '1,860', '3,713', '3,304', '9,848', '44,653']
    ['강원', '9,251', '2,410', '5,627', '2,208', '4,249', '1,871', '3,186', '3,445', '8,535', '45,319']
    ['충북', '9,851', '2,643', '5,550', '2,908', '4,044', '2,019', '3,132', '3,279', '9,297', '42,971']
    ['충남', '8,258', '2,146', '5,626', '2,400', '4,161', '1,914', '3,008', '3,784', '8,169', '44,093']
    ['전북', '8,892', '2,196', '5,353', '2,954', '3,270', '1,857', '3,557', '3,509', '8,580', '42,440']
    ['전남', '8,716', '2,098', '5,391', '3,072', '4,268', '2,188', '3,217', '3,210', '7,373', '40,663']
    ['경북', '7,675', '2,136', '5,941', '2,953', '4,611', '2,032', '3,006', '2,588', '9,240', '41,505']
    ['경남', '8,795', '2,165', '5,327', '2,960', '3,904', '1,917', '3,189', '3,027', '8,056', '41,187']
    ['제주', '8,425', '2,528', '6,298', '2,860', '5,522', '2,070', '2,910', '3,087', '8,152', '43,017']
    

** 직접하기 **

- 행정안전부 지방물가정보 사이트의 [지방 공공 요금](http://www.mois.go.kr/frt/sub/a02/publicPriceList/screen.do) 페이지에서 **2016년 1월** 평균요금을 출력하시오.
- 평균요금을 숫자로 바꾸시오.
- [고려대 세종 캠퍼스 홈페이지](http://sejong.korea.ac.kr/kr)에 있는 셔틀버스 시간표를 출력하시오.
- 네이버 로그인을 해서 이메일 제목을 출력하시오.

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

참고: `pd.read_html(웹페이지)`은 웹 페이지에 있는 표(table)를 pandas DataFrame **리스트**로 변환한다.

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
