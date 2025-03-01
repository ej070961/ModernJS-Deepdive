# 🔗 프로토타입 (Prototype)

> 자바스크립트는 **프로토타입 기반의 객체지향 프로그래밍 언어**로 원시 타입 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 **객체**임.

---

## 1. 객체지향 프로그래밍  
- 프로그램을 명령어나 함수의 목록으로 보는 전통적인 **명령형 프로그래밍**의 **절차지향적 관점**에서 벗어나, 객체의 집합으로 프로그램을 표현하려는 **프로그래밍 패러다임**임.  
  - **객체**: 속성을 통해 여러 값을 하나의 단위로 구성한 **복합적인 자료구조**.  
- 객체지향 프로그래밍에서는 객체의 **상태(프로퍼티)**와 그 상태를 조작하는 **동작(메서드)**를 논리적으로 하나로 묶어 생각함.  
- 각 객체는 자기만의 기능을 수행하면서 다른 객체와 **관계**를 가질 수 있음.

---

## 2. 상속과 프로토타입  
- **상속(Inheritance)**: 어떤 객체의 프로퍼티나 메서드를 다른 객체가 **그대로** 물려받아 사용할 수 있는 것.  
- 자바스크립트는 **프로토타입**을 기반으로 상속을 구현하여, **불필요한 중복**을 제거함.

### 🏷️ 상속이 없을 경우, 기존 문제점
```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function() {
    return Math.PI * this.radius * 2;
  };
}

// 반지름이 1인 인스턴스
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스
const circle2 = new Circle(2);

// 인스턴스마다 동일 동작의 메서드가 중복 생성됨
console.log(circle1.getArea === circle2.getArea); // false
console.log(circle1.getArea());  // 3.141592653589793
console.log(circle2.getArea());  // 12.566370614359172
```
- 인스턴스를 생성할 때마다 **`getArea` 메서드를 중복** 생성→ 메모리 낭비 & 퍼포먼스 악영향.

### ✅ 상속을 통한 중복 제거
```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// 모든 인스턴스가 프로토타입의 getArea 메서드를 공유
Circle.prototype.getArea = function() {
  return Math.PI * this.radius * 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true
console.log(circle1.getArea());  // 3.141592653589793
console.log(circle2.getArea());  // 12.566370614359172
```
- **Circle.prototype**을 모든 인스턴스가 **상속**받아 사용하므로, 하나의 메서드를 공유해 **중복**을 없앰.

---

## 3. 프로토타입 객체  
> 프로토타입 객체는 객체 간 **상속을 구현**하기 위해 사용.

- 프로토타입은 어떤 객체의 **상위(부모) 객체**로서 다른 객체에 **공유 프로퍼티**를 제공함.  
- 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 **자신의 프로퍼티처럼** 자유롭게 사용 가능.  
- 모든 객체는 내부 슬롯 `[[Prototype]]`을 가지며, 이 **값**은 “프로토타입”의 참조를 담고 있음.  
  - ex) **객체 리터럴**로 생성된 객체 → `Object.prototype`이 프로토타입이 됨.

> 모든 객체는 하나의 프로토타입을 가지며, 모든 프로토타입은 **생성자 함수**와 연결되어 있음. → 객체와 프로토타입, 생성자 함수는 서로 밀접히 연결되어 존재함.

---

### `__proto__` 접근자 프로퍼티
- 모든 객체는 `obj.__proto__`를 통해 자신의 프로토타입(`[[Prototype]]`)에 **간접적으로** 접근 가능. (ES6 이후 일부 권장 X)  
- **접근자 프로퍼티**(`[[Get]]`, `[[Set]]`) 역할을 수행하며, 상호 참조 방지 등을 위해 자바스크립트 엔진 내부적으로 제약을 두고 있음.  
- 프로퍼티 검색 시, 객체에 해당 프로퍼티가 없으면 `__proto__`가 가리키는 **부모 프로토타입**을 따라 **체인**을 거슬러 올라감.

### 함수 객체의 `prototype` 프로퍼티
- **함수 객체만**이 소유하는 `prototype` 프로퍼티는 **생성자 함수**가 만들 인스턴스의 프로토타입을 가리킴.  
- 비생성자(non-constructor)인 **화살표 함수**나 **메서드 축약 표현**은 `prototype` 프로퍼티를 소유하지 않음.

> 모든 객체가 갖는 `__proto__` 접근자 프로퍼티와, **함수 객체**만이 갖는 `prototype` 프로퍼티는 **동일한 프로토타입 객체**를 가리키지만, **사용 주체**가 다름.

| 구분                                | 소유 주체     | 값                   | 사용 주체        | 사용 목적                                                                        |
|-------------------------------------|---------------|----------------------|------------------|------------------------------------------------------------------------------------|
| `__proto__` 접근자 프로퍼티         | 모든 객체     | 프로토타입의 참조   | 객체 자신        | 객체가 자신의 프로토타입에 접근하거나 교체할 때 사용                               |
| `prototype` 프로퍼티                | 생성자 함수   | 프로토타입의 참조   | 생성자 함수 자체 | 생성자 함수가 **인스턴스**의 프로토타입을 할당하기 위해 사용                      |

---

### 프로토타입의 `constructor` 프로퍼티와 생성자 함수
- 모든 프로토타입은 `constructor` 프로퍼티를 가지며, 이는 **자신(프로토타입)과 연결된 생성자 함수를 가리킴**.  
```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

console.log(me.constructor === Person); // true
```
- `me` 객체는 직접 `constructor` 프로퍼티를 갖고 있지 않지만, `me.__proto__`(즉, `Person.prototype`)의 `constructor` 프로퍼티를 통해 `Person`을 참조함.

---

## 4. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- **리터럴 표기법**으로 생성된 객체의 프로토타입의 `constructor` 프로퍼티가 **진짜 객체를 생성한 함수**와 항상 일치한다고 단정할 수는 없음.  
- 각 리터럴 표기법의 생성자 함수 및 프로토타입은 다음과 같음:

| 리터럴 표기법    | 생성자 함수 | 프로토타입         |
|------------------|-------------|--------------------|
| **객체 리터럴**  | `Object`    | `Object.prototype` |
| **함수 리터럴**  | `Function`  | `Function.prototype` |
| **배열 리터럴**  | `Array`     | `Array.prototype`  |
| **정규 표현식**  | `RegExp`    | `RegExp.prototype` |

> 결국 리터럴 표기법으로 생성된 객체도 **생성자 함수**와 연결되어 있고, 그 **프로토타입**을 상속받게 됨.

---

## 5. 프로토타입의 생성 시점
- **프로토타입**은 **생성자 함수**가 생성되는 시점에 함께 생성됨.  
- 프로토타입과 생성자 함수는 **단독**으로 존재할 수 없으며 **항상 쌍**으로 존재.

### 사용자 정의 생성자 함수와 프로토타입 생성 시점
- **constructor**(생성자 함수)로서 호출 가능한 함수는 평가(함수 객체 생성)되는 순간, 프로토타입도 함께 생성됨.
```javascript
console.log(Person.prototype); // { constructor: f }

function Person(name) {
  this.name = name;
}
```
- **non-constructor**(예: 화살표 함수, 메서드 축약)은 **프로토타입이 생성되지 않음**.

### 빌트인 생성자 함수와 프로토타입 생성 시점
- `Object`, `String`, `Number`, `Function`, `Array`, `RegExp`, `Date`, `Promise` 등 **빌트인 생성자 함수**도 전역 객체가 **생성**될 때 함께 프로토타입이 미리 준비됨.

> 객체가 생성되기 **이전**, 생성자 함수와 프로토타입은 이미 존재. 이후 객체를 생성할 때, 그 객체의 `[[Prototype]]`에 해당 프로토타입이 연결됨.

---

## 6. 객체 생성 방식과 프로토타입 결정
- 객체는 다양한 방식으로 생성되지만, 내부적으로 **추상 연산** `OrdinaryObjectCreate`를 통해 생성된다는 공통점이 있음.  
- `OrdinaryObjectCreate`가 어떤 프로토타입을 할당할지는 **객체 생성 방식**이 결정함.

### 객체 리터럴로 생성된 객체의 프로토타입
> **Object.prototype**이 프로토타입이 됨.
```javascript
const obj = { x: 1 };
// obj -> Object.prototype 상속
console.log(obj.constructor === Object);        // true
console.log(obj.hasOwnProperty('x'));          // true
```

### `Object` 생성자 함수로 생성된 객체의 프로토타입
> **Object.prototype**이 프로토타입이 됨.
```javascript
const obj = new Object();
obj.x = 1;
console.log(obj.constructor === Object);        // true
console.log(obj.hasOwnProperty('x'));          // true
```

### 생성자 함수로 생성된 객체의 프로토타입
> **생성자 함수의** `prototype` 프로퍼티가 가리키는 객체가 그 인스턴스의 프로토타입이 됨.
```javascript
function Circle(radius) { 
  this.radius = radius; 
}
const circle = new Circle(2);
// circle -> Circle.prototype
```

---

## 7. 프로토타입 체인
- 자바스크립트에서 객체지향 상속을 구현하는 **메커니즘**.  
- 객체의 프로퍼티 검색 시, 그 객체에 해당 프로퍼티가 없으면 `[[Prototype]]`을 따라 **부모 프로토타입**에서 찾고, 없으면 그 위를 찾아감.  
- **체인의 종점**은 언제나 `Object.prototype`이며, 여기서도 찾을 수 없으면 `undefined`를 반환(오류 X).

> 즉, **프로토타입 체인**은 상속 & 프로퍼티 검색을 위한 연결 고리임.

---

## 8. `instanceof` 연산자
- **`객체 instanceof 생성자함수`**  
- 우변 **생성자 함수**의 `prototype` 객체가 좌변 **객체**의 프로토타입 체인상에 존재하면 `true`, 그렇지 않으면 `false`.

---

## 9. 정적 프로퍼티/메서드
- **정적 프로퍼티/메서드**는 **인스턴스를 생성하지 않아도** 호출할 수 있는 프로퍼티/메서드를 말함.  
  - 예: `Math.random()`, `Array.isArray()` 등.  
- 인스턴스가 상속받는 프로토타입 체인에는 포함되지 않으므로, 인스턴스로는 호출할 수 없음.

---

## 10. 프로퍼티 존재 확인
```javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};
```
### (1) `in` 연산자
```javascript
console.log('name' in person); // true
```
- 객체 자신의 프로퍼티뿐 아니라, **프로토타입 체인 상의 프로퍼티**까지 확인함.

### (2) `Object.prototype.hasOwnProperty()`
```javascript
console.log(person.hasOwnProperty('name')); // true
```
- **자신**의 고유 프로퍼티인지 여부만 확인.

---

## 11. 프로퍼티 열거

### (1) `for...in` 문
- 객체 자신의 **열거 가능한(enumerable)** 프로퍼티뿐 아니라 **프로토타입 체인** 상의 프로퍼티도 순회함.  
- 상속 프로퍼티를 제외하고 싶다면 `hasOwnProperty` 검사 필요.

```javascript
const person = {
  name: 'Lee',
  address: 'Seoul',
  __proto__: { age: 20 }
};

for (const key in person) {
  if (!person.hasOwnProperty(key)) continue;
  console.log(key, person[key]);
}
// name Lee
// address Seoul
```

### (2) `Object.keys / Object.values / Object.entries`
- 객체 **자신의** 열거 가능한 프로퍼티만 빠르게 얻고 싶을 때 사용:  
  - `Object.keys(obj)` → 키 배열  
  - `Object.values(obj)` → 값 배열  
  - `Object.entries(obj)` → `[키, 값]` 쌍 배열

