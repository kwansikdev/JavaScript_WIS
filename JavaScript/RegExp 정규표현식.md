ㅓㅁ

# 정규표현식

<br>

### 1. 정규표현식(regular Expression)

문자열에서 특정 내용을 찾거나 대체 또는 발췌하는데 사용한다.

반복문과 조건문을 사용한 복잡한 코드도 정규표현식을 이용하면 매우 간단하게 표현할 수 있다. 하지만 주석이나 공백을 허용하지 않고 여러가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다는 문제가 있다.

리터럴 표기법으로 생성할 수 있다.

<img src="https://poiemaweb.com/img/regular_expression.png" style="zoom:50%;" />

정규표현식을 사용하는 자바스크립트 메소드

> RegExp.prototyp.exec , RegExp.prototype.test
>
> String.prototype.match , String.prototype.replace, String.prototype.search, String.prototype.split 등

<br>

#### 1.1. 플래그

| Flag | Meaning     | Description                               |
| :--: | :---------- | ----------------------------------------- |
|  i   | Ignore Case | 대소문자를 구별하지 않고 검색한다.        |
|  g   | Global      | 문자열 내의 모든 패턴을 검색한다.         |
|  m   | Multi Line  | 문자열의 행이 바뀌더라도 검색을 계속한다. |

플래그는 옵션이므로 선택적으로 사용한다.

플래그를 사용하지 않은 경우 문자열 내 검색 매칭 대상이 1개 이상이더라도 첫번재 매칭한 대상만을 검색하고 종료한다.

~~~javascript
const targetStr = 'Is this all there is?';

// 문자열 is를 대소문자를 구별하여 한번만 검색한다.
let regexr = /is/;

console.log(targetStr.match(regexr)); // [ 'is', index: 5, input: 'Is this all there is?' ]

// 문자열 is를 대소문자를 구별하지 않고 대상 문자열 끝까지 검색한다.
regexr = /is/ig;

console.log(targetStr.match(regexr)); // [ 'Is', 'is', 'is' ]
console.log(targetStr.match(regexr).length); // 3
~~~

<br>

#### 1.2. 패턴

패턴에는 검색하고 싶은 문자열을 지정한다. 이때 문자열의 따옴표는 생략한다. 따옴표르 포함하면 따옴표까지 검색한다. 또한 패턴은 특별한 의미를 가지는 매타문자 또는 기호로 표현할 수 있다.

`.` 은 임의의 문자 한 개를 의미한다.

패턴에 문자 또는 문자열을 지정하면 일치하는 문자 또는 문자열을 추출한다.

앞선 패턴을 최소 한번 반복하려면 앞선 패턴 뒤에 `+` 를 붙인다.

`|` 를 사용하면 or의 의미를 가지게 된다.

범위를 지정하려면 `[]` 내에 `-` 를 사용한다.

숫자를 추출하는 방법은 예시로  `[0-9]` 나타낸다. 숫자에 있는 콤마 때문에 결과가 분리될 수 있으므로 패턴에 포함시킨다

~~~javascript
const targetStr = 'AA BB Aa Bb 24,000';

// '0' ~ '9' 또는 ','가 한번 이상 반복되는 문자열을 반복 검색
const regexr = /[0-9,]+/g;

console.log(targetStr.match(regexr)); // [ '24,000' ]
~~~

간단히 표현하는 법은 `\d` 는 숫자를 의미한다. `\D` 는 `\d`오ㅏ 반대로 동작한다.

~~~javascript
const targetStr = 'AA BB Aa Bb 24,000';

// '0' ~ '9' 또는 ','가 한번 이상 반복되는 문자열을 반복 검색
let regexr = /[\d,]+/g;

console.log(targetStr.match(regexr)); // [ '24,000' ]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한번 이상 반복되는 문자열을 반복 검색
regexr = /[\D,]+/g;

console.log(targetStr.match(regexr)); // [ 'AA BB Aa Bb ', ',' ]
~~~

`\w` 는 알파벳과 숫자를 의미한다. `\W` 는 `\w` 와 반대로 동작한다.

<br>

#### 1.3. 자주 사용하는 정규표현식

특정 단어로 시작하는지 검사한다.

~~~javascript
const url = 'http://example.com';

// 'http'로 시작하는지 검사
// ^ : 문자열의 처음을 의미한다.
const regexr = /^http/;

console.log(regexr.test(url)); // true
~~~

특정단어로 끝나는지 검사한다.

~~~javascript
const fileName = 'index.html';

// 'html'로 끝나는지 검사
// $ : 문자열의 끝을 의미한다.
const regexr = /html$/;

console.log(regexr.test(fileName)); // true
~~~

숫자인지 검사한다.

~~~javascript
const targetStr = '12345';

// 모두 숫자인지 검사
// [^]: 부정(not)을 의미한다. 얘를 들어 [^a-z]는 알파벳 소문자로 시작하지 않는 모든 문자를 의미한다.
// [] 바깥의 ^는 문자열의 처음을 의미한다.
const regexr = /^\d+$/;

console.log(regexr.test(targetStr)); // true
~~~

하나 이상의 공백으로 시작하는지 검사한다.

~~~javascript
const targetStr = ' Hi!';

// 1개 이상의 공백으로 시작하는지 검사
// \s : 여러 가지 공백 문자 (스페이스, 탭 등) => [\t\r\n\v\f]
const regexr = /^[\s]+/;

console.log(regexr.test(targetStr)); // true
~~~

아이디로 사용 가능한지 검사한다.(영문자, 숫자만 허용, 4-10자리)

~~~javascript
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~10자리인지 검사
// {4,10}: 4 ~ 10자리
const regexr = /^[A-Za-z0-9]{4,10}$/;

console.log(regexr.test(id)); // true
~~~

메일 주소 형식에 맞는지 검사한다.

~~~javascript
const email = 'ungmo2@gmail.com';

const regexr = /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/;

console.log(regexr.test(email)); // true
~~~

핸드폰 번호 형식에 맞는지 검사한다.

~~~javascript
const cellphone = '010-1234-5678';

const regexr = /^\d{3}-\d{3,4}-\d{4}$/;

console.log(regexr.test(cellphone)); // true
~~~

특수 문자 포함 여부를 검사한다.

~~~javascript
const targetStr = 'abc#123';

// A-Za-z0-9 이외의 문자가 있는지 검사
let regexr = /[^A-Za-z0-9]/gi;

console.log(regexr.test(targetStr)); // true

// 아래 방식도 동작한다. 이 방식의 장점은 특수 문자를 선택적으로 검사할 수 있다.
regexr = /[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi;

console.log(regexr.test(targetStr)); // true

// 특수 문자 제거
console.log(targetStr.replace(regexr, '')); // abc123
~~~



<br>

### 2. Javascript Regular Expression

<br>

#### 2.1. RegExp Constructor

자바스크립으는 정규표현식을 위해 RegExp 객체를 지원한다.

RegExp 객체를 생성하기 위해서는 리터럴 방식과 RegExp 객체 생성자 함수를 사용할 수 있다.

~~~javascript
new RegExp(pattern[, flags])
~~~

- pattern 정규표현식의 텍스트
- flags 정규표현식의 플래그 (g, i, m, u, y)

<br>

#### 2.2. RegExp method

<br>

##### 2.2.1. RegExp.prototype.exec(target: string): RegExpExecArray | null

문자열을 검색하여 매칭 결과를 반환한다. 반환값은 배열 또는 null 이다.

~~~javascript
const target = 'Is this all there is?';
const regExp = /is/;

const res = regExp.exec(target);
console.log(res); // [ 'is', index: 5, input: 'Is this all there is?' ]
~~~

exec 메소드는 g 플래그를 지정하여도 첫번재 메칭 결과만을 반환한다.

<br>

##### 2.2.2. RegExp.prototype.test(target: string): boolean

문자열을 검색하여 매칭 결과를 반환한다. 반환값은 true 또는 false 이다.

~~~javascript
const target = 'Is this all there is?';
const regExp = /is/;

const res = regExp.test(target);
console.log(res); // true
~~~

