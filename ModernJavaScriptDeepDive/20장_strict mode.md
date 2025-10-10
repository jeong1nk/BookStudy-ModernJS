# 20.1. strict mode란?
- 선언하지 않은 변수 x가 있을 경우, 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다. (x 프로퍼티는 전역 변수처럼 사용 가능) 이러한 현상을 **🏷️암묵적 전역**이라고한다.
- 개발자의 의도와는 상관없이 발생한 암묵전 전역은 오류를 발생시키는 원인이 될 가능성이 크다. 따라서 반드시 var, let, const 키워드를 사용해 변수를선언한 다음 사용해야 한다.
<br />

-  🏷️**strict mode: 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.**
-  strict mode 대신 ESLint 같은 린트 도구를 사용해도 유사한 효과를 얻을 수 있다. 린트 도구는 문법적 오류 뿐만이 아닌 잠재적 오류까지 찾아내고 오류의 원인을 리포팅 해주는 유용한 도구이다.
-  린트 도구의 사용은 'https://poiemaweb.com/eslint'에서 참고할 수 있다.

# 20.2. strict mode의 적용
- strict mode를 사용하기 위해서는 전역 또는 함수 몸체의 선두에 `use strict;`를 추가하면 된다.
```javascript
function foo() {
  'use strict';

  x = 10; // ReferenceError: x is not defined
}
foo();
```
- 전역의 선두에 추가할 경우 스크립트 전체에, 함수 몸체의 선두에 추가할 경우 해당 함수와 중첩 함수에 strict mode가 적용된다.
```javascript
function foo() {
  x = 10; // 에러를 발생시키지 않는다.
  'use strict';
}
foo();
```
- `use strict;`를 선두에 위치시키지 않으면 strict mode가 제대로 동작하지 않는다.

# 20.3. 전역에 strict mode를 적용하는 것은 피하자
- 전역에 적용한 strict mode는 스크립트 단위로 적용된다.
```html
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict';
  </script>
  <script>
    x = 1; // 에러가 발생하지 않는다.
    console.log(x); // 1
  </script>
  <script>
    'use strict';

    y = 1; // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```
- strict mode 스크립트와 non-strict mode를 혼용하는 것은 오류를 발생시킬 수 있다.
- 특히 🔖외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다.
- 이 경우에는 즉시 실행 함수로 스크립트 전체를 감싸 스코프를 구분하고 즉시 실행 함수늬 선두에 strict mode를 적용하자.
```javascript
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
```

# 20.4. 함수 단위로 strict mode를 적용하는 것도 피하자
- 어떤 함수는 strict mode를 적용하고 어떤 함수는 적용 안 하는 것은 바람직하지 않고, 또 일일히 모든 함수에 적용하는 것은 번거롭다.
- strict mode가 적용된 함수가 참조할 함수 외부의 컨택스트에 strict mode를 적용하지 않으면 문제가 발생할 수 있다.
```javascript
(function () {
  // non-strict mode
  var lеt = 10; // 에러가 발생하지 않는다.

  function foo() {
    'use strict';

    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
}());
```
- 따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

# 20.5. strict mode가 발생시키는 에러
