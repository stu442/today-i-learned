## CSV 기본 (네이버 금융)


~~~ python
import csv
import requests
from bs4 import BeautifulSoup

url = "https://finance.naver.com/sise/sise_market_sum.naver?&page="

filename = "시가총액 1-200.csv"
# 만약 엑셀 열었을 때, 한글이 이상하다면 encoding 을 "utf8" 에서 "utf-8-sig" 로 변경
f = open(filename, "w", encoding="utf-8-sig", newline="")
writer = csv.writer(f)

# writerow 를 할땐, list(리스트) 형식으로 넣어야함

title = "|N|종목명|현재가|전일비|등락률|액면가|시가총액|상장주식수|외국인비율|거래량|PER|ROE|토론실|".split("|")
writer.writerow(title)

for page in range(1, 5):
	res = requests.get(url + str(page))
	res.raise_for_status()
	soup = BeautifulSoup(res.text, "lxml")
	
	data_rows = soup.find("table", attrs={"class":"type_2"}).find("tbody").find_all("tr")
	for row in data_rows:
		columns = row.find_all("td")
		if len(columns) <= 1: # 의미 없는 데이터는 skip
			continue
		# 한줄 for 문
		data = [column.get_text().strip() for column in columns]
		#print(data)
		writer.writerow(data)
~~~

CSV 형식의 파일을 쓰고 저장 가능