---
title: '[node.js] callback function의 개념'
categories:
    - node.js
tag:
    - node.js
    - callback function
---

## Callback Function이 무엇일까?  

자바스크립트에서 함수(function)는 일급 객체이다. 즉, 함수는 `Object` 타입이며 `String, Array, Number` 등 다른 일급 객체와 똑같이 사용될 수 있다. 함수 자체가 객체 타입이므로, 변수 안에 담을 수도 있고, 파라미터로 다른 함수에 전달해 줄 수도 있으며, 함수에서 만들어지고 반환 될 수도 있다.  

**Callback function**은 특정 함수에 매개변수로서 전달된 함수를 말한다. 그리고 이 callback function은 그 함수를 전달 받은 함수 안에서 호출된다. 이해를 돕기 위하여 jQuery에서 사용된 callback function의 예제를 확인해 보자.
```javascript
$("button_1").click(()=>{
    alert("button 1 clicked");
})
```
`click()` 메소드에 callback function이 매개변수로 전달 되었다. 이 함수는 `button_1` 에 마우스 클릭이 발생할 경우 호출된다.

#### blocking code vs non-blocking code  

우선 `input.txt` 를 다음과 같이 생성해 보자.
```text
Hello world!!
My name is Jongwon.
```
<br>
> **blocking code**  

Callback function이 사용되지 않는 blocking code의 예제  
어떤 작업을 실행하고, 끝나기를 기다리면서 아래에 있는 코드들을 실행하지 않음.
```javascript
const fs = require("fs");
const data = fs.readFileSync("input.txt");
console.log(data.toString());
console.log("Program has ended.");
```
위의 코드를 실행했을 때의 결과는 다음과 같다.
```text
Hello world!!
My name is Jongwon.
Program has ended.
```
위의 결과 값에서 알 수 있듯이, `input.txt` 에 적혀있는 문구가 출력되고 난 다음, 프로그램이 종료되었다는 문구가 출력된다.  

<br>
> **Non-Blocking code**

callback function이 사용된 Non-blocking code의 예제  
blocking code 예제와는 반대로 함수가 실행될 때, 프로그램은 함수가 끝나기를 기다리지 않고 아래에 있는 코드들을 실행한다. 이후 작업이 다 끝나면 callback function을 호출한다. 위의 코드를 다음과 같이 수정해 보자.
```javascript
const fs = require("fs");
fs.readFile('input.txt', (err,data) => {
    if (err) return console.error(err);
    console.log(data.toString());
});
console.log("Program has ended");
```
모든 node 어플리케이션의 비동기식 함수에서는 첫 번째 매개변수로 `error` 를 받고, 마지막 매개변수로는 `callback function` 을 받는다. `fs.readFile()` 함수는 비동기식으로 파일을 읽는 함수이고, 도중에 에러가 발생하면 err 객체에 여러 내용을 담고, 그렇지 않을 시에는 파일 내용을 다 읽고 출력한다. 위의 코드를 실행했을 때의 결과는 다음과 같다.
```text
Program has ended.
Hello world!!
My name is Jongwon.
```
`fs.readFile()` 메소드가 실행 된 후, 프로그램이 메소드가 끝날때까지 대기하지 않고 곧바로 다음 명령어를 실행하였기 때문에, 프로그램이 끝났다는 메시지를 출력 한 후에,  `input.txt` 에 적혀있는 문구가 출력되고 프로그램이 종료된다. 만약 `fs.readFile()` 다음에 실행되는 코드가 `console.log()` 가 아니라 `fs.readFile()` 보다 실행 시간이 오래걸리는 코드였다면 `input.txt` 에 적혀있는 문구가 먼저 출력될 것이다.