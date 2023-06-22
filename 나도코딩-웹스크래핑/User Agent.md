# User Agent

웹사이트에 접속할때, 웹사이트는 접속하는 자의 헤더의 정보를 볼 수 있음.
그 중 [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) 란에는 사용자 소프트웨어의 식별 정보를 담고 있는데,
크롤링 할때 봇인지, 진짜 사람인지 판별하는 요소 중 하나임

## 에러코드

~~~python
import requests
URL = 'http://www.tistory.com'
response = requests.get(URL)
response.status_code
~~~

이때 status_code 가 [200번](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/200)이 나온다면, 정상적으로 접속을 한 것이다.

하지만, 403 에러가 뜬다면, [서버에 요청이 전달 되었지만, 권한 때문에 거절되었다는 것을 의미함](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/403)


## User Agent 바꾸기

[https://www.whatismybrowser.com/detect/what-is-my-user-agent](https://www.whatismybrowser.com/detect/what-is-my-user-agent) 에서 User-Agent 를 확인

~~~python
import requests
url = "http://nadocoding.tistory.com"
headers = {"User-Agent" : "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3"}
res = requests.get(url, headers=headers)
res.raise_for_status()
with open("nadocoding.html", "w", encoding="utf8") as f:
	f.write(res.text)
~~~