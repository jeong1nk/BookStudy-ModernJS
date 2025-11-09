- 크롬, 사파리, 파이어폭스, 엣지 같은 에버그린 브라우저의 ES6 지원율은 약 98%로 대부분 ES6 사양을 지원한다. 하지만 IE 11의 ES6 지원율을 약 11%다. 그리고 매년 새롭게 도립되는 ES6 이상의 버전과 제안 단계에 있는 ES ㅈ안 사양(ES.NEXT)은 브라우저에 따라 지원율이 제각각 이다.
- 따라서 최신 ECMAScript 사양을 사용해 프로젝트를 진행하려면 최신 사양을 ㅗ작성된 모드를 경우에 따라 IE를 포함한 구형 부라우저에서 문제업이 동작시키기 위한 개발환경을 구축하는 것이 필요하다.
- 또한 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더도 필요하다. ES6 모듈(ESM)은 대부분의 모던 브라우저에서 사용할 수 있다. 하지만 다음과 같은 이유로 아직 ESM 보다는 별도의 모듈 로더를 사용하는 것이 일반적이다.
  - IE를 포함한 구형 브라우저는 ESM을지원하지않는다.
  - ESM을 사용하더라도 트랜스파일링이나 번들링이 필요한 것은 변함이 없다.
  - ESM이 아직 지원하지 않는 기능이 있고 점차 해결되고는 있지만 아직 몇가지 이슈가 존재한다.
- 이번 장에서는 트랜스파일러인 Babel과 모듈 번들러인 Webpack을 이용해 ES6+/ES.NEXT 개발 환경을 구축해보자. 아울라 Webpack을 통해 Babel을 로드해 ES6+/ES.NEXT 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하도록 ES5 사양의 소스코드로 트랜스파일링하는 방법도 알아보자.

# 49.1. Babel
- 아래 예제에는 ES6의 화살표 함수와 ES7의 지수 연산자를 사용하고 있다.
```javascript
[1, 2, 3].map(n => n ** n);
```
- IE 같은 구형 브라우저 에서는 ES6의 화살표 함수와 ES7의 지수 연산자를 지원하지 않을 수 있다. Babel을 사용하면 위 코드를 다음과 같이 ES5사양으로 변환할 수 있다.
```javascript
"use strict";

[1, 2, 3].map(function (n) {
  return Math.pow(n, n);
});
```
- 이처럼 Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드 를 IE 같은 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로  변환(트랜스파일링)할 수 있다. Babel을 사용하기 위한 개발 환경을 구축해보자.

### 49.1.1 Babel 설치
- npm을 이용해 babel을 설치해보자. 터미널에서 다음과 같이 입력해서 Babel을설치한다.
```
# 프로젝트 폴더 생성
$ mkdir esnext-project && cd esnext-project
# package.json 생성
$ npm init  -y
# babel-core, babel-cli 설치
$ npm install --save-dev @babel/core @babel/cli
```
-  설치가 완료된 이후 package.json 파일은 다음과 같다. 불필요한 설정은 삭제했다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3"
  }
}
```
-  참고로 Babel, Webpack, 플러그인의 버전은 빈번하게 업그레이드 된다. `npm install`은 언제나 최신 버전의 패키지를 생성하므로 위 버전과는 다른 최신 버전의 패키지가 설치될 수도 있다. 만약 위 버전 그대로 설치하고 싶다면 아래와 같이  패키지 이름 뒤에 @과 설치하고 싶은 버전을 지정한다.
```
# 버전 지정 설치
$ npm install --save-dev @babel/core@7.10.3 @babel/cli@7.10.3
```

### 49.1.2. Babel 프리셋 설치와 babel.config.json 설정 파일 설정
- Babel을 사용하려면 @babel/preset-env를 설치해야 한다. @babel/preset-env는 함께 사용되어야하는 Babel 플러그인을 모아  둔 것으로 Babel 프리셋이라고 부른다.
-  @babel/preset-env은 필요한 플러그인 들을 프로젝트 지원환경에 맞춰 동적으로 결정해준다. 프로젝트 지원환경은 Browserslist 형식으로  .browserslistrc 파일에 상세히 설정할 수 있다. 만약 프로젝트 지원 환경 설정 작업을  생략하면 기본값으로 설정된다.
-  일단은 기본 설정으로 진행하자. 기본 설정은 모든 ES6+/ES.NEXT 사양의 소스코드를 변환한다.
```
# @babel/preset-env 설치
$ npm install --save-dev @babel/preset-env
```
-  설치가 완료된 이후 package.json 파일은 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/preset-env": "^7.10.3"
  }
}
```
-  설치가 완료되면 프로젝트 루트 폴더에 babel.config.json 설정 파일을 생성하고 다음과 같이 작성한다. 지금 설치한 @babel/preset-env르 사용하겠다는 의미이다.
```json
{
  "presets": ["@babel/preset-env"]
}
```

### 49.1.3. 트랜스파일링
- Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링 해보자. Babel CLI 명령어를 사용할 수도  있으나 트랜스파일링할 때마다  매번 Babel CLI 명령어를 입력하는 것은 번거로우므로 npm scripts에 Babel CLI 명령어를 등록하여 사용하자.
- package.json 파일에 scripts를 추가한다. 완성된 packsge.json 파일은 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/preset-env": "^7.10.3"
  }
}
```
- 위 npm scripts의 build는 src/js 폴더(타깃 폴더에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 그 결과물을 dist/js 폴더에 저장한다. 사용한 옵션의 의미는 다음과 같다.
  - `-w': 타깃 폴더에 잇는 모든 자바스크립트 파일들의 변경을 감지하여 자동으로 트랜스 파일링한다.(--watch 옵션의 축약형)
  - `-d`: 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성한다.(--out-dir 옵션의 축약형)
- 이제 트랜스파일링은 테스트하기 위해 ES6+/ES.NEXT 사양의 자바스크립트 파일을 작성해보자. 프로젝트 루트 폴더에 src.js 폴더를 생성한 후 lib.js와 main.js를 추가한다.
```javascript
// src/js/lib.js
export const pi = Math.PI; // ES6 모듈

export function power(x, y) {
  return x ** y; // ES7: 지수 연산자
}

// ES6 클래스
export class Foo {
  #private = 10; // stage 3: 클래스 필드 정의 제안

  foo() {
    // stage 4: 객체 Rest/Spread 프로퍼티 제안
    const { a, b, ...x } = { ...{ a: 1, b: 2 }, c: 3, d: 4 };
    return { a, b, x };
  }

  bar() {
    return this.#private;
  }
}
```
```javascript
// src/js/main.js
import { pi, power, Foo } from './lib';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());
```
- 터미널에서 다음과 같이 명령어를 입력해 트랜스파일링을 실행한다.
```
$ npm run install

> esnext-project@1.0.0  build /User/폴더주소
> babel src/js -w -d dist/js
```
### 49.1.4. Babel 플러그인 설치
- 설치가  필요한 Babel 플러그인은 Babel 홈페이지에서 검색할 수 있다. Babel 홈페이지 상단 메뉴의  Serch 란에 제안 사양의 이름을 입력하면 해당 플러그인을 검색할 수 있다. 여기서는 피드 정의 제안 플러그인을 검색하기 위해  "class filed"를 입력해보자.

- 검색된 Babel 플러그인 중에서 public/private 클래스 필드를 지원하는 @babel/plugin-proposal-class-properties를 설치하자.
```
$ npm install --save-dev  @babel/plugin-proposal-class-properties
```
- 설치가 완료된 이후 package.json 파일은 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3"
  }
}
```
- 설치한 플러그인은 babel.config.json 설정 파일에 추가해야한다. babel.config.json 설정 파일을 다음과 같이 수정한다.
```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```
- 다시 터미널에서 다음과 같이 명령어를 입력해 트랜스파일링을 실행해보자.
```
$ npm run bulid

> esnext-project@1.0.0  build /User/폴더주소
> babel src/js -w -d dist/js
```
- 트랜스 파일링에 성공하면 프로젝트 루트 폴더에 dist/js 폴더가 자동으로 생성되고 트랜스파일링된 main.js와 lib.js가 저장된다. 트랜스파일링된 main.js를 실행해보자. 결과는 다음과 같다.
```
$ node dist/js/main
3.141592653589793
36.4621596072079
{ a:1, b:2, x: { c:3, d:4 } }
10

> esnext-project@1.0.0  build /User/폴더주소
> babel src/js -w -d dist/js
```

### 49.1.5. 브라우저에서 모듈 로딩 테스트
- 앞에서 main.js와 lib.js 모듈을 트랜스파일링해 ES5 사양으로 변환된 main.js를 실행한 결과 문제없이 실행되는 것을 확인햇다. ES6+에서 새롭게 추가된 기능은 물론 현재 제안 상태에 있는 "클래스 필드 정의 제안"도 ES5도 트랜스파일링되었고 ES6의 모듈의 inport와  export 키워드도 트랜스파일링되어 모듈 기능도 정상적으로 동작하는 것을 확인했다.
- 하지만 위 예제의 모듈 기능은 Node.js 호나경에서 동작한 것이고 Babel이 모듈을 트랜스파일링한 것도 Node.js가 기본 지원하는 CommomJS 방식의 모듈 로딩 시스템에 따른 것이다. 다음은 src/js/main.js가 Babel에 의해 트랜스파일링된 결과다.
```javascript
// dist/js/main.js
"use strict";

var _lib = require("./lib");

// src/js/main.js
console.log(_lib.pi);
console.log((0, _lib.power)(_lib.pi, _lib.pi));
var f = new _lib.Foo();
console.log(f.foo());
console.log(f.bar());
```
- 브라우저는 CommonJS 방식의 require 함수를 지원하지 않으므로  위에서 트램스파일링된 결과를 그대로 브라우저에서 실행하면 에러가 발생한다. 프로젝트 루트 폴더에 다음과 같이 index.html을 작성하여 트랜스파일링된 자바스크립트 파일을 브라우저에서 실행해보자.
```html
<!DOCTYPE html>
<html>
<body>
  <script src="dist/js/lib.js"></script>
  <script src="dist/js/main.js"></script>
</body>
</html>
```
- 위 HTML 파일을 브라우저에서 실행하면 다음과 같은 에러가 발생한다.
```
Uncaught ReferenceError: exports is not defined
  at lib.js
main.js:3 Uncaught ReferenceError: require is not defined
  at main.js:3
```
- 브라우저의 ES6 모듈(ESM)을 사용하도록 Babel을 설정할 수도 있으나 앞서 설명한 바와 같이 ESM을 사용하는 것은 문제가 있다. Webpack을 통해 이러한 문제를 해결해보자.

# 49.2. Webpack
- 🏷️Webpack: 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러다. Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없다. 그리고 여러 개의 자바스크립트 파일을  하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움도 사라진다.
- Webpack과 Babel을 이용해 ES6+/ES.NEXT 개발 환경을 구축해보자. Webpack이 자바스크립트 파일을 번들링하기 전에 Babel을 로드해 ES6+/ES.NEXT 사양의 소스코드를  ES5 사양의 소스코드로 트램스파일링하는 작업을 실행하도록 설정할 것이다.

### 49.2.1. Webpack 설치
- 터미널에서 다음과 같이 명령어를 입력해 Webpack을 설치한다.
```
$ npm install --save-dev webpack webpack-cli
```
- 설치가 완료된 이후 package.json 파일은 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```

### 49.2.2. babel-loader 설치
- Webpack이 모듈을 번들링할 때 Babel을 사용해 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양이 소스코드로  트랜스파일링 하도록 babel-loader을 설치한다.
```
$ npm install --save-dev webpack babel-loader
```
- 이제 npm scripts를 변경해 Babel 대신 Webpack을 실행하도록 수정하자. 다음과 같이 package.json 파일의 scripts를 변경한다. 완성된 package.json 파일은 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "webpack -w"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "babel-loader": "^8.1.0",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```
- 
