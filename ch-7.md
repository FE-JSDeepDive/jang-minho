# Chapter 7 - 연산자
<br />

### 문자열 연결 연산자
**+ 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다.**  

```javascript
'1' + 2;  // 12
1 + '2';  // 12

1 + 2;  // 3

1 + true;  // 2
1 + false;  // 1

1 + null;  // 1

+undefined;  // NaN
1 + undefined;  // NaN
```
자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다.  
이를 **암묵적 타입 변환 (Implicit coercion)** 또는 **타입 강제 변환 (type coercion)** 이라고 한다.  

<br />



### 동등 / 일치 비교 연산자

> **동등 비교(==) 연산자는 좌항과 우항의 피연산자를 비교할 때**
> **먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교한다.**

```javascript
'0' == '';  // false
0 == '';  // true
0 == '0';  // true
false == 'false';  // false
false == '0';  // true
false == null;  // false
false == undefined;  // false
```

<br />

> **일치 비교(===) 연산자는 좌항과 우항의 피연산자가**
> **타입도 같고 값도 같은 경우에 한하여 true 를 반환한다.**

```javascript
5 === 5;  // true
5 === '5';  // false
```

다만 NaN 은 자신과 일치하지 않는 유일한 값이다.  
```javascript
NaN === NaN;  // false
```

따라서 숫자가 NaN 인지 조사하려면 `Number.isNaN` 을 사용하면 된다.  
```javascript
Number.isNaN(NaN);  // true
Number.isNaN(10);  // false
Number.isNaN(1 + undefined);  // true
```

숫자 0도 주의하자.
```js
0 === -0;  // true
0 == -0;  // true
```

<br />

> [!Note]  
> ES6 에서 도입된 `Object.is` 메서드는 정확한 비교 결과를 반환한다.
> ```javascript
> -0 === +0;  // true
> Object.is(-0, +0);  // false
> 
> NaN === NaN;  // false
> Object.is(NaN, NaN);  // true
> ```

<br />

### 드 모르간의 법칙
논리 연산자로 구성된 복잡한 표현식은 가독성이 좋지 않아 한눈에 이해하기 어려울 때가 있다.  
드 모르간의 법칙을 활용하여 복잡한 표현식을 좀 더 가독성 좋은 표현식으로 변환할 수 있다.
```javascript
!(x || y) === (!x && !y);
!(x && y) === (!x || !y);
```

<br />



### typeof 연산자
typeof 연산자는 피연산자의 데이터 타입을 문자열로 변환한다.  
string, number, boolean, undefined, symbol, object, function 을 반환한다.  

```javascript
typeof ''  // string
typeof 1  // number
typeof NaN  // number
typeof true  // boolean
typeof undefined  // undefined
typeof Symbol()  // symbol
typeof null  // object
typeof []  // object
typeof {}  // object
typeof new Date()  // object
typeof /test/gi  // object
typeof function () {}  // function 
```
위처럼 typeof 연산자가 반환하는 문자열은 7개의 데이터 타입과 정확히 일치하지는 않는다.  
> **null 값을 연산해보면 null 이 아닌 object 를 반환한다.**

> [!Warning]
> 선언되지 않은 식별자를 typeof 연산자로 연산해보면 undefined 를 반환한다.
> ```javascript
> typeof undeclared;  // undefined
> ```


<br /> <br /> <br />

> 이번 주제는 기초적인 내용들이 너무 많아 주의해야 할 부분만 적었습니다.
