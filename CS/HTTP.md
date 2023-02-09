# HTTP 란?

## HTTP

HTTP 는 Hyper Text Transfer Protocol 의 준말입니다. 브라우저와 서버 간 데이터를 주고받기 위한 방식으로 HTTP 프로토콜을 사용하고 있습니다.

## 요청과 응답 (Request & Response)

HTTP 프로토콜로 데이터를 주고받기 위해서는 브라우저가 서버에게 요청을 보내고, 서버는 브라우저에게 응답을 보내야합니다.

요청을 할때는 요청에 대한 정보를 담아 서버로 보냅니다.

또, 서버가 응답할 때도 응답에 대한 정보를 담아 클라이언트에게 보냅니다.

이런 정보가 담긴 메시지를 HTTP 메시지라고 합니다.

HTTP 메시지는 시작줄, 헤더, 본문으로 구성됩니다.

## 실제 요청 HTTP 메시지

> GET https://www.zerocho.com HTTP/1.1<br />
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...<br />
Upgrade-Insecure-Requests: 1<br /><br />
(본문 없음)

첫 줄이 시작줄입니다. 

두 번째 줄은 헤더입니다. 요청에 대한 정보를 담고 있습니다.

헤더에서 한줄을 띄고 본문이 시작됩니다. 본문은 요청을 할 때 보낼 데이터를 담는 부분입니다.

지금은 단순히 주소로만 요청을 보내기에 데이터는 담아 보내지 않았습니다.

## 실제 응답 메시지

> HTTP/1.1 200 OK<br />
Connection: keep-alive<br />
Content-Encoding: gzip<br />
Content-Length: 35653<br />
Content-Type: text/html;<br /><br />
\<!DOCTYPE html>\<html lang="ko" data-reactroot="">\<head><title...

요청과 마찬가지, 시작줄, 헤더, 본문으로 구성되어있습니다.

첫 줄은 버전 상태코드 상태메시지로 구성되었습니다. 200은 성공적인 요청이었다는 의미입니다.

두 번째 줄부터는 헤더입니다. 응답에 대한 정보를 담고있습니다.

본문이 있네요. 응답에는 보통 본문이 있습니다.

## HTTP 상태코드

### 2xx - 성공

- 200 : GET 요청에 대한 성공
- 204 : No Content. 성공했으나 응답 본문에 데이터가 없음
- 205 : Reset Content. 성공했으나 클라이언트 화면을 새로고침하도록 권고
- 206 : Partial Conent. 성공했으나 일부 범위의 데이터만 반환

### 3xx - 리다이렉션

300번대의 상태코드는 대부분 클라이언트가 이전 주소로 데이터를 요청하여 서버에서 새 URL 로 리다이렉트를 유도하는 경우입니다.

- 301 : Moved Permanently, 요청한 자원이 새 URL에 존재
- 303 : See other, 요청한 자원이 임시 주소에 존재
- 304 : Not Modified, 요청한 자원이 변경되지 않았으므로 클라이언트에서 캐싱된 자원을 사용하도록 권고.

### 4xx- 클라이언트 에러

400번대 상태 코드는 대부분 클라이언트의 코드가 잘못된 경우입니다. 유효하지 않은 자원을 요청했거나 요청이나 권한이 잘못된 경우 발생합니다. 

- 400 : Bad Request, 잘못된 요청
- 401 : Unauthorized, 권한 없이 요청. Authorization 헤더가 잘못된 경우
- 403 : Forbidden, 서버에서 해당 자원에 대해 접근 금지
- 404 : 요청한 자원이 서버에 없음
- 405 : Method Not Allowed, 허용되지 않은 요청 메서드
- 409 : Conflict, 최신 자원이 아닌데 업데이트하는 경우.

### 5xx- 서버 에러
500번대 상태 코드는 서버에서 오류가 난 경우입니다.

- 501 : Not Implemented, 요청한 동작에 대해 서버가 수행할 수 없는 경우
- 503 : Service Unavailable, 서버가 과부하 또는 유지 보수로 내려간 경우
