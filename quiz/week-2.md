### 1. null 을 처리할 때 성능을 위해서 다음 중 어떤 구문을 사용해야 하는지 설명해주세요

```javascript
(1) : if (nullValue) {
  // ...
}

(2) : if (nullValue !== null) {
  // ...
}
```

> (2) 을 사용해야 합니다  
> V8 엔진에서 Falsy 를 처리하기 위해서는 값을 Falsy 로 처리하고, 그 다음 비교해야 하지만,  
> !== null 을 사용한다면 비교만 하면 되기 때문에 더 효율적입니다  

<br>

---

### 2. 배열과 객체의 자원 소모와 메모리 효율을 비교하고, 아래와 같은 케이스에 오버헤드를 비교하세요
```javascript
const arrayTest = [];
const objectTest = {
  name: 'John Doe',
};

arrayTest['name'] = 'John Doe';
```

> **배열**  
> `[]` 을 생성하면서 대략적으로 16~32 bytes 의 메모리가 소비됩니다  
> 인덱스를 기반으로 정렬된 메모리에서 직접 접근할 수 있어서 더 빠릅니다  
> 대부분의 JavaScript 엔진은 배열을 **Packed Array** 로 최적화하여 더 빠르게 접근합니다
>   
> **객체**  
> `{}` 을 생성하면서 대략적으로 32~64 bytes 의 메모리가 소비됩니다  
> 해시 테이블을 사용하여 키-값 매핑을 하므로 **메모리 오버헤드** 가 있습니다  
> 하지만 배열에 숫자가 아닌 키를 추가하면 JavaScript 엔진이   
> **배열을 일반 객체처럼 취급하게 되어** 최적화가 깨집니다  

<br>

---

### 3. 배열에 함수를 저장해 순회하는 것이 왜 체이닝을 통해 함수를 순회하는 것보다 느린지 설명해주세요

> 배열을 순회하며 함수를 실행할 때 각 호출마다 Context 가 쌓여 성능이 저하되고,  
> 상태를 갱신하는 과정에서 함수 호출 오버헤드가 체이닝보다 증가합니다  
> 다만 명시적이고 이해하기 쉬우며, 동적으로 조작 가능하고, 개별 함수 실행 확인이 쉽습니다  

**배열 순회**
```javascript
const handlers = [
  (x) => x + 1,
  (x) => x * 2,
  (x) => x - 3
];

let value;
handlers.forEach(fn => {
  value = fn(value);
});
```


**함수 체이닝**
```javascript
const pipe = (...fn) => (x) => fn.reduce((acc, fn) => fn(acc), x);

const transform = pipe(
  (x) => x + 1,
  (x) => x * 2,
  (x) => x - 3
);
```
> 성능을 더 높이고 싶다면 reduce 대신 재귀를 사용할 수 있습니다
