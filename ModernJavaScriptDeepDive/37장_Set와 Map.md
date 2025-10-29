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
