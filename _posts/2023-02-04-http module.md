---
title: '[node.js] http module 알아보기'
categories:
    - node.js
tag:
    - node.js
    - http module
    - server
    - client
---

## HTTP 모듈에 대하여 알아보자

#### index.html 작성

```html
<html>
    <head>
        <title>Sample server</title>
    </head>
    <body>
        Hello World!
    </body>
</html>
```

#### server.js 작성
```javascript
const http = require("http");   // http module
const fs = require("fs");       // file system module
const url = require("url");     // url module

const hostname = "127.0.0.1";
const port = 8081;

// 서버 생성
const server = http.createServer((request, response) => {
    // url.parse - url 문자열을 객체로 변환하여 return
    var pathname = url.parse(request.url).pathname;
    console.log("Request for " + pathname + " received.");

    if(pathname == "/"){
        pathname = "/index.html";
    }

    fs.readFile(pathname.substr(1), (err, data) => {
        if(err){ 
            // 페이지를 찾을 수 없는 경우
            console.log(err);
            response.statusCode = 404;
            response.setHeader('Content-Type', 'text/html');
        }else{
            // 페이지를 찾은 경우
            response.statusCode = 200;
            response.setHeader('Content-Type', 'text/html');
            response.write(data.toString());
        }
        response.end();
    })
})

server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
})
```
클라이언트에서 서버에 접속을 하면 URL에서 열고자 하는 파일을 파싱하여 열어준다. 그러나 파일이 존재하지 않는다면 콘솔에 에러 메시지를 출력한다. 서버를 실행하고 다음 링크들을 접속해보자.  
- [http://127.0.0.1:8081/](http://127.0.0.1:8081/)
- [http://127.0.0.1:8081/index.html](http://127.0.0.1:8081/index.html)
- [http://127.0.0.1:8081/error](http://127.0.0.1:8081/error)  

```text
$ node server.js
Server running at http://127.0.0.1:8081/
Request for / received.
Request for /index.html received.
Request for /error received.
[Error: ENOENT: no such file or directory, open 'error'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: 'error'
}
```
링크들을 클릭하였을 때 출력되는 내용은 위와 같다.

#### client.js 작성  
```javascript
const http = require("http");

const options = {
    host: "localhost",
    port: "8081",
    path: "/index.html"
};

const req = http.request(options, (response) => {
    var body = "";
    response.on('data', (data) => {
        body += data;
    })
    response.on('end', () => {
        console.log(body);
    })
});

req.end();
```
위의 클라이언트 코드는 서버에 `index.html` 페이지를 요청하는 코드이다. 서버를 켜둔 상태에서 요청을 해야 response로 받은 페이지가 정상적으로 출력된다.  

```text
$ node server.js    (terminal 1 - server)
$ node client.js    (terminal 2 - client)
<html>
    <head>
        <title>Sample server</title>
    </head>
    <body>
        Hello World!
    </body>
</html>
```