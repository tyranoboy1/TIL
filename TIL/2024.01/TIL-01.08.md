# JavaScript Study

## this 키워드

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.<br/>
이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 _자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다._

```javascript
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야한다.
    return 2 * circle.radius;
  },
};
console.log(circle.getDiameter()); // 10
```

```javascript
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius
}
Circle.prototype.getDiameter = function () {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자로 알 수 없다.
    return 2 * ????.radius
}
// 생성자 함수로 인스턴스를 생성하러면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5)
```

생성자 함수를 정의하는 시점에서 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.<br/>
따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다. => this <br/>
_this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다._<br/>
_this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다._<br/>
_this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다._

```javascript
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  },
};
console.log(circle.getDiameter()); // 10
```

```javascript
// 생성자 함수
function Circle(radius) = {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius
};
Circle.prototype.getDiameter = function () {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius
}
const circle = new Circle(5)
console.log(circle.getDiameter()); // 10
```

_this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this바인딩이 동적으로 결정된다._

```javascript
// this는 어디서든지 참조 가능
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this);
  return number * number;₩
}
square(2);

const person = {
  name: "Lee",
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this.name); // {name: 'Lee', getName:f}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}
const me = new Person("Lee");
```

_this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다._

## 일반 함수 호출

_기본적으로 this에는 전역 객체가 바인딩된다._
_일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다._
_일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다._

## 메서드 호출

## 생성자 함수 호출

_해당 객체나 다른 값을 바라본다_

## Function.prototypem.apply/call/bind

_apply와 call은 메서드의 본질적인 기능인 함수를 호출하는 것_
