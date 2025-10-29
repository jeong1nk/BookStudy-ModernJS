# 37.1. Set
- **🏷️Set 객체: 중복되지 않는 유일한 값들의 집합(set)**, 배열과 유사하지만 다음과 같은 차이가 있다.
  - 동일한 값을 중복하여 포함할 수 있다. -> 배열: O / Set 객체: X
  - 요소 순서에 의미가 있다. -> 배열: O / Set 객체: X
  - 인덱스로 요소에 접근할 수 있다.  -> 배열: O / Set 객체: X
- 객체의 특성은 수학적 집합의 특성과 일치하며, Set은 수학적 집합을 구현하기 위한자료구조가.
- 따라서 **Set을 통해 교지합, 합집합, 차집합, 여집합 등을 구할 수 있다.**

### 37.1.1. Set 객체의 생성
- Set 객체는 Set 생성자 함수로 생성한다.
- Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.
```javascript
const set = new Set();
console.log(set); // Set(0) {}
```
- **Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.**
```javascript
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```
- 중복을 허용하지 않는 Set 객체릐 윽성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.
```javascript
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 37.1.2. 요소 개수 확인
- Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.
```javascript
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```
- size 프로퍼티는 setter 함수 없이 getter 함ㅅ만 존재하는 접근자 프로퍼티다.
- 따라서 size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.
```javascript
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

### 37.1.3. 요소 추가
- Set 객체에 요소르 추가할때는 Set.prototype.add 메서드를 사용한다.
```javascript
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```
- add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. 따라서 add 메서드를 호출한 후에 add 메서드를 연속적으로 호출할 수 있다.
```javascript
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2) {1, 2}
```
- Set 객체에 중복된 요소의 추가는 허용되지 않는다. 이때 에러가 발생하지는 않고 무시된다.
```javascript
const set = new Set();

set.add(1).add(2).add(2);
console.log(set); // Set(2) {1, 2}
```
- 일치 비교 연산자 ===을 사용하면 NaN과 NaN을 다르다고 평가한다. 하지만 Set 객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
- +0과 =0은 일치 비교 연산자 ===와 마찬가지로 가다고 평가하여 중복 추가를 허용하지 않는다.
```javascript
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```
- Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.
```javascript
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```

### 37.1.4. 요소 존재 여부 확인
- Set 객체에 특정 요소가 존재하는지 확인하여면 Set.prototype.has 메서드를 사용한다.
- has 메서드는 특별 요소의 존재 여부를 나타내는 불리언 값을 반환한다.
```javascript
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 37.1.5. 요소 삭제
- Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다. delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.
- delete 메서드에는 인덱스가 아니라 삭제하려는 요소 값을 인수로 전달해야한다.
- Set 객체는 순서에 의미가 없다. 즉, 배열과 같이 인덱스를 갖지 않는다.
```javascript
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```
- 만약 존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시된다.
```javascript
const set = new Set([1, 2, 3]);

// 존재하지 않는 요소 0을 삭제하면 에러없이 무시된다.
set.delete(0);
console.log(set); // Set(3) {1, 2, 3}
```
- delete 메서드는 삭제 성공 여부를 나타내느 불리언 값을 반환한다. 따라서 Set.prototype.add 메서드와 달리 연속적으로 호출할 수 없다.
```javascript
const set = new Set([1, 2, 3]);

// delete는 불리언 값을 반환한다.
set.delete(1).delete(2); // TypeError: set.delete(...).delete is not a function
```

### 37.1.6. 요소 일괄 삭제
- Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용한다. clear 메서드는 언제나 undefined를 반환한다.
```javascript
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### 37.1.7. 요소 순회
- Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.
- Set.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달한다. 이때 콜백함수는 다음과 같이 3개의 인수를 전달받는다.
  1. 첫 번째 인수: 현재 순회중인 요소값
  2. 두 번째 인수: 현재 순회중인 요소값
  3. 세 번째 인수: 현재 순회 중인  Set 객체 자체
- 첫 번째와 두 번째 인수는 같은 값이다. 이는 Array.prototype.forEach 메서드와 인터페이스를 통일하기 위함이며 다른 의미는 없다.
- Array.prototype.forEach 메서드의 콜백 함수는 두 번째 인수로 현재 순회 중인 요소의 인덱스를전달 받는다. 하지만 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않는다.

- **Set 객체는 이터러블이다.** 따라서 for ...of 문으로 순회할 수 있으며,  스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

- Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회ㅏ는 순서는 요소가 추가된 순서를 따른다.
- 이는 ECMAScript 사양에 규정되어 있지는 않지만 다른 이터러블의 순회화 호환성을 유지 하기 위함이다.

### 37.1.8. 집합 연산
