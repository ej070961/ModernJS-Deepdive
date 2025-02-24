# 🔑 객체 리터럴 (Object Literal)

## 1. 객체(Object)란?  
- 자바스크립트에서 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 **객체**임.  
- 원시 타입의 값, 즉 원시 값은 **변경 불가능한 값**이지만, 객체 타입의 값(객체)은 **변경 가능한 값**임.  
- 객체는 0개 이상의 **프로퍼티(Property)**로 구성된 집합이며, 프로퍼티는 **키(key)**와 **값(value)**으로 구성됨.  
- 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있음.  
  - 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 **메서드(Method)**라고 부름.

> 객체는 프로퍼티와 메서드로 구성된 집합체임.  
> - **프로퍼티**: 객체의 상태(state)를 나타내는 값  
> - **메서드**: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)



## 2. 객체 리터럴에 의한 객체 생성  
- 자바스크립트는 **프로토타입 기반 객체지향 언어**로서 다양한 객체 생성 방법을 지원함:  
  1. 객체 리터럴  
  2. Object 생성자 함수  
  3. 생성자 함수  
  4. `Object.create` 메서드  
  5. 클래스(ES6)  
- **객체 리터럴**은 중괄호(`{...}`) 안에 0개 이상의 프로퍼티를 정의해 객체를 생성함.  
  - 프로퍼티를 하나도 정의하지 않으면 **빈 객체**가 생성됨.  
  - 객체 리터럴은 **값**으로 평가되는 표현식이기 때문에, 일반적으로 중괄호 뒤에 세미콜론을 붙임.  

```javascript
var person = {
  name: 'Lee',
  sayHello: function() {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

var empty = {}; // 빈 객체
```

- 객체 리터럴을 제외한 다른 객체 생성 방식은 모두 **함수**를 사용함.



## 3. 프로퍼티(Property)  
- 프로퍼티는 **키(key)와 값(value)**으로 구성됨.  
- 프로퍼티를 나열할 때는 쉼표(`,`)로 구분함.  
- 프로퍼티 키, 프로퍼티 값으로 사용 가능한 값:  
  - **프로퍼티 키**: 빈 문자열을 포함한 모든 문자열 또는 심벌 값  
    - 식별자 네이밍 규칙을 따르지 않는 이름이라면 반드시 **따옴표**로 감싸야 함.  
    - 문자열/심벌 외의 값을 사용하면 **암묵적 타입 변환**을 통해 문자열이 됨.  
  - **프로퍼티 값**: 자바스크립트에서 사용할 수 있는 **모든 값**  

### 🔧 프로퍼티 키의 동적 생성  
```javascript
var obj = {};
var key = 'hello';

// ES5
obj[key] = 'world';

// ES6
var obj2 = { [key]: 'world' };

// 기존 객체 obj에 age 프로퍼티를 동적으로 추가
obj.age = 20; 
```
- 이미 존재하는 프로퍼티 키를 중복 선언하면, **나중에 선언한 프로퍼티**가 먼저 선언한 프로퍼티를 덮어씀.


## 4. 메서드(Method)  
- 메서드는 **객체에 묶여 있는 함수**를 의미함.  
- 메서드 내부에서 사용하는 `this` 키워드는 해당 **객체 자신**을 가리키는 참조 변수임.



## 5. 프로퍼티 접근  
- 프로퍼티에 접근하는 방법은 2가지임:  
  1. **마침표 표기법**: 마침표 연산자(`.`) 사용  
  2. **대괄호 표기법**: 대괄호 연산자(`[]`) 사용  

```javascript
var person = {
  name: 'Lee'
};

console.log(person.name);     // 마침표 표기법
console.log(person['name']); // 대괄호 표기법
```

- 존재하지 않는 프로퍼티에 접근하면 **`undefined`**가 반환됨.

### ❗️ 주의: 프로퍼티 키에 하이픈(-) 등 문자가 포함될 때
```javascript
var person = {
  'last-name': 'Lee'
};

person.'last-name'; // SyntaxError: Unexpected string

// 브라우저 환경: NaN
// Node.js 환경: ReferenceError: name is not defined
person.last-name;
// 자바스크립트 엔진은 person.last를 먼저 평가 -> undefined
// 이후 name 식별자를 찾는데, 브라우저 환경에서는 name이라는 전역 변수가 암묵적으로 존재해 NaN이 됨
// Node.js 환경에서는 name 식별자가 없으므로 에러 발생
```



## 6. 프로퍼티 삭제  
- `delete` 연산자를 통해 객체의 프로퍼티를 삭제할 수 있음.

```javascript
var person = {
  name: 'Lee'
};

// name 프로퍼티 삭제
delete person.name;
console.log(person); // {}
```



## 7. ES6에서 추가된 객체 리터럴의 확장 기능

### 7.1 프로퍼티 축약 표현  
- 프로퍼티 값이 **동일한 이름**의 변수를 가리킬 때, 키를 생략 가능함.

```javascript
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };
console.log(obj); // { x: 1, y: 2 }
```

### 7.2 계산된 프로퍼티 이름 (Computed Property Name)  
- **문자열** 또는 문자열로 타입 변환 가능한 값으로 평가되는 표현식을 사용해 **프로퍼티 키를 동적**으로 생성하는 것임.  
- 객체 리터럴 내부에서 **계산된 프로퍼티 이름**을 사용할 수 있음.

```javascript
const prefix = 'prop';
let i = 0;

const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj);
// { 'prop-1': 1, 'prop-2': 2, 'prop-3': 3 }
```

### 7.3 메서드 축약 표현  
- 메서드를 정의할 때 `function` 키워드를 생략하는 축약 표현을 사용할 수 있음.

```javascript
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```

