# 생성자 함수에 의한 객체 생성

> - 생성자 함수: `new 연산자`와 함계 호출하여 객체(인스턴스)를 생성하는 함수
> - 인스턴스: 생성자 함수에 의해 생성된 객체 

## 1. Object 생성자 함수
- new 연산자와 함께 object 생성자 함수를 호출하면 빈 객체를 생성하여 반환 
    - Object 생성자 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공 
- 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있음
 
 ## 2. 생성자 함수
 ### 객체 리터럴에 의한 객체 생성 방식의 문제점
 - 객체 리터럴에 의한 객체 생성 방식은 **단 하나의 객체만 생성**하며, 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적 
    - 객체는 프로퍼티를 통해 객체 고유의 상태를 표현하고, 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현
    - 따라서 프로퍼티는 객체마다 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적
    - 하지만 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 함 

```Javascript
const circle1 = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }   
}
console.log(circle1.getDiameter()); // 10
const circle2 = {
    radius: 10,
    getDiameter() {
        return 2 * this.radius;
    }
};
console.log(circle2.getDiameter()); //20
```

### 생성자 함수에 의한 객체 생성 방식의 장점 
- 생성자 함수에 의한 객체 생성 방식은 마치 객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용해 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능 
```Javascript
// 생성자 함수
function Circle(radius){
    // 생성자 함수 내부의 this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
}
// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circLe2 = new CircLe(10): // 반지름이 10인 Circle 객체를 생성
console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```
> `this`: 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수로 this가 가리키는 값은 함수 호출 방식에 따라 동적으로 결정
> - 일반 함수로서 호출: this가 가리키는 값은 전역 객체 (브라우저 환경에서는 window, Node.js 환경에서는 global)
> - 메서드로서 호출: 메서드를 호출한 객체(마침표 앞의 객체)
> - 생성자 함수로서 호출: 생성자 함수가 (미래에) 생성할 인스턴스 

### 생성자 함수의 인스턴스 생성 과정
- 생성자 함수의 역할은 **인스턴스를 생성**하는 것과 **생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)**하는 것
<br/>

1️⃣ **인스턴스 생성과 this 바인딩** 
- 암묵적으로 빈 객체가 생성되며, 이 인스턴스는 this에 바인딩됨
- 이 처리는 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행 
> `this 바인딩`: this와 this가 가리킬 객체를 바인딩하는 것 

2️⃣ **인스턴스 초기화**
- 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화
- 이 처리는 개발자가 기술 

3️⃣ **인스턴스 반환**
- 생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환 

```Javascript
function Circle(radius) {
// 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}
// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```
> 여기서 함수를 new 연산자 없이 호출하면 일반 함수로 호출되고, new 연산자와 함께 호출하면 생성자 함수로서 호출되는 구조 

### constructor와 non-constructor의 구분
- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: 메서드, 화살표 함수 
> new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 함 

