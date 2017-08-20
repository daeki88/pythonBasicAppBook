#  문서 데이터
## 참고 사이트
- Beautiful Soup: https://www.crummy.com/software/BeautifulSoup/bs4/doc/#
- requests: http://pythonstudy.xyz/python/article/403-%ED%8C%8C%EC%9D%B4%EC%8D%AC-Web-Scraping
- urllib: https://www.acmicpc.net/blog/view/16
- Python for Data Analysis by Wes McKinney: https://github.com/wesm/pydata-book

## 데이터 수집 및 파싱(Parsing)
### 웹 페이지 읽기

<details>
<summary>`requests`는 HTTP GET, POST, PUT, DELETE 등을 사용할 수 있으며...</summary>
`requests`는 HTTP GET, POST, PUT, DELETE 등을 사용할 수 있으며, 편리한 데이타 인코딩 기능을 제공하고 있다.
```python
import requests
 
# GET
resp = requests.get('http://httpbin.org/get')
print(resp.text)
 
# POST
dic = {"id": 1, "name": "Kim", "age": 10}
resp = requests.post('http://httpbin.org/post', data=dic)
print(resp.text)
 
resp = requests.put('http://httpbin.org/put')
resp = requests.delete('http://httpbin.org/delete')
```
</details>

- 다음(daum) 홈페이지에 접속해서 HTML 문서를 가져와 화면에 출력하는 예이다.

```python
import requests
 
resp = requests.get( 'http://daum.net' )
# resp.raise_for_status()
 
if (resp.status_code == requests.codes.ok):
    html = resp.text
    print(html)
```

- 지방 물가 정보(행정안전부)

```python
import bs4, requests

site = 'http://www.moi.go.kr/frt/sub/a02/farmProductPriceList_3/screen.do'
resp = requests.get(site)

bs = bs4.BeautifulSoup(html, 'html.parser')
tags = bs.select('div.news_area h2 a') # Top 뉴스
title = tags[0].getText()
```