# BeautifulSoup 4

## 기본

~~~python
import requests
from bs4 import BeautifulSoup

url = "https://comic.naver.com/webtoon"
res = requests.get(url)
res.raise_for_status()

soup = BeautifulSoup(res.text, "lxml")
# print(soup.title)
# print(soup.title.get_text())
# print(soup.a) # soup 객체에서 처음 발견되는 a element 출력
# print(soup.a.attrs) # a element 의 속성 정보를 출력
# print(soup.a["href"]) # a element 의 href 속성 '값' 정보를 출력

# print(soup.find("a"), attrs={"class":"Nbtn_upload"}) # class="Nbtn_upload" 인 a element 를 찾아줘
# print(soup.find(attrs={"class":"Nbtn_upload"})) # class="Nbtn_upload" 인 어떤 element 를 찾아줘

# print(soup.find("li", attrs={"class":"rank01"})
~~~

## 활용편 (네이버 웹툰)

~~~python
import requests
from bs4 import BeautifulSoup

url = "https://comic.naver.com/webtoon"
res = requests.get(url)
res.raise_for_status()

soup = BeautifulSoup(res.text, "lxml")

# 네이버 웹툰 전체 목록 가져오기 .find_all("태그", "속성")
cartoons = soup.find_all("a", attrs={"class":"title"})
for cartoon in cartoons:
# 텍스트를 가져오는 방법 .get_text()
	print(cartoon.get_text())
~~~

~~~python
import requests
from bs4 import BeautifulSoup

url = "https://comic.naver.com/webtoon/list?titleId=799793"
res = requests.get(url)
res.raise_for_status()

soup = BeautifulSoup(res.text, "lxml")

cartoons = soup.find_all("td", attrs={"class":"title"})
title = cartoons.a.get_text()
# a태그의 href 속성을 가져오고 싶을때 .a["href"]
link = cartoons.a["href"]
print(title)
print("https://comic.naver.com" + link)

# for 를 이용해서 모든 제목과 링크를 가져옴

for cartoon in cartoons :
	title = cartoon.a.get_text()
	link = "https://comic.naver.com" + cartoon.a["href"]
	print(title, link)
~~~

## 활용편 (쿠팡)

- HTTP METHOD 

서버에 요청을 보낼때, HTTP 메서드가 존재.

- GET 방식과 POST 방식

GET 방식은 누구나 볼 수 있게 **URL에 보내는** 방식

큰 데이터는 못 보내..

POST 방식은 HTTP 메시지 **BODY** 에 보내는 방식

아이디나 패스워드 전송 시 POST 방식이 좋다.

큰 데이터는 post 방식이 적합

~~~ python
import requests
import re
from bs4 import BeautifulSoup


headers = {"User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3 Safari/605.1.15"}

# 1~5 page를 본다.
for i in range(1, 6):
	print("페이지 :", i)
	url = f"https://www.coupang.com/np/search?q=맥북&channel=user&component=&eventCategory=SRP&trcid=&traid=&sorter=scoreDesc&minPrice=&maxPrice=&priceRange=&filterType=&listSize=36&filter=&isPriceRange=false&brand=&offerCondition=&rating=0&page={i}&rocketAll=false&searchIndexingToken=1=9&backgroundColor="
	
	# User Agent String 검색 후 집어넣기
	res = requests.get(url, headers=headers)
	res.raise_for_status()
	soup = BeautifulSoup(res.text, "lxml")
	
	# 정규식으로 class가 search-product 로 시작하는 class 를 모두 찾아봄
	
	items = soup.find_all("li", attrs={"class":re.compile("^search-product")})
	
	# 제품명을 하나 가져온다.
	
	print(items[0].find("div", attrs={"class":"name"}).get_text())
	
	# 원하는 정보를 전부 가져와본다
	
	for item in items:
		
		#광고 제품 제외
		ad_badge = item.find("span", attrs={"class:ad-badge-text"})
		if ad_badge:
			print("광고 상품 제외합니다.")
			continue
		
		
		name = item.find("div", attrs={"class":"name"}).get_text() # 제품명
		
		# 애플 제품 제외
	
		if "Apple" in name:
			print("애플 상품 제외합니다.")
		
		price = item.find("strong", attrs={"class":"rice-value"}).get_text() # 가격
		rate = item.find("em", attrs={"class":rating}) # 평점
		# 평점이 없는 경우를 거름
		if rate:
			rate = rate.get_text() 
			
		else:
			print("평점 없는 상품은 제외합니다.")
			continue
			
		rate_cnt = item.find("span", attrs={"class":"rating-total-count"}) #평점 수
		if rate_cnt:
			rate_cnt = rate_cnt.get_text() # "(26)" 이런식으로 나옴
			rate_cnt = rate_cnt[1:-1] # 슬라이스 해줌
			print("리뷰 수", rate_cnt)
		else:
			print("평점 없는 상품은 제외합니다.")
			continue
		
		if float(rate) >= 4.5 and int(rate_cnt) >= 50:
			print(name, price, rate, rate_cnt)

~~~

파이썬, [정규식 기본 참조](https://github.com/stu442/today-i-learned/blob/main/나도코딩-웹스크래핑/정규식%20기본.md)


## 활용편 (다음 이미지)

~~~ python
import requests
from bs4 import BeautifulSoup

for year in range(2015, 2020):
	# 인코딩된 String
	url = "https://search.daum.net/search?w=tot&nil_mtopsearch=btn&DA=YZR&q={url}%EB%85%84+%EC%98%81%ED%99%94+%EC%88%9C%EC%9C%84".format(year)
	res = requests.get(url)
	res.raise_for_status()
	soup = BeautifulSoup(res.text, "lxml")
images = soup.find_all(“img”, attrs={“class”:”thumb_img”})

	for idx, image in enumerate(images):
		image_url = image[“src”]
		if image_url.startswith(“//“):
			image_url = “https:” + image_url
		
		print(image_url)
		# 전달받아 가공한 image_url을 다시 요청함
		image_res = requests.get(image_url)
		image_res.raise_for_status()
		
		# 파일 다운로드
		with open(movie_{}__{}.jpg”.format(yaer, idx+1), “wb”) as f:
			f.write(image_res.content)
			
		if idx >= 4: # 상위 5개 이미지 까지만 다운로드
			break
		
~~~

[Request 기본 with문 참조](https://github.com/stu442/today-i-learned/blob/main/나도코딩-웹스크래핑/Request%20기본.md)
