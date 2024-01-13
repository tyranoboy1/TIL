# JavaScript Study

## 렉시컬 스코프

```javascript
const x = 1;
function outerFunc() {
  const x = 10;
  function innerFunc() {
    console.log(x); //10
  }
  innerFunc();
}
outerFunc();
```

```javascript
const x = 1;
function outerFunc() {
  const x = 10;
  innerFunc();
}
function innerFunc() {
  console.log(x); //1
}
outerFunc();
```

_자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다._<br/>
_이를 렉시컬 스코프(정적 스코프)라고 한다._

```javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}
function bar() {
  console.log(x);
}
foo();
bar();
```

_렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다. => 렉시컬 스코프_<br/>
_함수는 자신의 내부 슬롯에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다._<br/>

## 함수 객체의 내부 슬롯 [[Environment]]

_함수 객체의 내부 슬롯에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프이다. 또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값이다._<br/>
_함수 객체는 내부 슬롯에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다._<br/>

```javascript
const x = 1;
function foo() {
  const x = 10;
  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
  // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
  bar();
}
// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Enviroment]]에 저장하여 기억한다.
function bar() {
  console.log(x); // 1
}
foo();
bar();
```

_외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯[[Enviroment]]에 저장된 렉시컬 환경의 참조가 할당된다._

## 클로저와 렉시컬 환경

```javascript
const x = 1;
// 1)
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x); // 2)
  };
  return inner;
}
// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
// 함수의 생명주기가 끝나고 팝되어 제거된 함수의 지역변수를 참조
const innerFunc = outer(); // 3)
innerFunc(); //4) 10
```

_외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다._<br/>
_이러한 중첩 함수를 클로저라고한다._<br/>
_outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다._

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      function foo() {
        const x = 1;
        const y = 2;
        // 일반적으로 클로저라고 하지 않는다.
        function bar() {
          const z = 3;

          debugger;
          // 상위 스코프의 식별자를 참조하지 않는다.
          console.log(z);
        }
        return bar;
      }
      const bar = foo();
      bar();
    </script>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      function foo() {
        const x = 1;

        // bar 함수는 클로저였지만 곧바로 소멸한다.
        // 이러한 함수는 일반적으로 클로저라고 하지 않는다.
        function bar() {
          debugger;
          // 상위 스코프의 식별자를 참조한다.
          console.log(x);
        }
        bar();
      }
      foo();
    </script>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      function foo() {
        const x = 1;
        const y = 2;

        // 클로저
        // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
        function bar() {
          debugger;
          // 상위 스코프의 식별자를 참조한다.
          console.log(x);
        }
        return bar;
      }
      const bar = foo();
      bar();
    </script>
  </body>
</html>
```

_클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다._<br/>
_클로저에 의해 참조되는 상위 스코프에 변수를 자유 변수라고 부른다. => 클로저란 함수가 자유 변수에 대해 닫혀있다. 라는 의미이다._

## 클로저의 활용

_클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다._<br/>
_상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다._<br/>

```javascript
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가시킨다.
  return ++num;
};
console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

오류를 발생시킬 가능성을 내포하고 있는 코드<br/>
전제 조건<br/>

1. 카운트 상태(num 변수의 값)는 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.<br/>
2. 이를 위해 카운트 상태(num 변수의 값)는 increase 함수만이 변경할 수 있어야 한다.

```javascript
// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태 변수
  let num = 0;
  // 카운트 상태를 1만큼 증가시킨다.
  return ++num;
};
// 이전 상태를 유지하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

```javascript
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;
  // 카운트 상태를 1만큼 증가시킨다.
  return function () {
    // 카운트 상태를 1만큼 증가시킨다.
    return ++num;
  };
})();
console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

_클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다._

```javascript
// 카운트 상태 변경 함수
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;
  // 클로저인 메서드를 갖는 객체를 반환한다.
  // 객체 리터럴 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
  return {
    // num:0, // 프로퍼티는 public하므로 은닉되지 않는다.
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    },
  };
})();
console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

```javascript
const Counter = (function () {
  // 1) 카운트 상태 변수
  let num = 0;

  function Counter() {
    // this.num = 0 // 2) 프로퍼티는 public하므로 은닉되지 않는다.
  }
  Counter.prototype.increase = function () {
    return ++num;
  };
  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0;
  };
  return Counter;
})();
const counter = new Counter();
console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

```javascript
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}
// 보조 함수
function increase(n) {
  return ++n;
}
// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다.
const increaser = makeCounter(increase); // 1)
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // 2)
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

_makeCounter 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다_
