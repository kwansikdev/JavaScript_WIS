# Module (모듈)

![](https://poiemaweb.com/img/es6.png)

모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다.

모듈은 세부 사항을 캡슐화하고 공개가 필요한 API만을 외부에 노출한다.

 <br>

> **모듈은 파일 단위로 분리**
>
> 애플리케이션은 필요에 따라 **명시적으로 모듈을 로드하여 재사용**

즉, 모듈은 애플리케이션에 분리되어 개별적으로 존재하다가 애플리케이션의 로드에 의해 비로소 애플리케이션의 일원이 된다.

기능별로 분리되어 작성되므로 코드의 단위를 명확히 분리하여 애플리케이션을 구성할 수 있으며 재상용성이 좋아서 개발 효율성과 유지보수성을 높일 수 있다.

<br>

클라이언트 사이드 자바스크립트는 파일마다 독립적인 파일 스코프를 갖지 않고 하나의 전역 객체에 바인딩되기 때문에 전역 변수가 중복되는 등의 문제가 발생할 수 있다.

자바스크립트를 클라이언트 사이드에 국한하지 않고 범용적으로 사용하고자 하는 움직임이 생기면서 모듈 기능은 반드시 해결해야 하는 핵심 과제가 되었고 이런 상황에서 제안된 것이 **CommonJS**와 **AMD**(Asynchronous Module Definition)이다.

script 태그에 `type="module"` 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다. 모듈의 파일 확장자는 mjs를 권장한다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```

단, 아래와 같은 이유로 아직까지는 브라우저가 지원하는 ES6 모듈 기능보다는 **Webpack** 등의 모듈 번들러를 사용하는 것이 일반적이다.

- IE를 포함한 구형 브라우저는 ES6 모듈을 지원하지 않는다.
- 브라우저의 ES6 모듈 기능을 사용하더라도 트랜스파일링이나 번들링이 필요하다.
- 아직 지원하지 않는 기능이 있다.
- 점차 해결되고는 있지만 아직 몇가지 이슈가 있다.



<br>

### 1. 모듈 스코프

모듈은 **파일 스코프**를 갖는다. 즉, 모듈 내에서 var 키워드가 선언한 변수는 더 이상 전역 변수가 아니며 wundow 객체의 프로퍼티가 아니다.

모듈 내에서 선언한 변수는 모듈 외부에서 참조할 수 없다. 스코프가 다르기 때문이다.

```javascript
// foo.mjs
var x = 'foo';
var y = 'boo';
console.log(x); // 'foo'

// bar.mjs
// 중복 선언이 아니다. 스코프가 다른 변수이다.
var x = 'bar';
// 다른 모듈에서 선언한 변수는 모듈 외부에서 참조할 수 없다. 스코프가 다르기 때문이다.
console.log(y); // ReferenceError: y is not defined
```

```html
<!DOCTYPE html>
<html>
<body>
  <script type="module" src="foo.mjs"></script>
  <script type="module" src="bar.mjs"></script>
</body>
</html>
```



<br>

### 2. export 키워드

모듈은 독립적인 파일 스코프를 갖기 때문에 모듈 안에 선언한 모든 것들은 기본적으로 해당 모듈 내부에서만 참조할 수 있다.

만약 모듈 안에 선언한 항목을 외부에 공개하여 다른 모듈들이 사용할 수 있게 하고 싶다면 export해야 한다.

선언된 변수, 함수, 클래스 모두 export할 수 있다.

선언문 앞에 매번 export 키워드를 붙이는 것이 싫다면 export 대상을 모아 하나의 객체로 구성하여 한번에 export할 수도 있다.

```javascript
// lib.js
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}

// 클래스의 공개
export class Person {
  constructor(name) {
    this.name = name;
  }
}

//
// lib.js
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```



<br>

### 3, import 키워드

export한 모듈을 로드하려면 export한 이름으로 import한다.

```javascript
// app.js
// 같은 폴더 내의 lib.js 모듈을 로드. 확장자 js는 생략 가능.
// 단, 브라우저 환경에서는 모듈의 파일 확장자를 생략할 수 없다.
import { pi, square, Person } from './lib';

console.log(pi);         // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
```

각각의 이름을 지정하지 않고 하나의 이름으로 한꺼번에 import할 수도 있다. 이때 import되는 항목은 as 뒤에 지정한 이름의 변수에 할당된다.

```javascript
// app.js
import * as lib from './lib';

console.log(lib.pi);         // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person('Lee')); // Person { name: 'Lee' }
```

이름을 변경하여 import할 수도 있다.

```javascript
// app.js
import { pi as PI, square as sq, Person as P } from './lib';

console.log(PI);    // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P('Kim')); // Person { name: 'Kim' }
```

