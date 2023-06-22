# Request 기본

~~~python
import requests

  

res = requests.get("http://google.com")

res.raise_for_status()

  

print(len(res.text))

  

with open("mygoogle.html", "w", encoding="utf8") as f:

f.write(res.text)
~~~

## with 문

자원을 획득하고, 사용 한 후, 반납해야 하는 경우 이용

1. 자원을 획득
2. 자원을 사용
3. 자원을 반납

### with 문 문법

~~~python
with EXPRESSION [as VARIABLE]:
	BLOCK
~~~

클래스의 객체를 바로 만들고,
만든 객체를 이용하여 인사를 하고
만든 객체를 소멸시킴

객체의 라이프사이클(생성 >> 사용 >> 소멸)을 설계 가능