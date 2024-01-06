# JavaScript Study

## 전역 변수의 문제점

전역 변수의 무분별한 사용은 위험 => 전역변수를 반드시 사용해야 이유가 없다면 지역 변수로 사용<br/>
_모든 코드가 전역 변수를 참조하고 변경할 수 있는 암묵적 결합을 허용한다._<br/>
_전역 변수의 생명 주기가 길다. => 따라서 메모리 리소스도 오랜 기간 소비하고 전역 변수의 상태를 변경할 수 있는 시간도 길고 기회도 많다._<br/>
_스코프 체인 상에서 종점에 존재 => 전역 변수는 스코프 체인 상에서 종점에 존재하기 때문에 검색 속도가 가장 느리다._<br/>
_네임스페이스 오염 => 자바스크립트의 가장 큰 문제점 중 하나는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다._

```javascript
var x = 1;
// ...
// 변수의 중복 선언, 기존 변수에 값을 재할당 한다.
var x = 100;
console.log(x); //100
```

### 지역 변수의 생명주기

```javascript
function foo() {
  var x = "local";
  console.log(x); // local
  return x;
}
foo();
console.log(x); // ReferenceError: x is not defined
```

함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸된다.<br/>
_지역 변수의 생명주기는 함수의 생명 주기와 일치한다._

```javascript
var x = "global";
function foo() {
  console.log(x); // undefined
  var x = "local";
}
foo();
console.log(x); // global
```

_호이스팅은 스코프를 단위로 동작한다._<br/>
_호이스팅은 변수 선언이 스코프의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징이다._

### 전역 변수의 생명 주기

함수는 함수 몸체의 마지막 문 또는 반환문이 실행되면 종료하지만 전역 코드에는 반환문을 사용할 수 없으므로 마지막 문이 실행되어 더이상 실행할 문이 없을때 종료한다.<br/>
브라우저 환경에서 전역 객체는 window이므로 브라우저 환경에서 var키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티이다.<br/>
_var 키워드로 선언한 전역 변수의 생명주기는 전역 객체의 생명 주기와 일치한다._

### 전역 변수의 사용을 억제하는 방법

_전역 변수를 반드시 사용해야 할 이유를 찾지 못한다면 지역 변수를 사용하는 것이 좋다 => 변수의 스코프는 좁을 수록 좋다._<br/>

즉시 실행 함수 => 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

```javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
})();

console.log(foo); // ReferenceError: foo is not defined
```

네임스페이스 객체 => 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가

```javascript
var MYAPP = {};

MYAPP.name; = 'Lee'

console.log(MYAPP.name) // Lee

// -----------------------

var MYAPP = {}

MYAPP.person = {
    name:'Lee'
    address:'Seoul'
}

console.log(MYAPP.person.name) // Lee

```

모듈 패턴 => 클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다.<br/>
모듈 패턴의 특징은 전역 변수의 억제와 캡슐화까지 구현 할 수 있다.<br/>

캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것<br/>
객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라고 한다.

```javascript
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
  };
})();

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

ES6 모듈<br/>
ES6 모듈은 사용하면 더는 전역변수를 사용할 수 없다. => ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.

## let, const 키워드와 블록 레벨 스코프

### var 키워드로 선언한 변수의 문제점

var 키워드로 선언한 변수는 중복 선언이 가능하다.

```javascript
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수는 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

### 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. => 함수 외부에 변수는 전역 변수가 된다.

```javascript
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x는 중복 선언 된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

```javascript
var i = 10;

// for문에서 선언한 i는 전역 변수다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(x); // 5
```

### 변수 호이스팅

var 키워드로 변수를 선언하면 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진것처럼 동작한다.<br/>
즉 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

```javascript
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다.(1. 선언단계)
// 변수 foo는 undefined로 초기회된다.(2. 초기화 단계)
console.log(foo);

// 변수의 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

## let 키워드

### 변수 중복 선언 금지

let 키워드는 이름이 같은 변수를 중복 선언하면 문법 에러를 발생

```javascript
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.

var foo = 456;
let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 블록 레벨 스코프

let 키워드로 선언한 변수는 모든 코드 블록((함수, if문, for문, while문, try/catch 문 등))을 지역스코프로 인정하는 블록레벨스코프를 따른다.

```javascript
let foo = 1; // 전역 변수
{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}
console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

### 변수 호이스팅

let 키워드로 선언한 변수는 var와 달리 호이스팅이 발생하지 않는것처럼 동작한다. => let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생

```javascript
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언단계와 초기화 단계가 한번에 진행된다.

```javascript
// var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조 할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

_let 키워드로 선언한 변수는 '선언단계' 와 '초기화 단계'가 분리되어 진행된다._<br/>
런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.<br/>
let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점까지 변수를 참조할 수 없다. => 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대라고 부른다.

```javascript
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보이지만 그렇지 않다.

```javascript
let foo = 1; // 전역 변수
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2;
}
```

### 전역 객체와 let

```javascript
// 이 예제는 브라우저 환경에서 실행해야 한다.
// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}
// var 키워드로 선언한 전역 변수는 전역 객체의 window의 프로퍼티이다.
console.log(window.x); // 1
// 전역 객체를 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티이다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티이다.
console.log(window.foo); // f foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // f foo() {}
```

```javascript
// 이 예제는 브라우저 환경에서 실행해야 한다.
let x = 1;

// let,const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); // undefined
console.log(x); // 1
```
