
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



**속성에 의해 찾기**


```python
soup.select('a[href]')
```




    [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
     <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
     <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]



**속성값에 의한 찾기**


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
    헤드라인 제목:  조정받는 코스닥, 반등하는 코스피.....
    

** 직접하기 **

- 다음(daum) 사이트의 실시간 이슈 검색어를 추출해 보시오.
- 네이버 사이트에서 [코스피 실시간 지수](http://finance.naver.com/sise/sise_index.nhn?code=KOSPI)를 출력하시오.
- [네이버 환율 사이트](http://info.finance.naver.com/marketindex/?tabSel=exchange#tab_section)에서 엔화 현찰 살때 팔때 환율을 출력하시오. `iframe`으로 연결되어 있어서 사이트 주소를 정확히 입력해야 한다.

### 셀레늄(Selenium)

[Selenium](http://www.seleniumhq.org)은 웹 브라우저의 기능을 하도록 하는 모듈이다. 브라우저를 직접 실행하지 않고 selenium 메소드들을 이용해서 웹 브라우저 기능을 대신할 수 있게 한다. Selenium은 Selenium 2(Selenium WebDriver), Selenium 1(Selenium RC), Selenium IDE, Selenium-Grid 툴로 이루어 졌다. 우리가 사용하는 것은 Selenium 2(Selenium WebDriver)이다. 이것은 프로그래밍 언어(Java, C#, Python, Javascript등)에 맞는 인터페이스를 제공하여 프로그래밍을 이용하여 사용하기 편리하다. [Selenium 2](http://www.seleniumhq.org/docs/03_webdriver.jsp)를 이용하기 위해서는 웹 브라우저에 맞는 드라이버를 다운로드 해야 한다. 여기서는 웹 브라우저가 없이도 사용할 수 있는 [PhantomJS](http://phantomjs.org/quick-start.html)를 이용한다. 파이썬에서 사용하는 selenium에 대한 문서는 [http://selenium-python.readthedocs.io/index.html](http://selenium-python.readthedocs.io/index.html)을 참고한다. 더 자세한 사용법은 [Selenium 파이썬 웹드라이버 API](http://selenium-python.readthedocs.io/api.html)를 참조하자.

#### 설치

아나콘다 프롬프트 창에서 다음과 같이 입력하여 셀레늄을 설치한다.

```python
pip install selenium
```

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

먼저 driver를 설정한다. 드라이버는 웹 브라우저에 해당하는 것이라고 생각할 수 있다. 드라이버는 브라우저의 종류에 따라 설치되어 있어야 한다. 위에서 PhantomJS 드라이버를 설치했다. 드라이버 연결할 때는 드라이버의 위치를 알려주는 방법과 운영체제의 경로에 있으면 된다. 아래는 크롬 브라우저를 이용해서 접근하기 위해 [크롬 드라이버](https://sites.google.com/a/chromium.org/chromedriver/downloads)를 사용했다. 크롬 드라이버를 링크된 사이트에서 최신 버전으로 다운받아 drivers 디렉토리에 압축해제 시킨다. 그러면 `chromedriver.exe` 파일이 생긴다. 이것을 이용해 아래와 같이 사용할 수 있다.


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

chrome_path = 'drivers/chromedriver.exe'

assert os.path.exists(chrome_path)
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

위의 것을 실행하면 새창에서 크롬 브라우저가 뜨고 파이썬 홈페이지에 접속해서 `pycon`을 검색한 후 자동으로 종료된다.

크롬 드라이버 위치를 지정한다.

```python
chrome_path = 'drivers/chromedriver.exe'
```

지정된 경로가 올바르면 통과하고 그렇지 않으면 예외를 발생시키고 프로그램이 종료된다.

```python
assert os.path.exists(chrome_path)
```

크롬 드라이버를 이용해 웹드라이버 인스턴스를 만든다.

```python
driver = webdriver.Chrome(chrome_path)
```

`get()` 메소드를 이용해 사이트에 접속한다.

```python
driver.get("http://www.python.org")
```

접속한 페이지 제목에 `Python`이 있는지 확인한다.

```python
assert "Python" in driver.title
```

웹드라이버는 `find_element_by_*` 메소드를 이용해 성분을 찾아낼 수 있다. `name` 속성을 가진 `input` 성분은  `find_element_by_name` 메소드를 이용해서 찾을 수 있다. 아래는 속성이 `name`이고 속성값이 `q`인 성분을 찾아낸다.

```python
elem = driver.find_element_by_name("q")
```

`clear()`는 텍스트가 있으면 없앤다. `send_keys()` 메소드는 텍스트 입력란에 원하는 텍스트를 입력하는 것이다. `Keys.Return`은 엔터키를 치는 것과 같다.

```python
elem.clear()
elem.send_keys("pycon")
elem.send_keys(Keys.RETURN)
```
`close()` 메소드는 현재 탭을 닫는다. `quit()` 메소드는 현재 창을 닫는다.

```python
driver.close()
```

** 직접하기 **

- 크롬 드라이버를 다운로드받아 압축해제해서 위 프로그램을 실행하시오.

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

`pandas` 모듈은 데이터를 다루기 편리한 메소드들을 제공한다.

#### 생성

** 엑셀 데이터 읽기**


```python
import pandas as pd

df_excel = pd.read_excel('http://qrc.depaul.edu/Excel_Files/Presidents.xls'); df_excel
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>President</th>
      <th>Years in office</th>
      <th>Year first inaugurated</th>
      <th>Age at inauguration</th>
      <th>State elected from</th>
      <th># of electoral votes</th>
      <th># of popular votes</th>
      <th>National total votes</th>
      <th>Total electoral votes</th>
      <th>Rating points</th>
      <th>Political Party</th>
      <th>Occupation</th>
      <th>College</th>
      <th>% electoral</th>
      <th>% popular</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>George Washington</td>
      <td>8</td>
      <td>1789</td>
      <td>57</td>
      <td>Virginia</td>
      <td>69</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>69</td>
      <td>842.0</td>
      <td>None</td>
      <td>Planter</td>
      <td>None</td>
      <td>100.000000</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>1</th>
      <td>John Adams</td>
      <td>4</td>
      <td>1797</td>
      <td>61</td>
      <td>Massachusetts</td>
      <td>132</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>139</td>
      <td>598.0</td>
      <td>Federalist</td>
      <td>Lawyer</td>
      <td>Harvard</td>
      <td>94.964029</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Thomas Jefferson</td>
      <td>8</td>
      <td>1801</td>
      <td>57</td>
      <td>Virginia</td>
      <td>73</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>137</td>
      <td>711.0</td>
      <td>Democratic-Republican</td>
      <td>Planter, Lawyer</td>
      <td>William and Mary</td>
      <td>53.284672</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>3</th>
      <td>James Madison</td>
      <td>8</td>
      <td>1809</td>
      <td>57</td>
      <td>Virginia</td>
      <td>122</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>176</td>
      <td>567.0</td>
      <td>Democratic-Republican</td>
      <td>Lawyer</td>
      <td>Princeton</td>
      <td>69.318182</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>4</th>
      <td>James Monroe</td>
      <td>8</td>
      <td>1817</td>
      <td>58</td>
      <td>Virginia</td>
      <td>183</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>221</td>
      <td>602.0</td>
      <td>Democratic-Republican</td>
      <td>Lawyer</td>
      <td>William and Mary</td>
      <td>82.805430</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>5</th>
      <td>John Quincy Adams</td>
      <td>4</td>
      <td>1825</td>
      <td>57</td>
      <td>Massachusetts</td>
      <td>84</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>261</td>
      <td>564.0</td>
      <td>Democratic-Republican</td>
      <td>Lawyer</td>
      <td>Harvard</td>
      <td>32.183908</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Andrew Jackson</td>
      <td>8</td>
      <td>1829</td>
      <td>61</td>
      <td>Tennessee</td>
      <td>178</td>
      <td>642553</td>
      <td>1148018</td>
      <td>261</td>
      <td>632.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>None</td>
      <td>68.199234</td>
      <td>55.9706</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Martin Van Buren</td>
      <td>4</td>
      <td>1837</td>
      <td>54</td>
      <td>New York</td>
      <td>170</td>
      <td>764176</td>
      <td>1503534</td>
      <td>294</td>
      <td>429.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>None</td>
      <td>57.823129</td>
      <td>50.8253</td>
    </tr>
    <tr>
      <th>8</th>
      <td>William Henry Harrison</td>
      <td>0.8</td>
      <td>1841</td>
      <td>68</td>
      <td>Ohio</td>
      <td>234</td>
      <td>1275390</td>
      <td>2411808</td>
      <td>294</td>
      <td>329.0</td>
      <td>Whig</td>
      <td>Soldier</td>
      <td>Hampden-Sydney</td>
      <td>79.591837</td>
      <td>52.8811</td>
    </tr>
    <tr>
      <th>9</th>
      <td>James K. Polk</td>
      <td>4</td>
      <td>1845</td>
      <td>49</td>
      <td>Tennessee</td>
      <td>170</td>
      <td>1339494</td>
      <td>2703659</td>
      <td>275</td>
      <td>632.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>U. of North Carolina</td>
      <td>61.818182</td>
      <td>49.5437</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Zachary Taylor</td>
      <td>1</td>
      <td>1849</td>
      <td>64</td>
      <td>Louisiana</td>
      <td>163</td>
      <td>1361393</td>
      <td>2879184</td>
      <td>290</td>
      <td>447.0</td>
      <td>Whig</td>
      <td>Soldier</td>
      <td>None</td>
      <td>56.206897</td>
      <td>47.284</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Franklin Pierce</td>
      <td>4</td>
      <td>1853</td>
      <td>48</td>
      <td>New Hampshire</td>
      <td>254</td>
      <td>1607510</td>
      <td>3161830</td>
      <td>296</td>
      <td>286.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>Bowdoin</td>
      <td>85.810811</td>
      <td>50.8411</td>
    </tr>
    <tr>
      <th>12</th>
      <td>James Buchanan</td>
      <td>4</td>
      <td>1857</td>
      <td>65</td>
      <td>Pennsylvania</td>
      <td>174</td>
      <td>1836072</td>
      <td>4054647</td>
      <td>296</td>
      <td>259.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>Dickinson</td>
      <td>58.783784</td>
      <td>45.2832</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Abraham Lincoln</td>
      <td>4</td>
      <td>1861</td>
      <td>52</td>
      <td>Illinois</td>
      <td>180</td>
      <td>1865908</td>
      <td>4685561</td>
      <td>303</td>
      <td>900.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>None</td>
      <td>59.405941</td>
      <td>39.8225</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Ulysses S. Grant</td>
      <td>8</td>
      <td>1869</td>
      <td>46</td>
      <td>Illinois</td>
      <td>214</td>
      <td>3013650</td>
      <td>5722440</td>
      <td>294</td>
      <td>403.0</td>
      <td>Republican</td>
      <td>Soldier</td>
      <td>US Military Academy</td>
      <td>72.789116</td>
      <td>52.6637</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Rutherford B. Hayes</td>
      <td>4</td>
      <td>1877</td>
      <td>54</td>
      <td>Ohio</td>
      <td>185</td>
      <td>4034311</td>
      <td>8413101</td>
      <td>369</td>
      <td>477.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>Kenyon</td>
      <td>50.135501</td>
      <td>47.9527</td>
    </tr>
    <tr>
      <th>16</th>
      <td>James A. Garfield</td>
      <td>0.5</td>
      <td>1881</td>
      <td>49</td>
      <td>Ohio</td>
      <td>214</td>
      <td>4446158</td>
      <td>9210420</td>
      <td>369</td>
      <td>444.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>Williams</td>
      <td>57.994580</td>
      <td>48.2731</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Grover Cleveland</td>
      <td>4</td>
      <td>1885</td>
      <td>47</td>
      <td>New York</td>
      <td>219</td>
      <td>4874621</td>
      <td>10049754</td>
      <td>401</td>
      <td>576.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>None</td>
      <td>54.613466</td>
      <td>48.5049</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Benjamin Harrison</td>
      <td>4</td>
      <td>1889</td>
      <td>55</td>
      <td>Indiana</td>
      <td>233</td>
      <td>5443892</td>
      <td>11383320</td>
      <td>401</td>
      <td>426.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>Miami</td>
      <td>58.104738</td>
      <td>47.8234</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Grover Cleveland</td>
      <td>4</td>
      <td>1893</td>
      <td>55</td>
      <td>New York</td>
      <td>277</td>
      <td>5551883</td>
      <td>12056097</td>
      <td>444</td>
      <td>576.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>None</td>
      <td>62.387387</td>
      <td>46.0504</td>
    </tr>
    <tr>
      <th>20</th>
      <td>William McKinley</td>
      <td>4</td>
      <td>1897</td>
      <td>54</td>
      <td>Ohio</td>
      <td>271</td>
      <td>7108480</td>
      <td>13935738</td>
      <td>447</td>
      <td>601.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>Allegheny College</td>
      <td>60.626398</td>
      <td>51.009</td>
    </tr>
    <tr>
      <th>21</th>
      <td>William Howard Taft</td>
      <td>4</td>
      <td>1909</td>
      <td>51</td>
      <td>Ohio</td>
      <td>321</td>
      <td>7676258</td>
      <td>14882734</td>
      <td>483</td>
      <td>491.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>Yale</td>
      <td>66.459627</td>
      <td>51.5783</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Woodrow Wilson</td>
      <td>8</td>
      <td>1913</td>
      <td>56</td>
      <td>New Jersey</td>
      <td>435</td>
      <td>6293152</td>
      <td>15040963</td>
      <td>531</td>
      <td>723.0</td>
      <td>Democrat</td>
      <td>Educator</td>
      <td>Princeton</td>
      <td>81.920904</td>
      <td>41.8401</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Warren G. Harding</td>
      <td>2</td>
      <td>1921</td>
      <td>55</td>
      <td>Ohio</td>
      <td>404</td>
      <td>16133314</td>
      <td>26753786</td>
      <td>531</td>
      <td>326.0</td>
      <td>Republican</td>
      <td>Editor</td>
      <td>None</td>
      <td>76.082863</td>
      <td>60.3029</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Herbert Hoover</td>
      <td>4</td>
      <td>1929</td>
      <td>54</td>
      <td>California</td>
      <td>444</td>
      <td>21411991</td>
      <td>36790364</td>
      <td>531</td>
      <td>400.0</td>
      <td>Republican</td>
      <td>Engineer</td>
      <td>Stanford</td>
      <td>83.615819</td>
      <td>58.2</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Franklin Roosevelt</td>
      <td>12</td>
      <td>1933</td>
      <td>51</td>
      <td>New York</td>
      <td>472</td>
      <td>22825016</td>
      <td>39749382</td>
      <td>531</td>
      <td>876.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>Harvard</td>
      <td>88.888889</td>
      <td>57.4223</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Dwight D. Eisenhower</td>
      <td>8</td>
      <td>1953</td>
      <td>62</td>
      <td>New York</td>
      <td>442</td>
      <td>33936137</td>
      <td>61551118</td>
      <td>531</td>
      <td>699.0</td>
      <td>Republican</td>
      <td>Soldier</td>
      <td>US Military Academy</td>
      <td>83.239171</td>
      <td>55.1349</td>
    </tr>
    <tr>
      <th>27</th>
      <td>John F. Kennedy</td>
      <td>3</td>
      <td>1961</td>
      <td>43</td>
      <td>Massachusetts</td>
      <td>303</td>
      <td>34221344</td>
      <td>68828960</td>
      <td>537</td>
      <td>704.0</td>
      <td>Democrat</td>
      <td>Author</td>
      <td>Harvard</td>
      <td>56.424581</td>
      <td>49.7194</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Richard M. Nixon</td>
      <td>5</td>
      <td>1969</td>
      <td>56</td>
      <td>New York</td>
      <td>301</td>
      <td>31785148</td>
      <td>73203370</td>
      <td>538</td>
      <td>477.0</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>Whittier</td>
      <td>55.947955</td>
      <td>43.4203</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Jimmy Carter</td>
      <td>4</td>
      <td>1977</td>
      <td>52</td>
      <td>Georgia</td>
      <td>297</td>
      <td>40830763</td>
      <td>81555889</td>
      <td>538</td>
      <td>518.0</td>
      <td>Democrat</td>
      <td>Businessman</td>
      <td>US Naval Academy</td>
      <td>55.204461</td>
      <td>50.0648</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Ronald Reagan</td>
      <td>8</td>
      <td>1981</td>
      <td>69</td>
      <td>California</td>
      <td>489</td>
      <td>43904153</td>
      <td>86515221</td>
      <td>538</td>
      <td>634.0</td>
      <td>Republican</td>
      <td>Actor</td>
      <td>Eureka College</td>
      <td>90.892193</td>
      <td>50.7473</td>
    </tr>
    <tr>
      <th>31</th>
      <td>George Bush</td>
      <td>4</td>
      <td>1989</td>
      <td>64</td>
      <td>Texas</td>
      <td>426</td>
      <td>48886097</td>
      <td>91584820</td>
      <td>538</td>
      <td>548.0</td>
      <td>Republican</td>
      <td>Businessman</td>
      <td>Yale</td>
      <td>79.182156</td>
      <td>53.3779</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Bill Clinton</td>
      <td>8</td>
      <td>1993</td>
      <td>46</td>
      <td>Arkansas</td>
      <td>370</td>
      <td>44909326</td>
      <td>104425014</td>
      <td>538</td>
      <td>539.0</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>Georgetown</td>
      <td>68.773234</td>
      <td>43.0063</td>
    </tr>
    <tr>
      <th>33</th>
      <td>George W. Bush</td>
      <td>8</td>
      <td>2001</td>
      <td>54</td>
      <td>Texas</td>
      <td>271</td>
      <td>50460110</td>
      <td>105417258</td>
      <td>538</td>
      <td>NaN</td>
      <td>Republican</td>
      <td>Businessman</td>
      <td>Yale</td>
      <td>50.371747</td>
      <td>47.867</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Barack Obama</td>
      <td>n/a</td>
      <td>2009</td>
      <td>47</td>
      <td>Illinois</td>
      <td>365</td>
      <td>69492376</td>
      <td>129438754</td>
      <td>538</td>
      <td>NaN</td>
      <td>Democrat</td>
      <td>Lawyer</td>
      <td>Columbia University</td>
      <td>67.843866</td>
      <td>53.6875</td>
    </tr>
  </tbody>
</table>
</div>



** 웹페이지 표 읽기(행정안전부 지방 물가 정보)**

행정안전부 지방물가 정보 사이트 [http://www.mois.go.kr/frt/sub/a02/mulMain/screen.do](http://www.mois.go.kr/frt/sub/a02/mulMain/screen.do)에서 농축산물 전월 평균 가격정보를 가져오자.


```python
import bs4, requests
import pandas as pd

# 행정 안전부 지방 물가 정보 - 농축산물
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

        구분    쇠고기  돼지고기   닭고기    달걀    배추     무    감자  고추가루      콩      쌀
    0   서울   9864  2619  6353  2281  3330  1769  3622  3329  11153  48638
    1   부산   9382  2370  5962  3497  5046  1961  2929  3329   9601  43952
    2   대구   9428  2455  6423  2447  4345  1691  3428  3071   9373  41517
    3   인천   8691  2438  5652  2179  3797  1622  3761  2783   9317  42322
    4   광주  10433  2447  5219  2346  4240  2061  3730  3382   9868  43047
    5   대전   8013  2599  5091  3169  2905  1589  3876  4131   9329  42780
    6   울산   9160  2196  6485  2587  4608  1935  3581  3957  10523  42880
    7   경기   9627  2389  5917  2542  4263  1860  3713  3304   9848  44653
    8   강원   9251  2410  5627  2208  4249  1871  3186  3445   8535  45319
    9   충북   9851  2643  5550  2908  4044  2019  3132  3279   9297  42971
    10  충남   8258  2146  5626  2400  4161  1914  3008  3784   8169  44093
    11  전북   8892  2196  5353  2954  3270  1857  3557  3509   8580  42440
    12  전남   8716  2098  5391  3072  4268  2188  3217  3210   7373  40663
    13  경북   7675  2136  5941  2953  4611  2032  3006  2588   9240  41505
    14  경남   8795  2165  5327  2960  3904  1917  3189  3027   8056  41187
    15  제주   8425  2528  6298  2860  5522  2070  2910  3087   8152  43017
    

참고: `pd.read_html(웹페이지)`은 웹 페이지에 있는 표(table)를 pandas DataFrame **리스트**로 변환한다.

** 직접하기 **

- 국가 지표 체계 홈페이지 [http://www.index.go.kr/potal/main/EachDtlPageDetail.do?idx_cd=1007](http://www.index.go.kr/potal/main/EachDtlPageDetail.do?idx_cd=1007)에서 지역별 인구 및 인구밀도 표를 출력하시오.
- 가져온 데이터의 열이름을 `df.columns`를 이용하여 변경하시오. 예를 들어 `2012인구`, `2012인구밀도` 등으로 바꾸시오.

**데이터 프레임 내용 출력**


```python
df['구분'] # Series 형으로 출력
```




    0     서울
    1     부산
    2     대구
    3     인천
    4     광주
    5     대전
    6     울산
    7     경기
    8     강원
    9     충북
    10    충남
    11    전북
    12    전남
    13    경북
    14    경남
    15    제주
    Name: 구분, dtype: object




```python
df[['구분', '쇠고기', '감자']] # DataFrame형 출력
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>구분</th>
      <th>쇠고기</th>
      <th>감자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서울</td>
      <td>9864</td>
      <td>3622</td>
    </tr>
    <tr>
      <th>1</th>
      <td>부산</td>
      <td>9382</td>
      <td>2929</td>
    </tr>
    <tr>
      <th>2</th>
      <td>대구</td>
      <td>9428</td>
      <td>3428</td>
    </tr>
    <tr>
      <th>3</th>
      <td>인천</td>
      <td>8691</td>
      <td>3761</td>
    </tr>
    <tr>
      <th>4</th>
      <td>광주</td>
      <td>10433</td>
      <td>3730</td>
    </tr>
    <tr>
      <th>5</th>
      <td>대전</td>
      <td>8013</td>
      <td>3876</td>
    </tr>
    <tr>
      <th>6</th>
      <td>울산</td>
      <td>9160</td>
      <td>3581</td>
    </tr>
    <tr>
      <th>7</th>
      <td>경기</td>
      <td>9627</td>
      <td>3713</td>
    </tr>
    <tr>
      <th>8</th>
      <td>강원</td>
      <td>9251</td>
      <td>3186</td>
    </tr>
    <tr>
      <th>9</th>
      <td>충북</td>
      <td>9851</td>
      <td>3132</td>
    </tr>
    <tr>
      <th>10</th>
      <td>충남</td>
      <td>8258</td>
      <td>3008</td>
    </tr>
    <tr>
      <th>11</th>
      <td>전북</td>
      <td>8892</td>
      <td>3557</td>
    </tr>
    <tr>
      <th>12</th>
      <td>전남</td>
      <td>8716</td>
      <td>3217</td>
    </tr>
    <tr>
      <th>13</th>
      <td>경북</td>
      <td>7675</td>
      <td>3006</td>
    </tr>
    <tr>
      <th>14</th>
      <td>경남</td>
      <td>8795</td>
      <td>3189</td>
    </tr>
    <tr>
      <th>15</th>
      <td>제주</td>
      <td>8425</td>
      <td>2910</td>
    </tr>
  </tbody>
</table>
</div>



** 직접하기 **

- 2012년 인구와 2017년 인구 밀도를 각각 출력하시오.

**값 출력**

정당에 속해 있는 사람들의 수를 센다.


```python
df_excel['Political Party'].value_counts()
```




    Republican               14
    Democrat                 13
    Democratic-Republican     4
    Whig                      2
    None                      1
    Federalist                1
    Name: Political Party, dtype: int64



#### 기본 통계

- 요약

`describe()` 함수를 이용하여 기본적인 통계량을 관찰할 수 있다. `describe(include='all')`을 이용해서 모든 열에 대해서 통계량을 관찰할 수 있다.


```python
df_excel.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year first inaugurated</th>
      <th>Age at inauguration</th>
      <th># of electoral votes</th>
      <th>Total electoral votes</th>
      <th>Rating points</th>
      <th>% electoral</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>35.000000</td>
      <td>35.000000</td>
      <td>35.000000</td>
      <td>35.000000</td>
      <td>33.000000</td>
      <td>35.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1892.542857</td>
      <td>55.085714</td>
      <td>261.114286</td>
      <td>385.085714</td>
      <td>552.606061</td>
      <td>68.048420</td>
    </tr>
    <tr>
      <th>std</th>
      <td>64.758530</td>
      <td>6.381828</td>
      <td>118.620198</td>
      <td>143.817567</td>
      <td>159.117280</td>
      <td>15.092928</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1789.000000</td>
      <td>43.000000</td>
      <td>69.000000</td>
      <td>69.000000</td>
      <td>259.000000</td>
      <td>32.183908</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1843.000000</td>
      <td>51.000000</td>
      <td>176.000000</td>
      <td>292.000000</td>
      <td>444.000000</td>
      <td>57.123855</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1885.000000</td>
      <td>55.000000</td>
      <td>234.000000</td>
      <td>401.000000</td>
      <td>564.000000</td>
      <td>66.459627</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1943.000000</td>
      <td>57.500000</td>
      <td>343.000000</td>
      <td>531.000000</td>
      <td>632.000000</td>
      <td>80.756370</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2009.000000</td>
      <td>69.000000</td>
      <td>489.000000</td>
      <td>538.000000</td>
      <td>900.000000</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_excel.describe(include='all')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>President</th>
      <th>Years in office</th>
      <th>Year first inaugurated</th>
      <th>Age at inauguration</th>
      <th>State elected from</th>
      <th># of electoral votes</th>
      <th># of popular votes</th>
      <th>National total votes</th>
      <th>Total electoral votes</th>
      <th>Rating points</th>
      <th>Political Party</th>
      <th>Occupation</th>
      <th>College</th>
      <th>% electoral</th>
      <th>% popular</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>35</td>
      <td>35.0</td>
      <td>35.000000</td>
      <td>35.000000</td>
      <td>35</td>
      <td>35.000000</td>
      <td>35</td>
      <td>35</td>
      <td>35.000000</td>
      <td>33.000000</td>
      <td>35</td>
      <td>35</td>
      <td>35</td>
      <td>35.000000</td>
      <td>35</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>34</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>15</td>
      <td>NaN</td>
      <td>30</td>
      <td>30</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>10</td>
      <td>20</td>
      <td>NaN</td>
      <td>30</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Grover Cleveland</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Ohio</td>
      <td>NaN</td>
      <td>NA()</td>
      <td>NA()</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Republican</td>
      <td>Lawyer</td>
      <td>None</td>
      <td>NaN</td>
      <td>NA()</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>NaN</td>
      <td>6</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>21</td>
      <td>8</td>
      <td>NaN</td>
      <td>6</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1892.542857</td>
      <td>55.085714</td>
      <td>NaN</td>
      <td>261.114286</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>385.085714</td>
      <td>552.606061</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>68.048420</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>64.758530</td>
      <td>6.381828</td>
      <td>NaN</td>
      <td>118.620198</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>143.817567</td>
      <td>159.117280</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>15.092928</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>min</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1789.000000</td>
      <td>43.000000</td>
      <td>NaN</td>
      <td>69.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>69.000000</td>
      <td>259.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>32.183908</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1843.000000</td>
      <td>51.000000</td>
      <td>NaN</td>
      <td>176.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>292.000000</td>
      <td>444.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>57.123855</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1885.000000</td>
      <td>55.000000</td>
      <td>NaN</td>
      <td>234.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>401.000000</td>
      <td>564.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>66.459627</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1943.000000</td>
      <td>57.500000</td>
      <td>NaN</td>
      <td>343.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>531.000000</td>
      <td>632.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>80.756370</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>max</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2009.000000</td>
      <td>69.000000</td>
      <td>NaN</td>
      <td>489.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>538.000000</td>
      <td>900.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>100.000000</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



- 열별 평균, 합계
`mean()` 함수를 이용하여 평균을 구할 수 있다. 숫자형에 대해서만 계산한다. `sum()`을 이용하여 열별 합계를 구할 수 있다. 문자열일 경우 각 행의 문자열들을 연결한다.


```python
df.mean() # Series형 반환
```




    쇠고기      9028.8125
    돼지고기     2364.6875
    닭고기      5763.4375
    달걀       2710.1875
    배추       4160.1875
    무        1897.2500
    감자       3365.3125
    고추가루     3325.9375
    콩        9150.8750
    쌀       43186.5000
    dtype: float64




```python
df.sum()
```




    구분      서울부산대구인천광주대전울산경기강원충북충남전북전남경북경남제주
    쇠고기                               144461
    돼지고기                               37835
    닭고기                                92215
    달걀                                 43363
    배추                                 66563
    무                                  30356
    감자                                 53845
    고추가루                               53215
    콩                                 146414
    쌀                                 690984
    dtype: object



`df.iloc[행슬라이싱, 열슬라이싱]` 을 이용하여 파이썬 슬라이싱 문법을 사용할 수 있다.


```python
df.iloc[:, 1:].sum()
```




    쇠고기     144461
    돼지고기     37835
    닭고기      92215
    달걀       43363
    배추       66563
    무        30356
    감자       53845
    고추가루     53215
    콩       146414
    쌀       690984
    dtype: int64



또한 이름으로도 가능한다. `df.loc[:, '쇠고기':'닭고기']`를 이용하여 `쇠고기` 열부터 `닭고기` 열까지를 잘라낼 수 있다.


```python
df.loc[:5, '쇠고기':'닭고기']
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>쇠고기</th>
      <th>돼지고기</th>
      <th>닭고기</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9864</td>
      <td>2619</td>
      <td>6353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9382</td>
      <td>2370</td>
      <td>5962</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9428</td>
      <td>2455</td>
      <td>6423</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8691</td>
      <td>2438</td>
      <td>5652</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10433</td>
      <td>2447</td>
      <td>5219</td>
    </tr>
    <tr>
      <th>5</th>
      <td>8013</td>
      <td>2599</td>
      <td>5091</td>
    </tr>
  </tbody>
</table>
</div>



- 정렬
`sort_values(by=['colname'])`을 이용해서 지정된 열로 데이터프레임을 정렬할 수 있다.


```python
df.sort_values(by=['쇠고기'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>구분</th>
      <th>쇠고기</th>
      <th>돼지고기</th>
      <th>닭고기</th>
      <th>달걀</th>
      <th>배추</th>
      <th>무</th>
      <th>감자</th>
      <th>고추가루</th>
      <th>콩</th>
      <th>쌀</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>경북</td>
      <td>7675</td>
      <td>2136</td>
      <td>5941</td>
      <td>2953</td>
      <td>4611</td>
      <td>2032</td>
      <td>3006</td>
      <td>2588</td>
      <td>9240</td>
      <td>41505</td>
    </tr>
    <tr>
      <th>5</th>
      <td>대전</td>
      <td>8013</td>
      <td>2599</td>
      <td>5091</td>
      <td>3169</td>
      <td>2905</td>
      <td>1589</td>
      <td>3876</td>
      <td>4131</td>
      <td>9329</td>
      <td>42780</td>
    </tr>
    <tr>
      <th>10</th>
      <td>충남</td>
      <td>8258</td>
      <td>2146</td>
      <td>5626</td>
      <td>2400</td>
      <td>4161</td>
      <td>1914</td>
      <td>3008</td>
      <td>3784</td>
      <td>8169</td>
      <td>44093</td>
    </tr>
    <tr>
      <th>15</th>
      <td>제주</td>
      <td>8425</td>
      <td>2528</td>
      <td>6298</td>
      <td>2860</td>
      <td>5522</td>
      <td>2070</td>
      <td>2910</td>
      <td>3087</td>
      <td>8152</td>
      <td>43017</td>
    </tr>
    <tr>
      <th>3</th>
      <td>인천</td>
      <td>8691</td>
      <td>2438</td>
      <td>5652</td>
      <td>2179</td>
      <td>3797</td>
      <td>1622</td>
      <td>3761</td>
      <td>2783</td>
      <td>9317</td>
      <td>42322</td>
    </tr>
    <tr>
      <th>12</th>
      <td>전남</td>
      <td>8716</td>
      <td>2098</td>
      <td>5391</td>
      <td>3072</td>
      <td>4268</td>
      <td>2188</td>
      <td>3217</td>
      <td>3210</td>
      <td>7373</td>
      <td>40663</td>
    </tr>
    <tr>
      <th>14</th>
      <td>경남</td>
      <td>8795</td>
      <td>2165</td>
      <td>5327</td>
      <td>2960</td>
      <td>3904</td>
      <td>1917</td>
      <td>3189</td>
      <td>3027</td>
      <td>8056</td>
      <td>41187</td>
    </tr>
    <tr>
      <th>11</th>
      <td>전북</td>
      <td>8892</td>
      <td>2196</td>
      <td>5353</td>
      <td>2954</td>
      <td>3270</td>
      <td>1857</td>
      <td>3557</td>
      <td>3509</td>
      <td>8580</td>
      <td>42440</td>
    </tr>
    <tr>
      <th>6</th>
      <td>울산</td>
      <td>9160</td>
      <td>2196</td>
      <td>6485</td>
      <td>2587</td>
      <td>4608</td>
      <td>1935</td>
      <td>3581</td>
      <td>3957</td>
      <td>10523</td>
      <td>42880</td>
    </tr>
    <tr>
      <th>8</th>
      <td>강원</td>
      <td>9251</td>
      <td>2410</td>
      <td>5627</td>
      <td>2208</td>
      <td>4249</td>
      <td>1871</td>
      <td>3186</td>
      <td>3445</td>
      <td>8535</td>
      <td>45319</td>
    </tr>
    <tr>
      <th>1</th>
      <td>부산</td>
      <td>9382</td>
      <td>2370</td>
      <td>5962</td>
      <td>3497</td>
      <td>5046</td>
      <td>1961</td>
      <td>2929</td>
      <td>3329</td>
      <td>9601</td>
      <td>43952</td>
    </tr>
    <tr>
      <th>2</th>
      <td>대구</td>
      <td>9428</td>
      <td>2455</td>
      <td>6423</td>
      <td>2447</td>
      <td>4345</td>
      <td>1691</td>
      <td>3428</td>
      <td>3071</td>
      <td>9373</td>
      <td>41517</td>
    </tr>
    <tr>
      <th>7</th>
      <td>경기</td>
      <td>9627</td>
      <td>2389</td>
      <td>5917</td>
      <td>2542</td>
      <td>4263</td>
      <td>1860</td>
      <td>3713</td>
      <td>3304</td>
      <td>9848</td>
      <td>44653</td>
    </tr>
    <tr>
      <th>9</th>
      <td>충북</td>
      <td>9851</td>
      <td>2643</td>
      <td>5550</td>
      <td>2908</td>
      <td>4044</td>
      <td>2019</td>
      <td>3132</td>
      <td>3279</td>
      <td>9297</td>
      <td>42971</td>
    </tr>
    <tr>
      <th>0</th>
      <td>서울</td>
      <td>9864</td>
      <td>2619</td>
      <td>6353</td>
      <td>2281</td>
      <td>3330</td>
      <td>1769</td>
      <td>3622</td>
      <td>3329</td>
      <td>11153</td>
      <td>48638</td>
    </tr>
    <tr>
      <th>4</th>
      <td>광주</td>
      <td>10433</td>
      <td>2447</td>
      <td>5219</td>
      <td>2346</td>
      <td>4240</td>
      <td>2061</td>
      <td>3730</td>
      <td>3382</td>
      <td>9868</td>
      <td>43047</td>
    </tr>
  </tbody>
</table>
</div>



## 시각화


```python
get_ipython().magic('matplotlib inline')
import matplotlib.pyplot as plt
import numpy as np
```

- line


```python
import numpy as np
from matplotlib import font_manager, rc

font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
rc('font', family=font_name)

df.set_index('구분').plot(kind='line', xticks=np.arange(len(df['구분'])), rot=90)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x238e2d606d8>




![png](output_89_1.png)


`xticks=np.arange(16)`는 xtick이 보여질 위치를 지정하는 것이다.

- boxplot


```python
get_ipython().magic('matplotlib inline')
from matplotlib import font_manager, rc

font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
rc('font', family=font_name)

df.boxplot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x238e2e63550>




![png](output_91_1.png)


- 파이 그래프


```python
get_ipython().magic('matplotlib inline')
df_excel['Political Party'].value_counts().plot(kind="pie")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x238e30c83c8>




![png](output_93_1.png)


- 바차트


```python
df_excel['Political Party'].value_counts().plot(kind="bar")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x238e319b4e0>




![png](output_95_1.png)


## 참고 사이트

- Beautiful Soup: https://www.crummy.com/software/BeautifulSoup/bs4/doc/#
- requests: http://pythonstudy.xyz/python/article/403-%ED%8C%8C%EC%9D%B4%EC%8D%AC-Web-Scraping
- urllib: https://www.acmicpc.net/blog/view/16
- Python for Data Analysis by Wes McKinney: https://github.com/wesm/pydata-book
- http://www.hanbit.co.kr/channel/category/category_view.html?cms_code=CMS9481416663
- Python for Data Analysis by Wes McKinney: https://github.com/wesm/pydata-book
- Pandas Documentation: http://pandas.pydata.org/pandas-docs/stable/
