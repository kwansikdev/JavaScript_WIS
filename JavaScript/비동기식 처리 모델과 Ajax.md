# 비동기식 처리 모델과 Ajax

<br>

### 1. Ajax(Asynchronous JavaScript and XML)

브라우저에서 웹페이지를 요청하거나 링크를 클릭하면 화면 갱신이 발생한다. 이것은 브라우저와 서버와의 통신에 의한 것이다.

<img src="https://poiemaweb.com/img/req_res.png" style="zoom:48%;" />

서버는 요청받은 페이지(HTML)를 반환하는데 이때 HTML에서 로드하는 CSS나 JavaScript 파일들도 같이 반환된다. 클라이언트 요청에 따라 서버는 정적인 파일을 반환할 수 있고 서버 사이드 프로그램이 만들어낸 파일이나 데이터를 반환할 수 있다. 서버로부터 웹페이지가 반환되면 클라이언트(브라우저)는 이를 렌더링하여 화면에 표시한다.

<br>

Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 이용해서 **비동기적(Asynchronous)**으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미한다.

서버로부터 웹페이지가 반환되면 화면 전체를 갱신해야 하는데 **페이지 일부만을 갱신하고도 동일한 효과를 볼 수 있는 것**이 Ajax이다. 페이지 일부만 로드하여 갱신하면 되므로 빠른 퍼포먼스와 부드러운 화면 표시 효과를 기대할 수 있다.

<img src="https://poiemaweb.com/img/ajax-webpage-lifecycle.png" style="zoom:33%;" />

<br>

### 2. JSON(JavaScript Object Notation)

클라이언트와 서버 간에는 데이터 교환이 필요하다. JSON(JavaScript Object notation)은 클라이언트와 서버 간 데이터 교환을 위한 규칙, 즉 **데이터 포맷**을 말한다.

JSON은 일반 텍스트 포맷보다 효과적인 데이터 구조화가 가능하며 XML 포맷보다 가볍고 사용하기 간편하며 가독성도 좋다.

자바스크립트의 **객체 리터럴**과 매우 흡사하지만 **JSON은 순수한 텍스트로 구성된 규칙이 있는 데이터 구조이다.**

~~~javascript
{
  "name": "Lee",
  "gender": "male",
  "age": 20,
  "alive": true
}
~~~

**키는 반드시 '큰따옴표로만' 둘러싸야한다.**

<br>

#### 2.1. JSON.stringify

JSON.stringify 메소드는 객체를 JSON 형식의 문자열로 변환한다.

~~~javascript
JSON.stringify(value[, replacer[, space]])
~~~

| 매개변수 | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| value    | JSON 문자열로 변환할 값                                      |
| replacer | 문자열화 동작 방식을 변경하는 함수,<br />JSON 문자열에 포함될 값 객체의 속성들을 선택하기 위한 화이트리스트(whitelist)로 쓰이는 [`String`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String) 과 [`Number`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number) 객체들의 배열 |
| space    | 가독성을 목적으로 JSON 문자열 출력에 공백을 삽입하는데 사용되는 [`String`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String) 또는 [`Number`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number) 객체 |

<br>

~~~javascript
const o = { name: 'Lee', gender: 'male', age: 20 };

// 객체 => JSON 형식의 문자열
const strObject = JSON.stringify(o);
console.log(typeof strObject, strObject);
// string {"name":"Lee","gender":"male","age":20}

// 객체 => JSON 형식의 문자열 + prettify
const strPrettyObject = JSON.stringify(o, null, 2);
console.log(typeof strPrettyObject, strPrettyObject);
/*
string {
  "name": "Lee",
  "gender": "male",
  "age": 20
}
*/

// replacer
// 값의 타입이 Number이면 필터링되어 반환되지 않는다.
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === 'number' ? undefined : value;
}

// 객체 => JSON 형식의 문자열 + replacer + prettify
const strFilteredObject = JSON.stringify(o, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "Lee",
  "gender": "male"
}
*/

const arr = [1, 5, 'false'];

// 배열 객체 => 문자열
const strArray = JSON.stringify(arr);
console.log(typeof strArray, strArray); // string [1,5,"false"]

// replacer
// 모든 값을 대문자로 변환된 문자열을 반환한다
function replaceToUpper(key, value) {
  return value.toString().toUpperCase();
}

// 배열 객체 => 문자열 + replacer
const strFilteredArray = JSON.stringify(arr, replaceToUpper);
console.log(typeof strFilteredArray, strFilteredArray); // string "1,5,FALSE"
~~~

<br>

#### 2.2. JSON.parse

JSON.parse 메소드는 JSON 데이터를 가진 문자열을 객체로 반환한다.

> 서버로부터 브라우저로 전송된 JSON 데이터는 문자열이다.
>
> 이 문자열을 객체로서 사용하려면 객체화하여야 하는데 이를 역직렬화(Deserializing)이라 한다.
>
> 역직렬화를 위해서 **내장 객체 JSON의 static 메소드인 JSON.parse**를 사용한다.

~~~javascript
const o = { name: 'Lee', gender: 'male', age: 20 };

// 객체 => JSON 형식의 문자열
const strObject = JSON.stringify(o);
console.log(typeof strObject, strObject);
// string {"name":"Lee","gender":"male","age":20}

const arr = [1, 5, 'false'];

// 배열 객체 => 문자열
const strArray = JSON.stringify(arr);
console.log(typeof strArray, strArray); // string [1,5,"false"]

// JSON 형식의 문자열 => 객체
const obj = JSON.parse(strObject);
console.log(typeof obj, obj); // object { name: 'Lee', gender: 'male' }

// 문자열 => 배열 객체
const objArray = JSON.parse(strArray);
console.log(typeof objArray, objArray); // object [1, 5, "false"]
~~~

배열이 JSON 형식의 문자열로 변환되어 있는 경우 JSON.parse 는 문자열을 배열 객체로 변환한다. 배열 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

~~~javascript
const todos = [
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'JavaScript', completed: false }
];

// 배열 => JSON 형식의 문자열
const str = JSON.stringify(todos);
console.log(typeof str, str);

// JSON 형식의 문자열 => 배열
const parsed = JSON.parse(str);
console.log(typeof parsed, parsed);

~~~



<br>

### 3. XMLHttpRequest

브라우저는 **XMLHttpRequest 객체**를 이용하여 Ajax 요청을 생성하고 전송한다. 서버가 브라우저의 요청에 대해 응답을 반환하면 같은 **XMLHttpRequest 객체가 그 결과를 처리**한다.

<br>

#### 3.1. Ajax request

~~~javascript
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
xhr.open('GET', '/users');
// Request를 전송한다
xhr.send();
~~~

##### 3.1.1 XMLHttpRequest.open

XMLHttpRequest 객체의 **인스턴스를 생성**하고 XMLHttpRequest.open 메소드를 사용하여 서버로의 요청을 준비한다. XMLHttpRequest.open의 사용법은 아래와 같다.

~~~javascript
XMLHttpRequest.open(method, url[, async])
~~~

| 매개변수    | 설명                                                         |
| :---------- | :----------------------------------------------------------- |
| method      | HTTP method (“GET”, “POST”, “PUT”, “DELETE” 등)              |
| url         | 요청을 보낼 URL                                              |
| async[옵션] | 비동기 조작 여부. 옵션으로 **default는 true**이며 비동기 방식으로 동작한다. |

<br>

##### 3.1.2 XMLHttpRequest.send

준비된 요청을 서버에 전달한다.

기본적으로 서버로 전송하는 데이터는 GET, POST 메소드에 따라 그 전송 방식에 차이가 있다.

- GET 메소드의 경우, URL의 일부분인 쿼리문자열(query string)로 데이터를 서버로 전송한다.
- POST 메소드의 경우, 데이터(페이로드)를 Request Body에 담아 전송한다.

XMLHttpRequest.send 메소드에는 **request body**에 담아 전송할 인수를 전달할 수 있다.

~~~javascript
xhr.send(null);
// xhr.send('string');
// xhr.send(new Blob()); // 파일 업로드와 같이 바이너리 컨텐트를 보내는 방법
// xhr.send({ form: 'data' });
// xhr.send(document);
~~~

만약 **요청 메소드가 GET인 경우, send 메소드의 인수는 무시되고 request body은 null로 설정된다.**

<br>

##### 3.1.3 XMLHttpRequest.setRequestHeader

XMLHttpRequest.setRequestHeader 메소드는 HTTP Request Header의 값을 설정한다. setRequestHeader 메소드는 **반드시 XMLHttprequest.open 메소드 호출 이후에 호출한다.**

자주 사용하는 Request Header인 content-type, Accept에 대해 살펴보면

<br>

**Content-type**

- request body 에 담아 전송할 데이터의 MIME-type의 정보를 표현한다.

  ~~~javascript
  MIME-type
  : 클라이언트에게 전송된 문서의 다양성을 알려주기 위한 메커니즘
  : 리소스를 내려받았을 때 해야 할 기본 동작이 무엇인지를 결정하기 위해 대게 MIME 타입을 사용
  ~~~

| 타입                        | 서브타입                                           |
| :-------------------------- | :------------------------------------------------- |
| text 타입                   | text/plain, text/html, text/css, text/javascript   |
| Application 타입            | application/json, application/x-www-form-urlencode |
| File을 업로드하기 위한 타입 | multipart/formed-data                              |

다음은 request body에 담아 서버로 전송할 데이터의 MIME-type을 지정하는 예이다.

~~~javascript
// json으로 전송하는 경우
xhr.open('POST', '/users');

// 클라이언트가 서버로 전송할 데이터의 MIME-type 지정: json
xhr.setRequestHeader('Content-type', 'application/json');

const data = { id: 3, title: 'JavaScript', author: 'Park', price: 5000};

xhr.send(JSON.stringify(data));
~~~

<br>

**Accept**

HTTP 클라이언트가 서버에 요청할 때 서버가 센드백할 데이터의 MIME-type을 Accept로 지정할 수 있다.

~~~javascript
// 서버가 센드백할 데이터의 MIME-type 지정: json
xhr.setRequestHeader('Accept', 'application/json');
~~~

만약 Accept 헤더를 설정하지 않으면, send 메소드가 호출될 때 Accept 헤더가 `*/*` 으로 전송된다.

<br>

#### 3.2. Ajax response

Ajax 응답 처리의 예

~~~javascript
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 onreadystatechange 이벤트 핸들러가 호출된다.
// 이 함수는 Response가 클라이언트에 도달하면 호출된다.
xhr.onreadystatechange = function (e) {
  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
~~~

XMLHttpRequest.send 메소드를 통해 서버에 Request를 전송하면 서버는 Response를 반환한다.

하지만 언제 Response가 클라이언트에 도달하는지는 알수 없다.

XMLHttpRequest.**onreadystatechange**는 Response가 **클라이언트에 도달하여 발생된 이벤트를 감지하고 콜백 함수를 실행**하여 준다. 이때 이벤트는 Request에 어떠한 변화가 발생한 경우, **즉 XMLHttpRequest.readyState 프로퍼티가 변경된 경우 발생**한다

<br>

XMLHttpRequest 객체는 response가 클라이언트에 도달했는지를 추적할 수 있는 프로퍼티를 제공한다.

그 프로퍼티가 바로 **XMLHttpRequest.readyState** 이다.

| Value | State            | Description                                           |
| ----- | ---------------- | ----------------------------------------------------- |
| 0     | UNSENT           | XMLHttpRequest.open() 메소드 호출 이전                |
| 1     | OPENED           | XMLHttpRequest.open() 메소드 호출 완료                |
| 2     | HEADERS_RECEIVED | XMLHttpRequest.send() 메소드 호출 완료                |
| 3     | LOADING          | 서버 응답 중(XMLHttpRequest.responseText 미완성 상태) |
| 4     | DONE             | 서버 응답 완료                                        |

<br>

~~~javascript
// XMLHttpRequest 객체의 생성
var xhr = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
xhr.open('GET', 'data/test.json');
// Request를 전송한다
xhr.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출한다.
xhr.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출된다.

  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
~~~



<br>

### 4. Web Server

웹서버는 브라우저와 같은 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그래밍이다.

Ajax는 웹서버와의 통신이 필요하므로 예지를 ㅅㄹ행하기 위해서는 웹서버가 필요하다

<br>

### 5. Ajax 예제

<br>

#### 5.1. Load HTML

Ajax를 이용하여 웹페이지에 추가하기 가장 손쉬운 데이터 형식은 HTML이다. 별도의 작업없이 전송받은 데이터를 DOM에 추가하면 된다.

<br>

#### 5.2. Load JSON

서버로부터 브라우저로 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체화하여야 하는데 이를 **역직렬화(Deserializing)** 이라 한다. 역직렬화를 위해서 **내장 객체 JSON의 static 메소드인 JOSN.parse()**를 사용한다.

<br>

#### 5.2. Load JSONP

요청에 의해 웹페이지가 전달된 서버와 동일한 도메인의 서버로 부터 전달된 데이터는 문제없이 처리할 수 있다. 

하지만 보안상의 이유로 다른 도메인(http와 https, 포트가 다르면 다른 도메인으로 간주한다)으로의 요청(크로스 도메인 요청)은 제한된다. 이것을 **동일출처원칙(Same-origin policy)** 이라고 한다.

<img src="https://poiemaweb.com/img/cdr.jpg" style="zoom:75%;" />

**1. 웹서저의 프록시 파일**

서버에 원격 서버로부터 데이터를 수집하는 별도의 기능을 추가하는 것이다. 이것을 **프록시(Proxy)** 라 한다.

<br>

**2. JSONP**

script 태그의 원본 주소에 대한 제약은 존재하지 않지만 이것을 이용하여 다른 도메인의 서버에서 데이터를 수집하는 방법이다. 자신의 서버에 함수를 정의하고 다른 도메인의 서버에 얻고자 하는 데이터를 인수로 하는 함수 호출문을 로드하는 것이다.

![](https://poiemaweb.com/img/comparison_between_ajax_and_jsonp.png)

<br>

**3. Cross-Origin Resource Sharing**

HTTP 헤더에 추가적으로 정보를 추가하여 브라우저와 서버가 서로 통신해야 한다는 사실을 알게하는 방법이다.

최신 브라우저에서만 동작하며 서버에 HTTP 헤더를 설정해 주어야 한다.

Node.js로 구현한 서버의 경우, CORS package를 사용하면 간단하게 Cross-Origin Resource Sharing을 구현할 수 있다.

~~~javascript
const express = require('express')
const cors = require('cors')
const app = express()

app.use(cors())

app.get('/products/:id', function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for all origins!'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
~~~

<br>

