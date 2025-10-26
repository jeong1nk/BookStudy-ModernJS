# 29.1. Math 프로퍼티
### 29.1.1. Math.PI
- 원주율 PI 값(3.141592653589793)을 반환한다.
```javascript
Math.PI; // -> 3.141592653589793
```

# 29.2. Math 메서드
### 29.2.1. Math.abs
- 인수로 전달된 숫자의 절대값을 반환한다.
- 절대값은 반드시 0 또는 양수이어야 한다.
```javascript
Math.abs(-1);        // -> 1
Math.abs('-1');      // -> 1
Math.abs('');        // -> 0
Math.abs([]);        // -> 0
Math.abs(null);      // -> 0
Math.abs(undefined); // -> NaN
Math.abs({});        // -> NaN
Math.abs('string');  // -> NaN
Math.abs();          // -> NaN
```

### 29.2.2. Math.round
- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.
```javascript
Math.round(1.4);  // -> 1
Math.round(1.6);  // -> 2
Math.round(-1.4); // -> -1
Math.round(-1.6); // -> -2
Math.round(1);    // -> 1
Math.round();     // -> NaN
```

### 29.2.3. Math.ceil
- 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.
- 소수점 이하를 올림하면 더 큰 정수가 된다. (예. 1.4의 소수점 이하를 올림하면 2가 되고, -1.4의 소수점 이하를 올림하면 -1)
```javascript
Math.ceil(1.4);  // -> 2
Math.ceil(1.6);  // -> 2
Math.ceil(-1.4); // -> -1
Math.ceil(-1.6); // -> -1
Math.ceil(1);    // -> 1
Math.ceil();     // -> NaN
```

### 29.2.4. Math.floor
- 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.(Math.ceil 메서드의 반대 개념)
```javascript
Math.floor(1.9);  // -> 1
Math.floor(9.1);  // -> 9
Math.floor(-1.9); // -> -2
Math.floor(-9.1); // -> -10
Math.floor(1);    // -> 1
Math.floor();     // -> NaN
```

### 29.2.5. Math.sqrt

### 29.2.6. Math.random

### 29.2.7. Math.pow

### 29.2.8 Math.max

### 29.2.9. Math.min
