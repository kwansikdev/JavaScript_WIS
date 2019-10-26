

# Spread 문법

Spread 문법(전개 문법)  `…` 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서(전개, 분산하여, spread) 개별적인 값들의 **목록**으로 만든다. 

Spread 문법의 대상은 Array, String, Map, Set, DOM data structure(NodeList, HTMLCollection), Arguments와 같이 for…of 문으로 순회할 수 있는 이터러블에 한정된다. 

----

 Spread 문법의 결과는 값이 아니다. 이는 Spread 문법  `…` 이 피연산자를 연산하여 값을 생성하는 연산자가 아님을 의미한다. **따라서 Spread 문법의 결과는 변수에 할당할 수 없다.** 

~~~javascript
const list = ...arr; // SyntaxError: Unexpected token ... 
~~~

이처럼 Spread 문법의 결과물은 단독으로 사용할 수 없고, 아래와 같이 쉼표로 구분한 값의 목록을 사용하는 문에서 사용한다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록(2019년 9월 현재 Stage 4 제안)



<br>

### 1. 함수 호출문의 인수 목록에서 사용하는 경우

요소들이 하나의 값으로 뭉쳐있는 배열을 펼쳐서 요소값들을 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야하는 경우가 있다.

~~~javascript
const arr = [1, 2, 3];

// 배열 arr의 요소 중에서 최대값을 구하기 위해 Math.max를 사용한다.
const maxValue = Math.max(arr);

console.log(maxValue); // NaN
~~~

Math.max 메소드는 매개변수 개수를 확정할 수 없는 가변 인자 함수이다. 

여러 개의 숫자를 인수로 전달받아 인수 중에서 최대값을 반환한다.

위 예제의 경우, 숫자가 아닌 배열을 인수로 전달하였기 때문에 최대값을 구할 수 없어서 NaN을 반환한다.

이와 같은 문제를 해결하기 위해 배열을 펼쳐서 요소값들을 개별적인 값들의 목록으로 만든 후 인수로 전달해야한다.

~~~javascript
var arr = [1, 2, 3];

// apply 함수의 2번째 인수(배열)는 apply 함수가 호출하는 함수의 인수 목록이다.
// 따라서 배열이 펼쳐져서 인수로 전달되는 효과가 있다.
var maxValue = Math.max.apply(null, arr);

console.log(maxValue); // 3
~~~

Spread 문법

~~~javascript
const arr = [1, 2, 3];

// Spread 문법을 사용하여 배열 arr을 1, 2, 3으로 펼쳐서  Math.max에 전달한다.
// Math.max(...[1, 2, 3])는 Math.max(1, 2, 3)과 같다.
const maxValue = Math.max(...arr);

console.log(maxValue); // 3
~~~



<br>

### 2. 배열 리터럴 내부에서 사용하는 경우

Spread 문법을 배열 리터럴에서 사용하면 보다 간결하고 가독성이 좋게 표현할 수 있다. 

<br>

#### 2.1. concat

~~~javascript
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

// ES6
const arr = [...[1, 2], 3, 4];
console.log(arr); // [1, 2, 3, 4]
~~~

<br>

#### 2.2. push

~~~javascript
// ES5
var arr1 = [1, 2];
var arr2 = [3, 4];

Array.prototype.push.apply(arr1, arr2);

console.log(arr1); // [1, 2, 3, 4]

// ES6
const arr1 = [1, 2];
const arr2 = [3, 4];

// arr1.push(3, 4)와 같다.
arr1.push(...arr2);

console.log(arr1); // [1, 2, 3, 4]
~~~

<br>

#### 2.3. splice

~~~javascript
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

// apply 메소드의 2번째 인수는 배열이다. 이것은 인수 목록으로 splice 메소드에 전달된다.
// [1, 0].concat(arr2) → [1, 0, 2, 3]
// arr1.splice(1, 0, 2, 3) → arr1[1]부터 0개의 요소를 제거하고
// 그자리(arr1[1])에 새로운 요소(2, 3)를 삽입한다.
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));

console.log(arr1); // [1, 2, 3, 4]

// ES6
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);

console.log(arr1); // [1, 2, 3, 4]
~~~

<br>

#### 2.4. 배열 복사

~~~javascript
// ES5
var origin  = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false

// ES6
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
~~~

얕은 복사하여 복사본을 생성한다.

<br>

#### 2.5. 유사 배열 객체를 배열로 변환

유사 배열 객체(Array-like object)를 배열로 변환하려면 slice 메소드를 apply 함수로 호출하는 것이 일반적이다. 

~~~javascript
// ES5
function sum() {
  // 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.apply(arguments);

  return args.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6
~~~

~~~javascript
// ES6
function sum() {
  // 유사 배열 객체인 arguments를 배열로 변환
  const args = [...arguments];

  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
~~~



<br>

### 3. 객체 리터럴 내부에서 사용하는 경우

Spread 문법의 대상은 이터러블이어야 하지만 Spread 프로퍼티는 객체 리터럴 내부에서 Spread 문법의 사용을 허용한다.

~~~javascript
// Spread 프로퍼티
const n = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }
~~~

Spread 프로퍼티가 도입되기 이전에는 ES6에서 도입된 Object.assign 메소드를 사용하여 여러개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가하였다. 

Spread 프로퍼티는 Object.assign 메소드를 대체할 수 있는 간편한 문법이다. 

