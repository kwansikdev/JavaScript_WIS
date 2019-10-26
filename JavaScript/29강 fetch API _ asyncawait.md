# fetch API & async/await

<br>

### 1. Mock Server

~~~javascript
$ cd ~/Desktop
$ git clone https://github.com/ungmo2/simple-rest-api-server.git
$ cd simple-rest-api-server
# install nodemon
$ npm install -g nodemon
# install dependencies
$ npm install
# start server
$ npm start
~~~



<br>

### 2. Fetch

~~~javascript
<!DOCTYPE html>
<html>
<head>
  <title>Fetch example</title>
</head>
<body>
  <pre class="result"></pre>
  <script>
    const $result = document.querySelector('.result');
    const render = todos => { $result.innerHTML = JSON.stringify(todos, null, 2); };

    // GET all
    fetch('http://localhost:9000/todos')
      .then(res => res.json())
      .then(render)
      .catch(console.log);

    // GET by id
    fetch('http://localhost:9000/todos/1')
      .then(res => res.json())
      .then(render)
      .catch(console.log);

    // POST
    fetch('http://localhost:9000/todos', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ id: 4, content: 'Angular', completed: false })
    }).then(res => res.json())
      .then(render)
      .catch(console.log);

    // DELETE by id
    fetch('http://localhost:9000/todos/4', {
      method: 'DELETE'
    }).then(res => res.json())
      .then(render)
      .catch(console.log);

    // DELETE by completed
    fetch('http://localhost:9000/todos/completed', {
      method: 'DELETE'
    }).then(res => res.json())
      .then(render)
      .catch(console.log);

    // PATCH : 리소스의 일부를 UPDATE
    fetch('http://localhost:9000/todos/1', {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ completed: true })
    }).then(res => res.json())
      .then(render)
      .catch(console.log);

  </script>
</body>
</html>
~~~



<br>

### 3. async/await

~~~javascript
<!DOCTYPE html>
<html>
<head>
  <title>async/await example</title>
</head>
<body>
  <pre class="result"></pre>
  <script>
    const $result = document.querySelector('.result');
    const render = todos => { $result.innerHTML = JSON.stringify(todos, null, 2); };

    // GET all
    (async function () {
      const res = await fetch('http://localhost:9000/todos');
      const todos = await res.json();
      render(todos);
    }());

    // GET by id
    (async function () {
      const res = await fetch('http://localhost:9000/todos/1');
      const todos = await res.json();
      render(todos);
    }());

    // POST
    (async function () {
      const res = await fetch('http://localhost:9000/todos', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ id: 4, content: 'Angular', completed: false })
      });
      const todos = await res.json();
      render(todos);
    }());

    // DELETE by id
    (async function () {
      const res = await fetch('http://localhost:9000/todos/4', {
        method: 'DELETE'
      });
      const todos = await res.json();
      render(todos);
    }());

    // DELETE by completed
    (async function () {
      const res = await fetch('http://localhost:9000/todos/completed', {
        method: 'DELETE'
      });
      const todos = await res.json();
      render(todos);
    }());

    // PATCH : 리소스의 일부를 UPDATE
    (async function () {
      const res = await fetch('http://localhost:9000/todos/1', {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ completed: true })
      });
      const todos = await res.json();
      render(todos);
    }());

  </script>
</body>
</html>
~~~

