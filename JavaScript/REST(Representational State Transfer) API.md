# REST(Representational State Transfer) API

'REST하게 클라이언트랑 서버간에 데이터를 주고 받는 방식’

**"웹에 존재하는 모든 자원(이미지, 동영상, DB 자원)에 고유한 URI를 부여해 활용"**하는 것

자원을 정의하고 자원에 대한 주소를 지정하는 방법론



<br>

### 1. REST API 중심 규칙

가장 중요한 기본적인 규칙은 두 가지이다. 

**URI는 자원을 표현하는 데에 집중**하고 **행위에 대한 정의는 HTTP Method를 통해 하는 것**이 REST한 API를 설계하는 중심 규칙이다.

**1. URI는 정보의 자원을 표현해야 한다.**

리소스명은 동사보다는 명사를 사용한다. URI는 자원을 표현하는데 중점을 두어야 한다. get 과 같은 행위에 대한 표현은 안된다.

~~~
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
~~~

**2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)으로 표현한다.**

~~~
# bad
GET /todos/delete/1

# good
DELETE /todos/1
~~~



<br>

### 2. HTTP Method

주로 5가지의 Method(GET, POST, PUT, PATCH, DELETE)를 사용하여 CURD를 구현한다.

| Method | Action         | 역활                     |
| ------ | -------------- | ------------------------ |
| GET    | index/retrieve | 모든/특정 리소스를 조회  |
| POST   | create         | 리소스를 생성            |
| PUT    | update all     | **리소스의 전체를 갱신** |
| PATCH  | update         | **리소스의 일부를 갱신** |
| DELETE | delete         | 리소스를 삭제            |



<br>

### 3. REST API의 구성

REST API는 자원(Resource), 행위(Verb), 표현(Representations)의 3가지 요소로 구성된다. 

REST는 자체 표현 구조(Self-descriptiveness)로 구성되어 REST API만으로 요청을 이해할 수 있다.

| 구성 요소       | 내용                    | 표현 방법             |
| --------------- | ----------------------- | --------------------- |
| Resource        | 자원                    | HTTP URI              |
| Verb            | 자원에 대한 행위        | HTTP Method           |
| Representations | 자원에 대한 행위의 내용 | HTTP Message Pay Load |



<br>

### 4. REST API의 Example

<br>

#### 4.1. json-server

Json-server를 사용하여 REST API를 사용하여 보자.

<br>

#### 4.2. GET

리소스에 있는 모든 요소를 조회(index)한다.

<br>

#### 4.3. POST

리소스에 새로운 요소를 생성한다

<br>

#### 4.4. PUT

PUT은 특정 리소스의 전체를 갱신할 때 사용한다.

<br>

#### 4.5. PATCH

특정 리소스의 일부를 갱신할 때 사용한다.

<br>

#### 4.6. DELETE

리소스에서 'key'를 사용하여 요소를 특정하고 삭제한다.