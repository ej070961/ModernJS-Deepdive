# this
## this 키워드 
- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**
- this를 통해 **자신이 속한 객체** 또는 **자신이 생성할 인스턴스의 프로퍼티나 메서드**를 참조할 수 있음 
- this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있음
- 단, this 바인딩(this와 this가 가리킬 객체를 바인딩하는 것)은 **함수 호출 방식** 에 의해 동적으로 결정 ❗️

## 함수 호출 방식과 this 바인딩
### 일반 함수 호출
- 기본적으로 this에는 전역 객체가 바인딩됨
- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로, 객체를 생성하지 않는 일반 함수에서 this는 의미가 없음
- 따라서 strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩됨

### 메서드 호출
- 객체 리터럴의 메서드 내부에서의 this는 **메서드를 호출한 객체**를 가리킴 
```Javascript
// 객체 리터럴
const circle = {
    radius: 5,
    getDiameter() {
        // this 는 메서드를 호출한 객체를 가리킨다.
        return 2 * this.radius; 
    }

}
console.log(circle.getDiameter()); // 10

```
### 생성자 함수 호출 
- 생성자 함수 내부의 this는 **생성자 함수가 생성할 인스턴스**를 가리킴 
```Javascript
// 생성자 함수
function Circle(radius) {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
}
Circle.prototype.getDiameter = function (){
    // this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius;
};
// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- apply, call, bind 메서드는 Function.prototype의 메서드로 모든 함수가 상속받아 사용할 수 있음
- `Function.prototype.apply`, `Function.prototype.call` 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출
    - 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩

```Javascript
/**
* 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
*@param thisArg - this 로 사용할 객체
*@param argsArray - 수에게 전달할 인수 리스트의 배열 또는 유사 배열 액체
*@returns 호출된 함수의 반환값
*/
Function.prototype.apply(thisArg[, argsArray])

/**
* 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
*@param thisArg- this 로 사용할 객체
*@param arg1, arg2, ... - 함수에게 전달할 인수 리스트
*@returns 호출된 함수의 반환값
*/
Function.prototype.call (thisArg[, arg1[, arg2[, ... ]]])

function getThisBinding()) i
return this;
}
// this로 사용할 객체
const thisArg = { a : 1 };
console.log(getThisBinding()); // window
1/ getThissinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // fa: 1}
```