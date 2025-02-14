## 1. `var` `let` `const` 의 차이점은 무엇인가요?

`var`
 - 스코프: 함수 스코프를 가집니다. 함수 내에선 어디서든 접근할 수 있습니다.
 - 재선언 및 재할당: 모두 가능합니다.
 - 호이스팅: 선언부는 호이스팅되지만, 초기화는 해당 줄에서 이루어지기 때문에 선언 전에는 `undefined` 가 할당되어 있습니다.

`let`
 - 스코프: 블록 스코프를 가집니다. `{}` 로 감싼 블록 내부에서만 유효합니다.
 - 재선언 및 재할당: 같은 스코프 내에서 재선언은 불가능하지만, 재할당은 가능합니다.
 - 호이스팅 & TDZ: 선언은 호이스팅되지만, **Temporal Dead Zone (TDZ) ** 때문에 선언 전에 접근하면 `ReferenceError` 가 발생합니다.

`const`
- 스코프: `let` 과 동일하게 블록 스코프를 가집니다.
- 재선언 및 재할당: 선언과 동시에 초기화가 필요하며, 이후 재할당과 재선언이 불가능합니다. 다만, **객체나 배열의 경우 참조값은 고정되지만 내부 프로퍼티는 변경할 수 있습니다**
- 호이스팅 & TDZ: `let` 과 마찬가지로 호이스팅되지만 TDZ 가 존재합니다.

<br />

## 2. 호이스팅 (Hoisting) 이란 무엇이며, 변수 선언에 어떤 영향을 미치나요?

호이스팅은 JavaScript 가 변수와 함수 선언을 해당 스코프의 최상단으로 끌어올리는 동작을 의미합니다.

`var`
- 선언은 호이스팅되어 함수 시작 시 이미 선언된 것으로 간주되지만, 할당은 실제 코드 실행 시점에 이루어집니다. 그래서 선언 전에 변수를 사용하면 `undefined` 가 반환됩니다.
```javascript
console.log(a);  // undefined
var a = 10;
```

`let` & `const`
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