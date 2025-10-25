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
- 빌트인 전역 함수 isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.
```javascript
// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isFinite(null); // -> false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. null은 0으로 암묵적 타입 변환된다.
isFinite(null); // -> true
```

### 28.3.2. Number.isInteger
- ES6에서 도입되었으며, 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
- 검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.
```javascript
// 인수가 정수이면 true를 반환한다.
Number.isInteger(0)     // -> true
Number.isInteger(123)   // -> true
Number.isInteger(-123)  // -> true

// 0.5는 정수가 아니다.
Number.isInteger(0.5)   // -> false
// '123'을 숫자로 암묵적 타입 변환하지 않는다.
Number.isInteger('123') // -> false
// false를 숫자로 암묵적 타입 변환하지 않는다.
Number.isInteger(false) // -> false
// Infinity/-Infinity는 정수가 아니다.
Number.isInteger(Infinity)  // -> false
Number.isInteger(-Infinity) // -> false
```

### 28.3.3. Number.isNaN
- ES6에서 도입되었으며, 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.
```javascript
// 인수가 NaN이면 true를 반환한다.
Number.isNaN(NaN); // -> true
```
- 빌트인 전역 함수 isNaN는 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isNaN는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.
```javascript
// Number.isNaN은 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isNaN(undefined); // -> false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. undefined는 NaN으로 암묵적 타입 변환된다.
isNaN(undefined); // -> true
```

### 28.3.4. Number.isSafeInteger
- ES6에서 도입되었으며, 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
- 안전한 정수값은 -(2^53 - 1)과 2^53 - 1 사이의 정수값이다
- 검사 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.
```javascript
// 0은 안전한 정수이다.
Number.isSafeInteger(0); // -> true
// 1000000000000000은 안전한 정수이다.
Number.isSafeInteger(1000000000000000); // -> true

// 10000000000000001은 안전하지 않다.
Number.isSafeInteger(10000000000000001); // -> false
// 0.5은 정수가 아니다.
Number.isSafeInteger(0.5); // -> false
// '123'을 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger('123'); // -> false
// false를 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger(false); // -> false
// Infinity/-Infinity는 정수가 아니다.
Number.isSafeInteger(Infinity); // -> false
```

### 28.3.5. Number.prototype.toExponential
- 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.
- 지수 표기법이란 매우 크거나 작은 숫잘르 표기할 때 주로 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.
- 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.
```javascript
(77.1234).toExponential();  // -> "7.71234e+1"
(77.1234).toExponential(4); // -> "7.7123e+1"
(77.1234).toExponential(2); // -> "7.71e+1"
```
- 숫자 리터럴과 함께 Number 프로토타입 메서드르 사용할 경우 에러가 발생한다.
```javascript
77.toExponential(); // -> SyntaxError: Invalid or unexpected token
```
- 자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석하지만, .toExponential()에서 . 앞의 숫자는 Number 래퍼 객체이다. 그러므로 .을 소수 구분 기호로 해석하면 프로퍼티로 해석할 수 없으므로 에러가 발생한다.
```javascript
77.1234.toExponential(); // -> "7.71234e+1"
```
- 위 예제의 경우 77. 다음에 숫자가 이어지므로 .은 명백하게 부동 소수점 숫자의 소수 구분 기호이다. 숫자에 소수점은 하나만 존재하기 때문이다.
- 그러므로 두번째 .의 경우 프로퍼티 접근 연산자로 해석된다.
- 따라서 숫자 리터럴과 함께 메서들르 사용할 경우 혼란 방지를 위해 그룹 연산자를 사용할 것을 권장한다.
```javascript
(77).toExponential(); // -> "7.7e+1"
```

### 28.3.6. Number.prototype.toFixed
- 숫자를 반올림하여 문자열로 반환한다.
- 반올림하는 소수점 이라 자릿수를 나타내는 0~20사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 0이 지정된다.
```javascript
// 소수점 이하 반올림. 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toFixed(); // -> "12346"
// 소수점 이하 1자리수 유효, 나머지 반올림
(12345.6789).toFixed(1); // -> "12345.7"
// 소수점 이하 2자리수 유효, 나머지 반올림
(12345.6789).toFixed(2); // -> "12345.68"
// 소수점 이하 3자리수 유효, 나머지 반올림
(12345.6789).toFixed(3); // -> "12345.679"
```

### 28.3.7. Number.prototype.toPrecision
- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.
- 인수로 전달받은 전제 자릿수로 표현할 수 없는 경우 지수 표기법을 ㅗ결과를 반환하낟.
- 전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 0이 지정된다.
```javascript
// 전체 자리수 유효. 인수를 전달하지 않으면 기본값 0이 전달된다.
(12345.6789).toPrecision(); // -> "12345.6789"
// 전체 1자리수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // -> "1e+4"
// 전체 2자리수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // -> "1.2e+4"
// 전체 6자리수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // -> "12345.7"
```

### 28.3.8. Number.prototype.toString
- 숫자를 문자열로 변환하여 반환한다.
- 진법을 나타내는 2~3 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하며 기본값 10진법이 지정된다.
```javascript
// 인수를 생략하면 10진수 문자열을 반환한다.
(10).toString(); // -> "10"
// 2진수 문자열을 반환한다.
(16).toString(2); // -> "10000"
// 8진수 문자열을 반환한다.
(16).toString(8); // -> "20"
// 16진수 문자열을 반환한다.
(16).toString(16); // -> "10"
```
  
