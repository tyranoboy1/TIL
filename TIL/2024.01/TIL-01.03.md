# JavaScript Study

## 함수

일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것<br/>

### 함수의 사용이유

함수는 필요할때 여러번 호출할 수 있다. => 중복되는 코드 함수를 통해 재사용성을 높일 수 있다.<br/>
재사용성을 높이는 함수는 편의성을 높이고 실수를 줄여 코드의 신뢰성을 높인다.

### 함수 리터럴

함수는 객체지만 일반 객체와는 다르다 _일반 객체는 호출할 수 없지만 함수는 호출 할 수 있다._ <br/>
일반 객체에는 없는 함수 객체만의 고유한 프로퍼티를 갖는다.

### 함수 정의

1. 함수 선언문

```javascript
function add(x, y) {
  return x + y;
}
```

_함수 선언문은 함수 이름을 생략할 수 없다._<br/>
_자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 거기에 함수객체를 할당한다._<br/>
_함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다._

2. 함수 표현식

```javascript
var add = function (x, y) {
  return x + y;
};
```

_자바스크립트의 함수는 일급 객체이다._<br/>
_일급 객체이므로 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다. => 함수표현식_

```javascript
// 함수 참조
console.dir(add);
console.dir(sub);

// 함수 호출
console.log(add(2, 5));
console.log(sub(2, 5));

// 함수 선언문
function add(x, y) {
  return x + y;
}
// 함수 표현식
var sub = function add(x, y) {
  return x + y;
};
```

_함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출 할 수 있지만, 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출 할 수없다._<br/>
_함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르기 때문_<br/>
_함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트의 고유의 특징 => 호이스팅!!_<br/>
_변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 되기때문에<br/>함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생._

3. Function 생성자 함수

```javascript
var add = new Function("x", "y", "return x+y");
```

_생성자 함수 => 객체를 생성하는 함수_<br/>
_함수 선언문과 함수 표현식으로 생성한 함수와 Function 생성자 함수로 생성한 함수는 동일하게 동작하지 않음 => 클로저_

4. 화살표 함수

```javascript
var add = (x, y) => x + y;
```

_화살표 함수는 항상 익명으로 정의한다._ -화살표 함수는 생성자 함수로 사용할 수 없고 this 바인딩 방식이 다르다.\_<br/>
_prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않는다._

## 함수 호출

매개변수를 통해 인수를 전달하는데 인수는 값으로 평가 될수 있는 표현식이어야한다.<br/>
매개변수는 함수 내부에서만 참조할 수 있고 함수 몸체 외부에서는 참조할 수 없다. => 매개변수의 스코프는 함수 내부.

_자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다. => undefined로 할당_<br/>
_자바스크립트는 동적 타입 언어이다. 따라서 자바스크립트 함수는 매개변수의 타입을 사전에 정의할 수 없다._<br/>
_함수는 한가지 일만 해야하며, 가급적 작게 만들어야한다._<br/>
_함수의 호출은 표현식이다_

## 참조에 의한 전달과 외부 상태의 변경

```javascript
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = "kim";
}

var num = 100;
var person = { name: "Lee" };

console.log(num); //100
console.log(person); // {name: 'Lee'}

// 원시값은 값 자체가 복사되어 전달되고 객체는 참조 값이 복사되어 전달
changeVal(num, person);
// 원시 값은 원본이 훼손되지 않는다.
console.log(num); // 100
// 객체는 원본이 훼손된다.
console.log(person); // {name: 'kim'}
```

_참조에 의한 전달을 통해 원본이 변경되는 문제를 해결하기 위해서는 객체를 불변 객체로 만들어 사용한다. => 부수효과 최소화_

## 즉시 실행 함수

```javascript
(function () {
  var a = 3;
  var b = 5;
  return a * b;
})();
```

함수 정의와 동시에 즉시 호출되는 함수 => 단 한번만 호출되며 다시 호출 할 수 없다.

## 재귀 함수

```javascript
function countdown(n) {
  if (n < 0) return;
  console.log(n);
  countdown(n - 1);
}
countdown(10);
```

함수가 자기 자신을 호출하는 것을 재귀 호출이라고한다. => 재귀 호출을 수행하는 함수가 재귀함수

## 중첩함수

```javascript
function outer() {
  var x = 1;
  // 중첩함수
  function inner() {
    var y = 2;
    // 외부함수의 변수 참조 가능
    console.log(x + y);
  }
  inner();
}
outer();
```

함수 내부에 정의된 함수

## 콜백함수

```javascript
// 외부에서 전달받은 f를 n만큼 호출
function repeat(n, f) {
  for(var i = 1; i<n i++){
    f(i)
  }
}
  var logAll = function (i) {
    console.log(i)
  }

  repeat(5, logAll) // 0 1 2 3 4

  var logOdds = function (i) {
    if(i % 2) console.log(i)
  }
repeat(5,logOdds)//1 3

```

_함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백함수라고 한다._<br/>
_매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차함수라고 한다._<br/>
_고차 함수는 콜백 함수를 자신의 일부분으로 합성하고 매개변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출한다._<br/>
_즉 콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다._<br/>
_ex... map함수, filter함수, reducer 함수 등이 있다._

## 순수 함수와 비 순수 함수

```javascript
// 현재 카운트
var count = 0;
// 순수 함수 increase는 동일한 인수가 전달되면 언제나 동일한 값을 반환
function increase(n) {
  return ++n;
}

// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태변경
count = increase(count);
console.log(count);

count = increase(count);
console.log(count);
```

```javascript
// 현재 카운트
var count = 0;
// 비순수 함수
function increase() {
  return ++count; // 외부 상태를 변경
}

// 비순수 함수는 외부 상태를 변경하므로 상태 변화를 추적하기 어려움
increase();
console.log(count);

increase();
console.log(count);
```

어떤 외부 상태에 의존하지 않고 변경하지도 않는 함수, 부수 효과가 없는 함수 => 순수 함수<br/>
외부 상태에 의존하거나 외부 상태를 변경하는 부수 효과가 있는 함수 => 비순수 함수
