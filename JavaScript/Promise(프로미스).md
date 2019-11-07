# Promise(프로미스)

<br>

### 1. 프로미스란?

비동기 처리를 위한 하나틔 패턴으로 콜백 함수를 사용한다. 

하지만 전통적인 콜백 패턴은 가독성이 나쁘고 비동기 처리 중 발생한 **에러의 예외 처리가 곤란**하며 여러 개의 비동기 처리 로직을 한꺼번에 처리하는 것도 한계가 있다.

Promise는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현한다.



<br>

### 2. 콜백 패턴의 단점

<br>

#### 2.1 콜백 헬

비동기식 처리 모델은 병렬적으로 태스크를 수행한다. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 즉시 다음 태스크를 실행한다.

자바스크립트에서 빈번하게 사용되는 비동기식 처리 모델은 요청을 병렬로 처리하여 다른 요청이 블로킹(blocking, 작업 중단)되지 않는 장점이 있다.

하지만 비동기 처리를 위해 콜백 패턴을 사용하면 처리 순서를 보장하기 위해 여러 개의 콜백 함수가 네스팅(nesting, 중첩)되어 복잡도가 높아지는 **콜백 헬(Callback Hell)**이 발생하는 단점이 있다.

<br>

콜백 헬이 발생하는 이유는 실행 완료를 기다리지 않고 즉시 다음 태스크를 실행한다. 따라서 비동기 함수 내에서 처리 결과를 반환하면 기대한 대로 동작하지 않는다.

비동기 함수의 처리 결과를 반환하는 경우, 순서가 보장되지 않기 때문에 그 반환 결과를 가지고 후속처리를 할 수 없다. 즉, **비동기 함수의 처리 결과에 대한 처리는 비동기 함수의 콜백 함수 내에서 처리해야 한다.** 이로 인해 콜백 헬이 발생한다.

<br>

비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우, 함수의 호출이 중첩이 되어 복잡도가 높아지는 현상이 발생하는데 이를  **콜백 헬(Callback Hell)**이라 한다. 이로 인해 **에러처리가 곤란하다.**



<br>

#### 2.2 에러 처리의 한계

콜백 방식의 비동기 처리가 갖는 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 것이다.

비동기 처리 함수의 콜백 함수는 해당 이벤트가 발생하면 태스크 큐로 이동한 후 호출 스택이 비어졌을 때, 호출 스택으로 이동되어 실행된다. 비동기 함수는 콜백 함수가 실행될 때까지 기디리지 않고 즉시 종료되어 호출 스택이 비어졌을 때, 호출 스택으로 이동되어 실행된다.

이후 이벤트가 발생했을 때, 비동기 처리 함수의 콜백 함수는 태스크 큐로 이동한 후 호출 스택이 비어졌을 때 호출 스택으로 이동되어 실행된다. 이때 비동기 처리 함수는 이미 호출 스택에서 제거된 상태이다. 

따라서 비동기 처리 함수의 콜백 함수를 호출 한 것은 그 비동기 처리 함수가 아니라는 것을 의미한다.



<br>

### 3. 프로미스의 생성

프로미스는 Promise 생성자 함수를 통해 인스턴스화한다. 

Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자로 전달받는데 이 콜백 함수는 **resole와 reject 함수**를인자로 전달받는다.

~~~javascript
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
});
~~~

<br>

Promise는 비동기 처리가 성공(fulfilled)하였는지 또는 실패(rejected)하였는지 등의 상태(state) 정보를 갖는다.

| 상태          | 의미                                      | 구현                                              |
| ------------- | ----------------------------------------- | ------------------------------------------------- |
| pending       | 비동기 처리가 아직 수행되지 않은 상태     | resole 또는 reject 함수가 아직 호출되지 않은 상태 |
| **fulfilled** | 비동기 처리가 수행된 상태(성공)           | resolve 함수가 호출된 상태                        |
| **rejected**  | 비동기 처리가 수행된 상태(실패)           | rejecet 함수가 호출된 상태                        |
| settled       | 비동기 처리가 수행된 상태(성공 또는 실패) | resovle 또는 rejecet 함수가 호출된 상태           |

<br>

Promise 생성자 함수가 인자로 전달받은 콜백 함수는 내부에서 비동기 처리 작업을 수행한다. 

이때 비동기 처리가 성공하면 콜백 함수의 인자로 전달받은 resolve 함수를 호출한다. 이때 프로미스는 ‘fulfilled’ 상태가 된다.

비동기 처리가 실패하면 reject 함수를 호출한다. 이때 프로미스는 ‘rejected’ 상태가 된다.

~~~javascript
const promiseAjax = (method, url, payload) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open(method, url);
    xhr.setRequestHeader('Content-type', 'application/json');
    xhr.send(JSON.stringify(payload));

    xhr.onreadystatechange = function () {
      // 서버 응답 완료가 아니면 무시
      if (xhr.readyState !== XMLHttpRequest.DONE) return;

      if (xhr.status >= 200 && xhr.status < 400) {
        // resolve 메소드를 호출하면서 처리 결과를 전달
        resolve(xhr.response); // Success!
      } else {
        // reject 메소드를 호출하면서 에러 메시지를 전달
        reject(new Error(xhr.status)); // Failed...
      }
    };
  });
};
~~~

비동기 함수 내에서 Promise 객체를 생성하고 그 내부에 비동기 처리를 구현한다. 

이때 비동기 처리에 성공하면 resolve 메소드를 호출하고 resolve 메소드의 인자로 비동기 처리 결과를 전달한다. 이 처리 결과는 Proimse 객체의 후속 처리 메소드로 전달된다.

만약 비동기 처리에 실패하면 reject 메소드를 호출하고 reject 메소드의 인자로 에러 메시지를 전달한다.



<br>

### 4. 프로미스의 후속 처리 메소드

Promise로 구현된 비동기 함수는 Promise 객체를 반환하여야 한다. 

Promise로 구현된 비동기 함수를 호출하는 측(promise consumer)에서는 Promise 객체의 후속 처리 메소드(then, catch)를 통해 비동기 처리 결과 또는 에러 메시지를 전달받아 처리한다. 

Promise 객체는 상태를 갖는다고 하였다. 이 상태에 따라 후속 처리 메소드를 체이닝 방식으로 호출한다



> **> then**
>
> 두 개의 콜백 함수를 인자로 전달 받는다. 
>
> 첫번째 콜백 함수는 성공 시 호출되고 두번째 함수는 실패시 호출된다.
>
> **then 메소드는 Promise를 반환한다.**

> **> catch**
>
> 예외(비동기 처리에서 발생한 에러와 then 메소드에서 발생한 에러가 발생하면 호출한다. 
>
> catch 메소드는 Promise를 반환한다.

~~~html
<!DOCTYPE html>
<html>
<body>
<!DOCTYPE html>
<html>
<body>
  <pre class="result"></pre>
  <script>
    const $result = document.querySelector('.result');
    const render = content => { $result.textContent = JSON.stringify(content, null, 2); };

    const promiseAjax = (method, url, payload) => {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.setRequestHeader('Content-type', 'application/json');
        xhr.send(JSON.stringify(payload));

        xhr.onreadystatechange = function () {
          if (xhr.readyState !== XMLHttpRequest.DONE) return;

          if (xhr.status >= 200 && xhr.status < 400) {
            resolve(xhr.response); // Success!
          } else {
            reject(new Error(xhr.status)); // Failed...
          }
        };
      });
    };

    /*
      비동기 함수 promiseAjax은 Promise 객체를 반환한다.
      Promise 객체의 후속 메소드를 사용하여 비동기 처리 결과에 대한 후속 처리를 수행한다.
    */
    promiseAjax('GET', 'http://jsonplaceholder.typicode.com/posts/1')
      .then(JSON.parse)
      .then(
        // 첫 번째 콜백 함수는 성공(fulfilled, resolve 함수가 호출된 상태) 시 호출된다.
        render,
        // 두 번째 함수는 실패(rejected, reject 함수가 호출된 상태) 시 호출된다.
        console.error
      );
  </script>
</body>
</html>
~~~



<br>

### 5. 프로미스의 에러 처리

Promise 객체의 후속 처리 메소드를 사용하여 비동기 처리 결과에 대한 후속 처리를 수행한다.

비동기 처리시 발생한 에러 메시지는 then 메소드의 두 번째 콜백 함수로 전달된다.

Promise 객체의 후속 처리 메소드 catch을 사용하여도 에러를 처리할 수 있다.

~~~javascript
promiseAjax('GET', 'http://jsonplaceholder.typicode.com/posts/1')
  .then(JSON.parse)
  .then(render)
  .catch(console.error);
~~~

then 메소드의 두 번째 콜백 함수는 비동기 처리에서 발생한 에러(reject 함수가 호출된 상태)만을 캐치한다.

하지만 catch 메소드는 비동기 처리에서 발생한 에러(reject 함수가 호출된 상태)뿐만 아니라 then 메소드 내부에서 발생한 에러도 캐치한다.



<br>

### 6. 프로미스 체이닝

비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우. 함수의 호출이 중첩되어 복잡도가 높아지는 콜백 헬이 발생한다.

프로미스는 후속 처리 메소드를 체이닝하여 여러 개의 프로미스를 연결하여 사용할 수 있다.

Promise 객체를 반환한 비동기 함수는 프로미스 후속 처리 메소드인 then이나 catch 메소드를 사용할 수 있다. 

따라서 then 메소드가 Promise 객체를 반환하도록 하면(then 메소드는 기본적으로 Promise를 반환한다.) 여러 개의 프로미스를 연결하여 사용할 수 있다.

~~~javascript
<!DOCTYPE html>
<html>
<body>
  <pre class="result"></pre>
  <script>
    const $result = document.querySelector('.result');
    const render = content => { $result.textContent = JSON.stringify(content, null, 2); };

    const promiseAjax = (method, url, payload) => {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.setRequestHeader('Content-type', 'application/json');
        xhr.send(JSON.stringify(payload));

        xhr.onreadystatechange = function () {
          if (xhr.readyState !== XMLHttpRequest.DONE) return;

          if (xhr.status >= 200 && xhr.status < 400) {
            resolve(xhr.response); // Success!
          } else {
            reject(new Error(xhr.status)); // Failed...
          }
        };
      });
    };

    const url = 'http://jsonplaceholder.typicode.com/posts';

    // 포스트 id가 1인 포스트를 검색하고 프로미스를 반환한다.
    promiseAjax('GET', `${url}/1`)
      // 포스트 id가 1인 포스트를 작성한 사용자의 아이디로 작성된 모든 포스트를 검색하고 프로미스를 반환한다.
      .then(res => promiseAjax('GET', `${url}?userId=${JSON.parse(res).userId}`))
      .then(JSON.parse)
      .then(render)
      .catch(console.error);
  </script>
</body>
</html>
~~~



<br>

### 7. 프로미스의 정적 메소드

Promise는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메소드를 갖을 수 있다. 4가지 정적 메소드를 제공한다.

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |

<br>

#### 7.1 Promise.resolve/Promise.reject

Promise.resolve와 Promise.reject 메소드는 존재하는 값을 **Promise로 래핑**하기 위해 사용한다.

정적 메소드 Promise.resolve 메소드는 인자로 전달된 값을 resolve하는 Promise를 생성한다.



<br>

#### 7.2 Promise.all

Promise.all 메소드는 프로미스가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다. 그리고 전달받은 모든 프로미스를 병렬로 처리하고 그 처리 결과를 resolve하는 새로운 프로미스를 반환한다. 

Promise.all 메소드는 전달받은 모든 프로미스를 병렬로 처리한다. 이때 모든 프로미스의 처리가 종료될 때까지 기다린 후 아래와 모든 처리 결과를 resolve 또는 reject한다.

- 모든 프로미스의 처리가 성공하면 **각각의 프로미스가 resolve한 처리 결과를 resolve하는 새로운 프로미스를 반환**한다. 이때 첫번째 프로미스가 가장 나중에 처리되어도 Promise.all 메소드가 반환하는 프로미스는 첫번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 담아 그 배열을 resolve하는 새로운 프로미스를 반환한다. 즉, **처리 순서가 보장된다.**

- 프로미스의 처리가 하나라도 실패하면 가장 먼저 실패한 프로미스가 reject한 에러를 reject하는 새로운 프로미스를 즉시 반환한다.

Promise.all 메소드는 전달 받은 이터러블의 요소가 프로미스가 아닌 경우, Promise.resolve 메소드를 통해 프로미스로 래핑된다.

