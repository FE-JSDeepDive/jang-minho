## 1. `var` `let` `const` 의 차이점은 무엇인가요?

**`var`**
 - 스코프: 함수 스코프를 가집니다. 함수 내에선 어디서든 접근할 수 있습니다.
 - 재선언 및 재할당: 모두 가능합니다.
 - 호이스팅: 선언부는 호이스팅되지만, 초기화는 해당 줄에서 이루어지기 때문에 선언 전에는 `undefined` 가 할당되어 있습니다.

**`let`**
 - 스코프: 블록 스코프를 가집니다. `{}` 로 감싼 블록 내부에서만 유효합니다.
 - 재선언 및 재할당: 같은 스코프 내에서 재선언은 불가능하지만, 재할당은 가능합니다.
 - 호이스팅 & TDZ: 선언은 호이스팅되지만, **Temporal Dead Zone (TDZ) ** 때문에 선언 전에 접근하면 `ReferenceError` 가 발생합니다.

**`const`**
- 스코프: `let` 과 동일하게 블록 스코프를 가집니다.
- 재선언 및 재할당: 선언과 동시에 초기화가 필요하며, 이후 재할당과 재선언이 불가능합니다. 다만, **객체나 배열의 경우 참조값은 고정되지만 내부 프로퍼티는 변경할 수 있습니다**
- 호이스팅 & TDZ: `let` 과 마찬가지로 호이스팅되지만 TDZ 가 존재합니다.

<br />

## 2. 호이스팅 (Hoisting) 이란 무엇이며, 변수 선언에 어떤 영향을 미치나요?

호이스팅은 JavaScript 가 변수와 함수 선언을 해당 스코프의 최상단으로 끌어올리는 동작을 의미합니다.

**`var`**
- 선언은 호이스팅되어 함수 시작 시 이미 선언된 것으로 간주되지만, 할당은 실제 코드 실행 시점에 이루어집니다. 그래서 선언 전에 변수를 사용하면 `undefined` 가 반환됩니다.
```javascript
console.log(a);  // undefined
var a = 10;
```

**`let` & `const`**
- 선언은 호이스팅되지만, 초기화가 런타임 내에 이루어지기 때문에 초기화 전에 접근하면 **Temporal Dead Zone (TDZ)** 에 있기 때문에 `ReferenceError` 가 발생합니다
```javascript
console.log(b);  // ReferenceError
let b = 20;
```

<br />

## 3. Temporal Dead Zone (TDZ) 이 무엇이며, 왜 발생하나요?

Temporal Dead Zone 은 변수가 블록의 시작부터 실제 선언문에 도달하기 전까지의 구간을 말합니다. 이 기간 동안 해당 변수에 접근하면 `ReferenceError` 가 발생합니다.

```javascript
PI;  // ReferenceError
const PI = 3.14;
```

```javascript
count;  // ReferenceError
let count = 10;
```

```javascript
const myNissan = new Car('red');  // ReferenceError

class Car {
  constructor(color) {
    this.color = color;
  }
}
```

```javascript
class MuscleCar extends Car {
  constructor(color, power) {
    this.power = power;
    super(color);  // TDZ 발생
  }
}

const myCar = new MuscleCar('blue', '300HP');  // ReferenceError
```

다만 `var`, `function`, `import` 는 TDZ 의 영향을 받지 않습니다.

TDZ 는 변수의 초기화가 선언문에서 이루어지기 때문에, 선언 전에 접근하는 것을 방지하여 예기치 않은 버그를 줄이기 위해 존재합니다.

<br />

## 4. const 로 선언한 객체의 프로퍼티를 변경할 수 있는 이유는 무엇인가요?

`const` 는 변수의 재할당을 막지만, 객체나 배열과 같이 참조형 데이터의 경우 **참조값의 불변성**만 보장합니다.

즉 객체의 참조가 바뀌는 것을 막을 뿐, 객체 내부의 프로퍼티는 변경이 가능합니다.

```javascript
const person = { name: 'Alice' };
person.name = 'Bob';

// const person = { name: 'Charlie' }; // 불가능
```

만약 객체 자체를 불변으로 만들고 싶다면, `Object.freeze()` 같은 방법을 사용해야 합니다.

<br />

## 5. var 변수의 함수 스코프에 대해 설명해주세요

`var` 로 선언한 변수는 해당 변수가 속한 함수 전체에서 유효합니다.

이와 달리, `let` 이나 `const` 를 사용하면 블록 스코프가 적용되어 if 문 바깥에서는 접근할 수 없습니다.

```javascript
function testScope() {
  if (true) {
    var message = 'Hello World!';
  }
  console.log(message);
}
```

<br />

## 6. 값 타입 (primitive) 과 참조 타입 (reference) 변수의 메모리 저장 및 처리 방식에 대해 설명해주세요

값 타입 (Primitive Type)
`number`, `string`, `boolean`, `null`, `undefined`, `Symbol`, `BigInt` 는 **값 자체** 가 변수에 저장됩니다.

- 예시
result -> 0xffe451 -> 10


참조 타입(Reference Type)
객체, 배열, 함수 등은 **메모리의 주소(참조값)** 가 변수에 저장됩니다. 따라서 변수 간의 대입 시 **같은 객체에 대한 참조**가 전달됩니다.

- 예시:
result -> 0xffe451 -> 1rxx0e6w, 1rxx0e6

3i5583xe -> 'hello'
3i72snqr -> 'world'

1rxx0e6w -> 3i5583xe
1rxx0e6 -> 3i72snqr

<br />

## 7: 자바스크립트에서 암묵적 타입 변환이 발생하는 주요 상황과 이를 피하기 위한 방법을 설명해주세요 
자바스크립트는 동적 타입 언어이므로 연산 중 피연산자의 타입이 다를 경우 암묵적 타입 변환(Type Coercion)이 발생합니다. 주요 사례는 다음과 같습니다다.

- **문자열 변환(String Conversion)**
  ```js
  console.log('5' + 3);  // "53"
  console.log(3 + '5');  // "35"
  ```
  문자열 연결 연산자로 인해 `+` 연산자가 피연산자 중 하나를 문자열로 변환합니다.

- **숫자 변환(Number Conversion)**
  ```js
  console.log('5' - 3);  // 2
  console.log('5' * '2');  // 10
  ```
  덧셈(`+`)을 제외한 산술 연산자는 문자열을 숫자로 변환합니다.

- **불리언 변환(Boolean Conversion)**
  ```js
  console.log(Boolean(0));  // false
  console.log(Boolean(''));  // false
  console.log(Boolean('false'));  // true
  ```
  falsy 값(`0`, `""`, `null`, `undefined`, `NaN`, `false`)은 `false`로 변환합니다.

- **암묵적 변환을 피하는 방법**
  - `===`(엄격한 비교) 사용
  - `Number()`, `String()`, `Boolean()` 등 명시적 변환 사용

<br />

## 8. `typeof` 연산자의 결과가 예측과 다르게 동작하는 경우를 예제와 함께 설명하고, 이를 보완하는 방법을 제시하세요.
```js
console.log(typeof null);  // "object"
console.log(typeof []);  // "object"
console.log(typeof function(){});  // "function"
```
- `typeof null`이 `"object"`로 나오는 이유는 자바스크립트 초기 버전의 버그 때문이며, 현재까지 수정되지 않았습니다.
- `typeof []`가 `"object"`를 반환하므로 배열과 일반 객체를 구분할 수 없습니다.

<br />

```js
console.log(Array.isArray([]));  // true
console.log([] instanceof Array);  // true
```
객체의 상세한 타입을 확인하려면 `Object.prototype.toString.call()`을 활용할 수도 있습니다.
```js
console.log(Object.prototype.toString.call([]));  // "[object Array]"
console.log(Object.prototype.toString.call(null));  // "[object Null]"
```

<br />

## 9. 자바스크립트에서 숫자 타입과과 관련하여 주의해야 할 점을 설명하고, 안전한 숫자 연산을 위한 방법을 제시하세요.
- 모든 숫자는 **배정밀도 64비트 부동소수점**(`IEEE 754`)으로 저장됩니다.
- 정수와 실수 구분 없이 `number` 타입 하나만 존재합니다.
- `Infinity`, `-Infinity`, `NaN` 같은 특별한 값이 존재합니다다.

<br />

```js
console.log(0.1 + 0.2);  // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3);  // false
```

<br />

- **반올림을 이용한 해결**
  ```js
  console.log(Math.round((0.1 + 0.2) * 100) / 100);  // 0.3
  ```
- **`BigInt` 사용 (정수 연산에 한함)**
  ```js
  console.log(10n + 20n);  // 30n
  ```
- **`Number.EPSILON` 활용**
  ```js
  function isEqual(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
  }
  console.log(isEqual(0.1 + 0.2, 0.3));  // true
  ```

> [!Tip]
> `Number.EPSILON` 은 자바스크립트가 식별할 수 있는 가장 작은 값입니다.