# 대표적인 ES6 문법

## ES6 개요
- ES6(ECMAScript 2015)는 2015년에 도입된 JavaScript의 6번째 표준안  
- JavaScript의 가장 큰 변화 중 하나로 평가되며, ES5(2009) 이후 6년 만의 대규모 업데이트 
  - 이후 ES7, ES8 등의 업데이트 계기가 됨.
- 코드 가독성, 유지보수성, 성능 개선을 목표로 함.  

<br/>
<br/>

## 1️⃣ 변수 선언 방식 (var, let, const)
| 키워드 | 재선언 | 값 변경(재정의) | 특징 |
|--------|------|--------|--------|
| `var` | ✅ 가능 | ✅ 가능 | 함수 스코프, 호이스팅 문제 발생 |
| `let` | ❌ 불가능 | ✅ 가능 | 블록 스코프, 변수 중복 선언 방지 |
| `const` | ❌ 불가능 | ❌ 불가능 | 블록 스코프, 상수 선언 |

    ```javascript
    let y = 10;
    y = 20;  // ✅ 가능
    let y = 30;  // ❌ 오류 발생 (재선언 불가)
    ```
<br/>
<br/>

## 2️⃣ 템플릿 리터럴 (Template Literals)
- 백틱(`)을 사용하여 문자열 생성  
- `${}`로 변수 삽입 가능  
- 줄바꿈 자동 적용  

- 기존 방식
    ```javascript
    var language = "JavaScript";
    console.log("이것은 " + language + "입니다.");
    ```
- 변경된 방식
    ```javascript
    const language = "JavaScript";
    console.log(`이것은 ${language}입니다.`);
    ```

<br/>
<br/>

## 3️⃣ 화살표 함수 (Arrow Function)
- `function` 키워드 없이 `=>`로 함수 작성 가능  
- `this`를 자동으로 바인딩함  

    ```javascript
    const sum = (a, b) => a + b;
    console.log(sum(5, 3)); // 8
    ```

<br/>
<br/>

## 4️⃣ 모듈화 (export / import)
- 코드 분할 및 재사용성을 위한 기능  
- `export`로 함수 및 변수를 내보내고, `import`로 불러옴  

    ```javascript
    // utils.js
    export function greet(name) {
    return `Hello, ${name}!`;
    }

    // main.js
    import { greet } from './utils.js';
    console.log(greet("SSAFY")); // Hello, SSAFY!
    ```

<br/>
<br/>

## 5️⃣ 클래스 (Class)  
- 객체 생성 및 상속을 위한 문법 제공
- `class` 키워드가 도입되어 객체 지향적인 코드를 더 쉽게 작성 가능  
- `constructor`로 초기화, `extends`로 상속 가능  

    ```javascript
    class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    introduce() {
        return `저는 ${this.name}이고, ${this.age}살입니다.`;
    }
    }

    const ssafy = new Person("SSAFY", 25);
    console.log(ssafy.introduce()); 
    // 저는 SSAFY이고, 25살입니다.
    ```

<br/>
<br/>

## 6️⃣ 구조 분해 할당 (Destructuring) 
- 객체나 배열에서 값을 추출하여 변수에 할당

    ```javascript
    const person = { name: "SSAFY", age: 25 };
    const { name, age } = person;
    console.log(name, age); // SSAFY 25
    ```

<br/>
<br/>

## 7️⃣ Rest & Spread 연산자 (`...`)  
- **Rest 연산자** : 여러 개의 값을 배열로 묶음  
- **Spread 연산자** : 배열 또는 객체를 펼쳐서 개별 요소로 만듦.  

    ````javascript
    // Rest
    function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
    }

    // Spread
    const numbers = [1, 2, 3];
    const newNumbers = [...numbers, 4, 5];
    ````

<br/>
<br/>

## 8️⃣ 배열 메서드 (forEach, map, reduce)  
| 메서드 | 설명 |
|--------|------|
| `forEach()` | 배열 요소를 순회하며 함수 실행 |
| `map()` | 모든 요소를 변환하여 새로운 배열 반환 |
| `reduce()` | 모든 요소를 누적하여 하나의 값 반환 |


<br/>
<br/>
<br/>
<br/>
<br/>


## (추가) 📌 Yarn vs. NPM

- 가장 큰 차이점 3가지 : 속도, 보안, 패키지 관리 방식

### **Yarn vs NPM 비교**
  
#### 1. 속도 
- **Yarn**: 병렬 설치 방식을 사용하여 의존성을 빠르게 설치  
- **NPM**: 기존에는 순차적으로 설치했으나, 최신 버전에서 속도 개선  

#### 2. 보안   
- **Yarn**: 패키지 다운로드 시 무결성 검증 기능(`checksums`) 제공  
- **NPM**: 최근 보안 기능이 개선되었으나, Yarn보다 도입이 늦음  

#### 3. 패키지 관리 방식   
- **Yarn**: `yarn.lock` 파일을 사용하여 패키지 버전 고정  
- **NPM**: `package-lock.json` 파일을 사용하지만, 이전 버전에서는 일관성이 부족  

