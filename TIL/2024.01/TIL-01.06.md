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
