# 22.1 this 키워드
- **🏷️this: 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 가리키는 자기 참조 변수**
- this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 **동적으로 결정된다.**
```javascript
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```
- 객체 리터럴의 메서드 내부에서는 tthis는 메서드를 호출한 객체, 즉 circle을 가리킨다.
```javascript
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```
- 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
- **this는 전역이든 함수 내부이든 코드 어디에서든 참조 가능하다.**
```javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```
- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로, 일반으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

# 22.2. 함수 호출 방식과 this 바인딩
- **🏷️this 바인딩: this에 바인딩될 값은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.**
- 함수가 호출하는 방식은 다음과 같이 다양하다.
  1. 일반 함수 호출
  2. 메서드 호출
  3. 생성자 함수 호출
  4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
```javascript
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```

### 22.2.1. 일반 함수 호출
- **기본적으로 this에는 전역 객체가 바인딩된다.**
```javascript
function foo() {
  console.log("foo's this: ", this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```
- 전역 함수는 물론이고 중첩 함수, 콜백 함수 등 **어떠한 함수라도 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.**
```javascript
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this);  // {value: 100, foo: ƒ}
    console.log("foo's this.value: ", this.value); // 100

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }

    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar();
  }
};

obj.foo();
```
```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  }
};

obj.foo();
```
- 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위해서는 다음과 같이 하면 된다.
- 위 방법 이외에도 자바스크립트는 this를 명시적으로 바인딩할 수 있는 `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메서드를 제공한다.
- 또는 화살표 함수를 사용해 this 바인딩을 일치시킬 수도 있다.

### 22.2.2. 메서드 호출
- **메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩 된다.**
- 주의할 점은 메서드를 호출한 객체에 바인딩 된다는 점이다.
```javascript
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```
- 위의 예제에서 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다.
- 따라서 getNatme 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.
```javascript
const anotherPerson = {
  name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```
- 그러므로 **메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩 된다.**
- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // ① Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // ② Kim
```

### 22.2.3. 생성자 함수 호출
- **생성자 함수 내부 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.**
```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### 22.2.4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- **이들 메서드는 `Function.prototype`의 메서드이므로, 모든 함수가 상속받아 사용할 수 있다.**
- `Function.prototype.apply`, `Function.prototype.call`메서드는 사용할 객체와 인수 리스트를 인수로 전달받아 **함수를 호출한다.**
```
/**
* 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.  
* @param thisArg - this로 사용할 객체
* @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
* @returns 호출된 함수의 반환값
*/
Function.prototype.apply(thisArg[, argsArray])

/**
* 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.  
* @param thisArg - this로 사용할 객체
* @param arg1, arg2, ...- 함수에게 전달할 인수 리스트
* @returns 호출된 함수의 반환값
*/
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```
```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```
- apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정객체를 호출한 함수의 this에 바인딩한다.
- `Function.prototype.bind` 메서드는 함수를 호출하지 않고, **첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.**
```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```
- bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치 하는 문제를 해결하기 위해 유용하게 사용된다.
```javascript
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```
