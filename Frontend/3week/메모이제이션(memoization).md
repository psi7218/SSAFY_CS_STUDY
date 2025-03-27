# JavaScript의 메모이제이션(memoization)

### 📌 메모이제이션이란?
- 계산 결과를 저장해, 동일한 입력이 들어올 경우 재계산 없이 저장된 값을 반환
- 성능 최적화를 위해 많이 사용

<br/>
<br/>

### 1️⃣ 메모이제이션 구현 방식
#### 1. 암시적 캐싱 (내부 구현)

```js
function memoizeAdd() {
  const cache = {};

  return function memoizedAdd(a, b) {
    const key = `${a},${b}`;

    if (key in cache) {
      return cache[key];
    } else {
      const result = a + b;

      cache[key] = result;

      return result;
    }
  };
}

const memoizedAdd = memoizeAdd();

console.log(memoizedAdd(3, 4)); // 결과를 계산하고 캐시합니다.
console.log(memoizedAdd(3, 4)); // 캐시에서 결과를 검색합니다.
console.log(memoizedAdd(5, 6)); // 새 결과를 계산하고 캐시합니다.
console.log(memoizedAdd(3, 4)); // 여전히 캐시에서 결과를 검색합니다.
```

- key 조합을 문자열로 저장해 중복 계산 방지
  - 각 항목은 함수의 입력을 키로, 출력을 값으로 사용

<br/>

#### 2. 데코레이터 함수 (외부 함수로 wrapping)
```js
function memoize(fn) {
  const cache = new Map();

  return function (...args) {
    const key = JSON.stringify(args);

    if (cache.has(key)) {
      return cache.get(key);
    }

    const result = fn.apply(this, args);
    cache.set(key, result);

    return result;
  };
}

function add(a, b) {
  return a + b;
}

const memoizedAdd = memoize(add);

console.log(memoizedAdd(3, 4)); // 결과를 계산하고 캐시합니다.
console.log(memoizedAdd(3, 4)); // 캐시 된 결과를 반환합니다.
console.log(memoizedAdd(5, 6)); // 새 결과를 계산하여 캐시합니다.
console.log(memoizedAdd(3, 4)); // 여전히 캐시에서 결과를 검색합니다.
```

- `fn`: 메모이제이션하고 싶은 원본 함수
    - 이 함수는 fn의 결과를 캐싱할 수 있는 새로운 함수를 반환
- `Map`을 사용하는 이유: 일반 객체보다 더 다양한 타입(문자열, 숫자, 함수 등)을 키로 사용할 수 있음
- 함수 로직을 수정하지 않고 메모이제이션 기능 부여 가능

<br/>
<br/>

### 2️⃣ 재귀 함수와 메모이제이션: 피보나치 수열

#### 1. 비효율적인 기존 재귀
```js
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```
- 중복 호출로 성능 저하 심함

<br/>

#### 2. 메모이제이션 적용
```js
function fibonacci(n) {
  if (n <= 1) return n;
  return memoizedFibonacci(n - 1) + memoizedFibonacci(n - 2);
}
const memoizedFibonacci = memoize(fibonacci);
```
- 중복 호출 제거, 속도 대폭 개선

<br/>
<br/>

### 3️⃣ 캐시 관리
- `.clear()` 메서드 구현으로 메모리 관리 및 테스트 용이
    ```js
    memoizedFunction.clear = function() {
    cache.clear();
    };
    ```

- **메모리 관리**: 캐시를 수동으로 제거할 수 있어 리소스를 확보할 수 있습니다.
- **데이터 최신성**: 오래되거나 잘못된 데이터를 제거하여 캐시 된 결과가 정확하게 유지되도록 합니다.
- **캐시 동작 제어**: 데이터 처리에 영향을 미치는 이벤트나 조건에 대응하여 캐시를 재설정할 수 있는 기능을 제공합니다.
- **테스트 및 디버깅**: 캐시 된 데이터의 간섭 없이 알려진 상태에서 함수를 작동할 수 있어 안정적인 테스트에 중요합니다.
- **성능 최적화**: 주기적인 재설정을 통해 크기와 조회 비용을 관리하여 캐시 효율성을 유지합니다.

<br/>
<br/>

### 4️⃣ 사용 사례
- 재귀 알고리즘 (피보나치 등)
- 비싼 API 요청 캐시
- 데이터 변환 결과 저장
- React 컴포넌트 렌더링 최적화 (React.memo, useMemo 등)

<br/>
<br/>

### 5️⃣ 메모이제이션 단점 및 주의점
| 항목 | 설명 |
|------|------|
| 메모리 사용 | 캐시가 많아지면 메모리 부담 증가 |
| 코드 복잡도 | 캐시 무효화 등 추가 관리 필요 |
| 순수 함수만 적용 | 부작용(side effect)이 있는 함수는 사용 불가 |
| 동시성 문제 | 멀티스레드 환경에서는 주의 필요 |

<br/>
<br/>
<br/>


- 참고 링크
  - 원문 : https://janhesters.com/blog/what-is-memoization-in-javascript-typescript
  - 유튜브 링크 : https://youtu.be/KwXWI8pm0Vk