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
