- **🏷️DOM(Documnet Object Model): HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 aPI, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조**

# 39.1 노드
### 39.1.1. HTML 요소와 노드 객체
- **🏷️HTML 요소:HTML 문서를 구성하는 개별적인 요소**

<img width="780" height="210" alt="Image" src="https://github.com/user-attachments/assets/29938ae2-3642-43fa-af73-5afd1d996673" />

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 어트리뷰트는 **어트리뷰트 노드**로, HTML 요소의 텍스트 콘텐츠는 **텍스트 노드**로 변환된다.

<img width="780" height="206" alt="Image" src="https://github.com/user-attachments/assets/0870dc5d-123f-4bfb-b47e-d0e113f00fdc" />

- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩 관계를 갖는다. 즉, HTML 요소의 콘텐츠 영역( 시작 태그와 종료 태그 사이)에는 텍스트 뿐만아니라 다른 HTML 요소도 포함할 수 있다.
- 이때 HTML 요소 간에는 중첩 관계에 의해 계층적인 부자 관계가 형성된다. 이러한 HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성요소인 HTML 요소를 객체화한 모드 노드 객체들을 트리 자료 구조로 구성한다.

#### 트리 자료구조
- 🏷️트리 자료구조: 노드들의 계층구조로 이뤄진다. 즉, **부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조(부자, 형제 관계)를 표현하는 비선형 자료구조를 말한다.**
- 트리 자료구조는 하나의 최상위 노드에서 시작한다. 최상위 노드는 부모 노드가 없으며, 루트 노드라한다.
- 루트 노드는 0개 이상의 자식 노드를 갖는다. 자식 노드가 없는 노드를 리프 노드라 한다.
- **노드 객체들로 구성된 트리 자료구조를 DOM이라 한다.** 노드 객체의 트리로 구조화되어 있기 때문에 DOM을 DOM 트리라고 부르기도 한다.

### 39.1.2. 노드 객체의 타입
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```
- 예를 들어 위의 HTML 문서를 렌더링 엔진이 파싱한다고 했을때, 렌더링 엔진은 위 HTML 문서를 파싱해 DOM을 생성한다.

<img width="1023" height="426" alt="Image" src="https://github.com/user-attachments/assets/38737e14-bab1-44e9-a0ba-4c7bee32ac1b" />

- 이처럼 DOM은 노드 객체의 **계층적인 구조**로 구성된다.
- **노드 객체는 총 12개의 종류(타입)가 있고 상속 구조를 갖는다.** 중요한 노드 타입은 다음과 같이 4가지 이다.
  - **1. 문서 노드(document)**
    - DOM 트리의 최상위에 존재하는 루트 노드로서, document 객체를 가리킨다.
    - document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩 되어 있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.
    - 모든 자바스크립트 코드는 전역 객체 window의 documnet 프로퍼티에 바인딩 되어있는 하나의 document 객체를 바라본다. 즉**HTML 문서당 documwnt 객체는 유일하다.**
    - 문서 노드, 즉 document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당한다. 즉, 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.

  - **2. 요소 노드(element)**
    - HTML 요소를 가리키는 객체로, 문서의 구조를 표현한다.
    - HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화 한다.

  - **3. 어트리뷰트 노드**
    - HTML 요소의 어트리뷰트를 가리키는 객체
    - 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 단, 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.
    - 즉, 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다. 
 
  - **4. 텍스트 노드**
    - HTML 요소의 텍스트를 가리키는 객체
    - 요소 노드가 문서의 구조를 표현한다면 텍스트 노드는 문서의 정보를 표현한다고 할 수 있다.
    - 텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드이다.
    - 즉, **텍스트 노드는 DOM 트리의 최종단**이다. 따라서 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 한다.

- 위 4가지 노드 타입을 제외한 노드는 후반에서 다룰 예정

### 39.1.3. 노드 객체의 상속 구조
- DOM은 HTML 문서의 계층적 수조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조이다.
- 즉 DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 이를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.
- DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 **브라우저 환경에서 추가적을 제공하는 호스트 객체다.** 하지만 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.

<img width="891" height="391" alt="Image" src="https://github.com/user-attachments/assets/386f8028-668a-4ee4-9873-6a910955f62b" />

- 모든 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.
- 요소노드는 Element 인터페이스를 상속받는다. 또한 요소노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElement, HTMLHeadElement 등의인터페이스를 상속 받는다.
- 예를 들어, input 요소를 파싱하여 객체화한 input 요소 노드 객체는, 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.
```html
<!DOCTYPE html>
<html>
<body>
  <input type="text">
  <script>
    // input 요소 노드 객체를 선택
    const $input = document.querySelector('input');

    // input 요소 노드 객체의 프로토타입 체인
    console.log(
      Object.getPrototypeOf($input) === HTMLInputElement.prototype,
      Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
      Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
      Object.getPrototypeOf(Element.prototype) === Node.prototype,
      Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
      Object.getPrototypeOf(EventTarget.prototype) === Object.prototype
    ); // 모두 true
  </script>
</body>
</html>
```
- 배열이 객체인 동시에 배열인 것처럼 input 요소 노드 객체도 다음과 같이 다양한 특성을 갖는 객체이며, 이러한 특성을 나타내는 기능들을 상속을 통해 제공받는다.

| input 요소 노드 객체의 특성 | 프로토타입을 제공하는 객체 |
|------------------------------|-----------------------------|
| 객체 | Object |
| 이벤트를 발생시키는 객체 | EventTarget |
| 트리 자료구조의 노드 객체 | Node |
| 브라우저가 렌더링 할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체 | HTMLElement |
| HTML 요소 중에서 input 요소를 표현하는 객체 | HTMLInputElement |

- 노드 객체의 상속 구조는 개발자 도구의 Elements 패널 우측의 Properties 패널에서 확인할 수 있다.
- 노드 객체에는 노드 객체의 종료, 즉 노드 타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고, 노드 타입에 따라 고유한 기능도 있다. 또한 모든 노드 객체는 트리 자료구조의 노드로서 공통적으로 트리 탐색기능이나 노드 정보 제공 기능이 필요하다. 이 같은 노드 관련 기능은 Node 인터페이스가 제공한다.
- HTML 요소가 객체화된 요소 노드 객체는 HTML 요소가 갖는 공통적인 기능잉 있다. 이는 HTMLElement 인터페이스가 제공한다.
- 하지만 요소 노드 객체는 HTML 요소의 종류에 따라 교유한 기능도 있다. 따라서 필요한 기능을 제공하는 인터페이스가 HTML 요소의 종류에 따라 각각 다르다.
- 이처럼 노드 객체는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.
<br />

- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOMAPI로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는스타일 등을 동적으로 조작할 수 있다.
- 여기서 중요한 것은 DOM API, 즉 DOM이 제공하는 프로퍼티와 메서드를 사용해 노드에 접근하고 HTML의 구조나 내용 또는 스타일 등을 동적으로 변경하는 방법을 익히는 것이다.
- 즉, HTML을 DOM과 연관지어 바라보아야 한다.

# 39.2. 요소 노드 취득
- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 **먼저 요소 노드를 취득해야 한다.** 텍스트 노드는 요소 노드의 자식 노드이고, 어트리뷰트 노드는 요소 노드와 연결되어 있기 때문에 텍스트 노드나 어트리뷰트 노드를 조작하고 할 때도 마찬가지다.
- 이처럼 요소 노드의 취득은 HTML 요소를 조작하는 시작점이다. 이를 위해 DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.

### 39.2.1. id를 이용한 요소 노드 취득
- Documnet.prototype.getElementById 메서드는 인수로 전달한 id 어트리튜브 값(이하 id 값)을 갖는 하나의 요소 노드를 탐색하여 반환한다.
- getElementById 메서드는 Documnet.prototype의 프로퍼티다. 따라서 반드시 문서 노드인 document를 통해 호출해야한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 'banana'인 요소 노드를 탐색하여 반환한다.
      // 두 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById('banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```
- id 값은 HTML 문서 내에서 유일한 값이어야 하며, class 어트리뷰트와는 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없다. 단, HTML 문서 내에 중복된 id 값을 갖는 HTML 요소가 여러개 존재하더라도 어떠한 에러도 발생하지 않는다.
- **즉, HTML 문서 내에는 중복된 id 값을 갖는 요소가 여러개 존재할 가능성이 있다.**
- 이러한 경우 getElementById 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환한다. 즉, **getElementById 메서드는 언제나 단 하나의 요소 노드를 반환한다.**
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="banana">Apple</li>
      <li id="banana">Banana</li>
      <li id="banana">Orange</li>
    </ul>
    <script>
      // getElementById 메서드는 언제나 단 하나의 요소 노드를 반환한다.
      // 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById('banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```
- 만약 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 getElementById 메서드는 null을 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 'grape'인 요소 노드를 탐색하여 반환한다. null이 반환된다.
      const $elem = document.getElementById('grape');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
      // -> TypeError: Cannot read property 'style' of null
    </script>
  </body>
</html>
```
- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적을 ㅗ선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      // id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다.
      console.log(foo === document.getElementById('foo')); // true

      // 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않는다.
      delete foo;
      console.log(foo); // <div id="foo"></div>
    </script>
  </body>
</html>
```
- 단, id값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다.
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      let foo = 1;

      // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
      console.log(foo); // 1
    </script>
  </body>
</html>
```

### 39.2.2. 태그 이름을 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다.
- 메서드 이름에 포함된 Elements가 복수형인 것에서 알 수 있듯이 getElementsByTagName 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      // 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elems = document.getElementsByTagName('li');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
      [...$elems].forEach(elem => { elem.style.color = 'red'; });
    </script>
  </body>
</html>
```
- 함수는 하나의 값만 반환할 수 있으므로 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료 구조에 담아 반환해야 한다.
- getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
- HTML 문서의 모든 요소 노드를 취득하려면 getElementsByTagName 메서드의 인수로 '*'를 전달한다.
```javascript
// 모든 요소 노드를 탐색하여 반환한다.
const $all = document.getElementsByTagName('*');
// -> HTMLCollection(8) [html, head, body, ul, li#apple, li#banana, li#orange, script, apple: li#apple, banana: li#banana, orange: li#orange]
```
- getElementsByTagName 메서드는 Documnet.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다.
- Document.prototype.getElementsByTagName 메서드는 DOM의 루트 노드인 문서 노드, 즉 documnet를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다. 하지만 Element.prototype.getElementsByTagName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      const $lisFromDocument = document.getElementsByTagName('li');
      console.log($lisFromDocument); // HTMLCollection(4) [li, li, li, li]

      // #fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두
      // 탐색하여 반환한다.
      const $fruits = document.getElementById('fruits');
      const $lisFromFruits = $fruits.getElementsByTagName('li');
      console.log($lisFromFruits); // HTMLCollection(3) [li, li, li]
    </script>
  </body>
</html>
```
- 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByTagName 메서드는 빈 HTMLCollection 객체를 반환한다.

### 39.2.3. class를 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값(이하 class 값)을 갖는 모든 요소 노드들을 탐색하여 반환한다.
- 인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class를 지정할 수 있다.
- getElementsByClassName 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="fruit apple">Apple</li>
      <li class="fruit banana">Banana</li>
      <li class="fruit orange">Orange</li>
    </ul>
    <script>
      // class 값이 'fruit'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName('fruit');

      // 취득한 모든 요소의 CSS color 프로퍼티 값을 변경한다.
      [...$elems].forEach(elem => { elem.style.color = 'red'; });

      // class 값이 'fruit apple'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $apples = document.getElementsByClassName('fruit apple');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      [...$apples].forEach(elem => { elem.style.color = 'blue'; });
    </script>
  </body>
</html>
```
- getElementsByClassName 메서드는 Documnet.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다.
- Document.prototype.getElementsByClassName 메서드는 DOM의 루트 노드인 문서 노드, 즉 documnet를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다. 하지만 Element.prototype.getElementsByClassName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <div class="banana">Banana</div>
    <script>
      // DOM 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $bananasFromDocument = document.getElementsByClassName('banana');
      console.log($bananasFromDocument); // HTMLCollection(2) [li.banana, div.banana]

      // #fruits 요소의 자손 노드 중에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $fruits = document.getElementById('fruits');
      const $bananasFromFruits = $fruits.getElementsByClassName('banana');

      console.log($bananasFromFruits); // HTMLCollection [li.banana]
    </script>
  </body>
</html>
```

- 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByClassName 메서드는 빈 HTMLCollection 객체를 반환한다.

### 39.2.4. CSS 선택자를 이용한 요소 노드 취득
