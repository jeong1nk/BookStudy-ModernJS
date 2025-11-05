# 43.1. Ajax란?
- **🏷️Ajax(Asynchronous JavaScript and XML): 자바스크립트를 사용해 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식**
- Ajax는 브라우저에서 제공하는 Web API인 XMLHttpReauest 객체를 기반으로 동작하며, XMLHttpReauest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.
<br />

- 이전의 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작한다. 따라서 화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링 했다.
- 이러한 전통적인 방식은 다음과 같은 단점이 있다.
  1. 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
  2. 변경할 필요가 없는 부분까지 처름부터 다시 렌더링한다. 이로 인해 화면전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.
  3. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.
 
- Ajax의 등장은 이전의 전통적인 패러다임을 획기적으로 전환했다. 즉, **서버로부터 웹페이지 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해졌다.**
- 이를 통해 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른퍼포먼스와 화면 전환이 가능해졌다.
- Ajax은 전통적인 방식과 비교했을 때 다음과 같은 장점이 있다.
  1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 문에 불필요한 데이터 통신이 발생하지 않는다.
  2. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
  3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

# 43.2. JSON
- **🏷️JSON: 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.**
### 43.2.1. JSON 표기 방식
- JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.
```json
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```
- **JSON의 키는 반드시 큰따옴표(작은 따옴표 불가)로 묶어야 한다.**
- 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 **문자열은 반드시 큰따옴표(작은 따옴표 불가)로 묶어야 한다.**

### 43.2.2. JSON.stringify
- 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 **직렬화**라고 한다.
```javascript
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis']
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환되지 않는다.
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === 'number' ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "Lee",
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/
```
- JSON.stringify 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.
```javascript
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);
/*
string [
  {
    "id": 1,
    "content": "HTML",
    "completed": false
  },
  {
    "id": 2,
    "content": "CSS",
    "completed": true
  },
  {
    "id": 3,
    "content": "Javascript",
    "completed": false
  }
]
*/
```

### 43.2.3. JSON.parse
- JSON 포매의 문자열을 객체로 변환한다.
- 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 **역직렬화**라고 한다.
```javascript
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis']
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환한다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: "Lee", age: 20, alive: true, hobby: ["traveling", "tennis"]}
```
- 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다.
- 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.
```javascript
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환한다. 배열의 요소까지 객체로 변환된다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
/*
 object [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
]
*/
```

# 43.3. XMLHttpRequest
- 브라우저는  주소창이나 HTML의 form 캐그 또는 a 캐그를 통해 HTTP 요청 전송 기능을 기본제공한다.
- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.
- Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

### 43.3.1. XMLHttpRequest 객체 생성
- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출해 생성한다.
- XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.

  
##  43.3.2. XMLHttpRequest 객체의 프로퍼티 메서드
- XMLHttpRequest 객체는 다양한 프로퍼티와 메서드를 제공한다.
- 대표적인 프로퍼티와 메서드는 다음과 같으며 중요한 프로퍼티와 메서드는 굵게 표시했다.
#### XMLHttpRequest 객체의 프로토타입 프로퍼티
| 프로토타입 프로퍼티 | 설명 |
| ------------------ | ------|
| **readyState** | HTTP 요청의 현대 상태를 나타내는 정수. 다음과 같은 정적 프로퍼티 값으로 갖는다.<br>- UNSENT: 0<br>- OPENED: 1<br>- HEADERS_RECEIVED: 2<br>- LOADING: 3<br>- DONE: 4 |
| **status** | HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수<br>예) 200 |
| **statusText** | HTTP 요청에 대한 응답 메세지를 나타내는 문자열<br>예) "OK" |
| **responseType** | HTTP 응답 타입<br>예)documetn, json,text,blob,arraybuffer |
| **response** | HTTP 요청에 대한 응답 몸체, responseType에 따라 타입이 다르다 |
| responseText | 서버가 전송한 HTTP 요청에 대한 응답 문자열 |

#### XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티
| 이벤트 핸들러 프로퍼티 | 설명 |
| ------------------ | ------ |
| **onreadystatechange** | readyState 프로퍼티 값이 변경된 경우 |
| onloadstart | HTTP 요청에 대한 응답을 받기 시작한 경우 |
| onprogress | HTTP 요청에 대한 응답을 받는 도중 추기적으로 발생 |
| onabort | abort 메서드에 의해 HTTP 요청이 중단되 경우 |
| **onerror** | HTTP 요청에 에러가 발생한 경우 |
| **onload** | HTTP 요청이 성공적으로 완료한 경우 |
| ontimeout | HTTP 요청 시간이 초과한 경우 |
| onloadend | HTTP 요청이 완료한 경우, HTTP 요청이 성공 또는 실패하면 발생 |

#### XMLHttpRequest 객체의 메서드
| 메서드 | 설명 |
| ------------------ | ------ |
| **open** | HTTP 요청 초기화 |
| **send** | HTTP 요청 전송 |
| **abort** | 이미 전송된 HTTP 요청 중단 |
| **setRequestHeader** | 특정 HTTP 요청 헤더의 값을 설정 |
| setResponseHeader | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

#### XMLHttpRequest 객체의 정적 프로퍼티
| 정적 프로퍼티 | 값 | 설명 |
| ------------- | --- | ------ |
| UNSENT | 0 | open 메서드 호출 이전 |
| OPENED | 1 | open 메서드 호출 이후 |
| HEADERS_RECEIVED | 2 | send 메서드 호출 이후 |
| LOADING | 3 | 서버 응답 중(응답 데이터 미완성 상태) |
| DONE | 4 | 서버 응답 완료 |

### 43.3.3. HTTP 요청 전송
