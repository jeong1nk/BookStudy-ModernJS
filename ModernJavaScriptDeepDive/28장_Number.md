- 표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

# 28.1. Number 생성자 함수
- Number 객체는 표준 빌트인 객체로 생성자 함수 객체다. 따라서 **new 연산자와 함께 호출하여 number 인스턴스를 생성할 수 있다.**
- Number 생성자 함수에 인수를 전달하지않고 new 연산자와 함께 호출하면 `[[NumberData]]` 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
```javascript
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```
- Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와  함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.
```javascript
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```
- Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후, [[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체를 생성한다. 인수르 숫자로 변환할 수 없다면 NaN을 [[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체를 생성한다.
```javascript
let numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```
- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다. 이를 이용해 명시적으로 타입을 ㅕㄴ환하기도 한다.
```javascript
// 문자열 타입 => 숫자 타입
Number('0');     // -> 0
Number('-1');    // -> -1
Number('10.53'); // -> 10.53

// 불리언 타입 => 숫자 타입
Number(true);  // -> 1
Number(false); // -> 0
```

# 28.2. Number 프로퍼티
### 28.2.1. Number.EPSILON
