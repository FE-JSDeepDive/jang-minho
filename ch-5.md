# Chapter 5 - 표현식과 문
<br />

## 값

```javascript
10 + 20;  // 30
```

**값은 표현식이 평가되어 생성된 결과** 를 말한다.  
<br />
모든 값은 데이터 타입을 가지며, 메모리에 2진수, 비트의 나열로 저장된다.  
메모리에 저장된 값은 데이터 타입에 따라 다르게 해석될 수 있다.
<br />



## 리터럴

**리터럴 (literal) 은 사람이 이해할 수 있는 문자 또는 약속된 기호를 이용해 값을 생성하는 표기법을 말한다.**  

```javascript
3
```

위 예제의 3 은 숫자 리터럴이다.  
자바스크립트 엔진은 3 을 저장한다.  
<br />

자바스크립트 엔진은 코드가 실행되는 시점인 런타임에 리터럴을 평가해 값을 생성한다.  
<br />

| 리터럴 | 예시 |
|-------|------|
| 정수 리터럴 | `100` |
| 부동소수점 리터럴 | `10.5` |
| 2 진수 리터럴 | `0b01000001` |
| 8 진수 리터럴 | `0o101` |
| 16 진수 리터럴 | `0x41` |
| 문자열 리터럴 | `"Hello"` |
| 불리언 리터럴 | `true`, `false` |
| `null` 리터럴 | `null` |
| `undefined` 리터럴 | `undefined` |
| 객체 리터럴 | `{ name: 'Lee', address: 'Seoul' }` |
| 배열 리터럴 | `[1, 2, 3]` |
| 함수 리터럴 | `function() {}` |
| 정규 표현식 리터럴 | `/[A-Z]+/g` |

<br />



## 표현식

**표현식 (expression) 은 값으로 평가될 수 있는 문 (statement) 이다.**  
**즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다.**  
<br />

```javascript
var score = 100;
```

위 예제의 100 은 리터럴이다. 리터럴 100 은 자바스크립트 엔진에 의해 평가되어 값을 생성하므로  
리터럴은 그 자체로 표현식이다.  
<br />



## 문

```javascript
var sum = 1 + 2;
```
**문 (statement) 은 프로그램을 구성하는 기본 단위이자 최소 실행 단위다.**  
문은 선언문, 할당문, 조건문, 반복문 등으로 구분할 수 있다.  
<br />


## 세미콜론과 세미콜론 자동 삽입 기능

세미콜론 (;) 은 문의 종료를 나타낸다. 즉, 자바스크립트 엔진은 세미콜론으로 문이 종료한 위치를 파악하고  
순차적로 하나씩 문을 실행한다. 따라서 문을 끝낼 때는 세미콜론을 붙여야 한다.  
하지만 0 개 이상의 문을 중괄호로 묶은 코드 블록 (`{ ... }`) 뒤에는 세미콜론을 붙이지 않는다.  
<br />

자바스크립트 엔진이 소스코드를 해석할 때 문의 끝이라고 예측되는 시점에 세미콜론을 자동으로 붙여주는  
**세미콜론 자동 삽입 기능 - ASI (automatic semicolon insertion)** 이 암묵적으로 수행되기 때문에  
문의 끝에 붙이는 세미콜론은 옵션이다.  
<br />

```javascript
function foo() {
  return
    {}
}

console.log(foo());  // undefined

var bar = function () {}
(function() {})();  // TypeError: (intermediate value)(...) is not a function
```
위의 예제와 같이 세미콜론 자동 삽입 기능의 동작과 개발자의 의도가 일치하지 않는 경우가 간혹 있다.  
다양한 의견이 있지만 아직 자바스크립트 활용이 미숙하다면 세미콜론을 붙이는 것을 강력히 추천한다.  
<br />



## 표현식인 문과 표현식이 아닌 문

문에는 표현식인 문과 표현식이 아닌 문이 있다.  
표현식인 문은 값으로 평가될 수 있는 문이며, 표현식이 아닌 문은 값으로 평가될 수 없는 문을 말한다.  

**표현식인 문과 표현식이 아닌 문을 구별하는 가장 간단하고 명료한 방법은 변수에 할당해 보는 것이다.**  
표현식인 문은 값으로 평가되므로 변수에 할당할 수 있지만, 표현식이 아닌 문은 값으로 평가할 수 없으므로  
변수에 할당하면 에러가 발생한다.  
<br />

```javascript
var foo = var x;  // SyntaxError: Unexpected token var
```

```javascript
var x;
x = 100;
```

```javascript
var foo = x = 100;
console.log(foo);  // 100
```
> 할당문을 값처럼 변수에 할당했다. 즉 `x = 100` 은 `x` 변수에 할당한 값 100 으로 평가된다.  

<br />


### 완료 값 (completion value)

크롬 개발자 도구에서 표현식이 아닌 문을 실행하면 언제나 `undefined` 를 반환한다.  
이를 완료 값이라고 한다.  
표현식의 평가 결과가 아니기 때문에 다른 값과 같이 변수에 할당할 수 없고 참조할 수도 없다.  
<br />
크롬 개발자 도구에서 표현식인 문을 실행하면 언제나 평가된 값을 반환한다.  