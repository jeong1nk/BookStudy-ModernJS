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

### 49.1.4. Babel 플러그링 설치

### 49.1.5. 브라우저에서 모듈 로딩 테스트
