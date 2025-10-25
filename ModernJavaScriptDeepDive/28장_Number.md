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
- ES6에서 도입되었으며, 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.
- 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵기 때문에, Number.EPSILON은 부동소수점으로 인해 발생하는 오차르 해결하기 위해 사용한다.
```javascript
0.1 + 0.2;         // -> 0.30000000000000004
0.1 + 0.2 === 0.3; // -> false
```
```javascript
function isEqual(a, b){
  // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // -> true
```

### 28.2.2. Number.MAX_VALUE
- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다.
- Number.MAX_VALUE보다 큰 숫자는 Infinity다.
```javascript
Number.MAX_VALUE; // -> 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // -> true
```

### 28.2.3. Number.MIN_VALUE
- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.
- Number.MIN_VALUE보다 작은 숫자는 0이다.
```javascript
Number.MIN_VALUE; // -> 5e-324
Number.MIN_VALUE > 0; // -> true
```

### 28.2.4. Number.MAX_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.
```javascript
Number.MAX_SAFE_INTEGER; // -> 9007199254740991
```

### 28.2.5. Number.MIN_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.
```javascript
Number.MIN_SAFE_INTEGER; // -> -9007199254740991
```

### 28.2.6. Number.POSITIVE_INFINITY
- 양의 무한대를 나타내는 숫자값 Infinity와 같다.
```javascript
Number.POSITIVE_INFINITY; // -> Infinity
```

### 28.2.7. Number.NEGATIVE_INFINITY
- 음의 무한대를 나타내는 숫자값 -Infinity와 같다.
```javascript
Number.NEGATIVE_INFINITY; // -> -Infinity
```

### 28.2.8. Number.NaN
- 숫자가 아님(Not-a-Number)을 나타내는 숫자값이다.
- Number.Nan은 window.Nan과 같다.
```javascript
Number.NaN; // -> NaN
```

# 28.3. Number 메서드
### 28.3.1. Number.isFinite
- ES6에서 도입되었으며, 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.
```javascript
// 인수가 정상적인 유한수이면 true를 반환한다.
Number.isFinite(0);                // -> true
Number.isFinite(Number.MAX_VALUE); // -> true
Number.isFinite(Number.MIN_VALUE); // -> true

// 인수가 무한수이면 false를 반환한다.
Number.isFinite(Infinity);  // -> false
Number.isFinite(-Infinity); // -> false
```
- 만약 인수가 NaN이면 언제나 false를반환한다.
```javascript
Number.isFinite(NaN); // -> false
```
- 빌트인 전역 함수 isFinite는 전달받은 인수를숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isFinite는 전달받은 인수르숫자로 암묵적 타입 변환하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.
```javascript
// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isFinite(null); // -> false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. null은 0으로 암묵적 타입 변환된다.
isFinite(null); // -> true
```

### 28.3.2. Number.isInteger
