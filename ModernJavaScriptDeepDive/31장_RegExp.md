# 31.1. 정규 표현식이란?
- **🏷️정규 표현식: 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal lauguage)**
- 정규 표현식은 자바스크립트의 고유문법이 아니며, 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다. 자바스크립트는 펄의 정규 표현식 문법을 ES3부터 도입했다.
- 정규 표현식은 **문자열을 대상으로 패턴 매칭 기능**을 제공한다.
- **패턴 매칭 기능: 특정 패턴과 일치하는 문자열을검색하거나 추출 또는 치환할 수 있는 기능**
- 그 예로 휴대폰 전화번호가 유효한 전화번호인지 체크하는 경우가 있다.
```javascript
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```
- 만약 정규 표현식이 없다면 반복문과 조건문을 통해 '첫 번째 문자가 숫자이고 이어지는 문자도 숫자이고...'와 같이 한 문자씩 연속해서 체크해야 한다.
- 다만, 정규 표현식은 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다는 문제가 있다.

# 31.2. 정규 표현식의 생성
- 정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.

1. 정규 표현식 리터럴
- `/regexp/i` : regexp = 패턴, i = 플래그 / = 시작,종료기호
- 정규 표현식 리터럴은 패턴과 플래그로 구성된다.
```javascript
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // -> true
```

2. RegExp 생성자 함수
```javascript
/**
 * parttern: 정규 표현식의 패턴
 * flags: 정규 표현식의 플래그(g, i, m, u, y)
 */
new RegExp(parttern[, flags])
```
```javascript
const target = 'Is this all there is?';

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // -> true
```
- RegExp 생성자 함수를 사용하여 RegExp 객체를 생성할 수도 있다.
- RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.
```javascript
const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is'); // -> 3
count('Is this all there is?', 'xx'); // -> 0
```

# 31.3 RegExp 메서드
