## SOP (Same-Origin Policy)
SOP, 동일 출처 정책이란 어떠한 문서나 스크립트가 다른 **프로토콜 / 포트 / 호스트** 에 있는 리소스를 사용하는 것을 제한하는 정책을 의미한다.

```
http://website.com/ex/ex.html
```

|리소스 요청|허용여부|
|:-:|:-:|
|[http://website.com/ex/](http://website.com/ex/)|성공|
|[http://website.com/ex1/](http://website.com/ex1/)|성공|
|[http://website.com:81/ex/ex.html](http://website.com:81/ex/ex.html)|실패, 포트가 다름|
|[http://wwebsite.com/ex/](http://wwebsite.com/ex/)|실패, 호스트가 다름|
|[https://website.com/ex/ex.html](https://website.com/ex/ex.html)|실패, 프로토콜 다름|

위와 같이 호스트, 포트, 프로토콜 중 하나라도 다르면 동일 출처 정책이 적용되서 요청에 실패한다.

## 해결 방법

### `document.domain`
단편적인 방법으로, 동일한 도메인으로 설정함을 통해 SOP 정책을 피할 수 있다. 만약, 현재 도메인이`http://store.company.com/index.html` 이라면,

```js
document.domain = "company.com";
```

과 같이 설정해서 `http://company.com/index.html` 에 요청을 보낼 수 있다. 단, 2개의 html 파일에 관련된 스크립트 파일에 모두 위와 같이 설정해줘야 하며 파이어폭스에서는 안된다는 제약사항이 있다. 또한 서버-클라이언트 통신에 쓰이는 방법이 아니라 **클라이언트 상에서 출처가 다른 프레임(frame)들에 대해 쓰인다.** 

따라서, 이 방법으로 서버와 통신할 수는 없다.

### CORS (Cross-Origin Resource Sharing)