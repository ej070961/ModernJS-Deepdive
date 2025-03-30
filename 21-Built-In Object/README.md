# 빌트인 객체

## 자바스크립트 객체의 분류 
> 자바스크립트 객체는 크게 3개의 객체로 분류할 수 있음
- 표준 빌트인 객체
    - ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공
    - 전역 객체의 프로퍼티로서 제공되어, 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있음 
- 호스트 객체
    - ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경(브라우저 환경 또는 Node.js환경)에서 추가로 제공하는 객체 
    - 브라우저 환경에서는 **클라이언트 사이즈 Web API**를 호스트 객체로 제공하고, Node.js환경에서는 Node.js 고유의 API를 호스트 객체로 제공
- 사용자 정의 객체
    - 사용자가 직접 정의한 객체를 뜻함 

## 표준 빌트인 객체
> 자바스크립트는 0bject, String, Number, Boolean, Symbol, Date, Math, RegEXp, Array, Map/Set, Weaknap/Weakset, Function, Promise, Reflect, Proxy, JSON, Error 등 40 여 개의 표준 빌트인 객체를 제공
- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체 
    - String, Number, Boolean, Function, Array, Date 는 생성자 함수로 호출하여 인스턴스를 생성할 수 있음 
- **생성자 함수 객체인 표준 빌트인 객체**는 `프로토타입 메서드`와 `정적 메서드`를 제공하고 **생성자 함수 객체가 아닌 표준 빌트인 객체**는 `정적 메서드`만 제공
    - 예시
    ```Javascript
    // Number 생성자 함수에 의한 Number 객체 생성
    const numObj = new Number(1.5); // Number {1.5}
    // toFixed 는 Number•prototype의 프로토타입 메서드다.
    // Number.prototype. torixed는 소수점 자리를 반올림하여 문자열로 반환한다.
    console.log(numObj.toFixed()); // 2
    // isinteger는 Number 의 정적 메서드다.
    // Number.isInteger는 인수가 정수 (integer)인지 검사하여 그 결과를 Boolean 으로 반환한다.
    console.log(Number.isInteger(0.5)); // false
    ```

## 원시값과 래퍼 객체
- 원시값 → 객체 X
- 원시값에 대해 **마침표 표기법**으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환 
    - 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌림 
- 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 **래퍼 객체**라고 함
- 래퍼 객체는 프로토타입 메서드를 상속받아 사용할 수 있고, 처리가 종료되면 래퍼 객체는 가비지 컬렉션의 대상이 됨 
```Javascript
const str = 'hello';
// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다. 
console. log(str.length); // 5
console.log(str.toUpperCase()); // HELLO

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

## 전역 객체 
- 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체
- 브라우저 환경에서는 window가 전역 객체를 가리키며, Node.js 환경에서는 global이 전역 객체를 가리킴 
- 전역 객체는 **표준 빌트인 객체**와 환경에 따른 **호스트 객체(클라이언트 Web API 또는 Node.js의 호스트 API)**, **var 키워드로 선언한 전역 변수와 전역 함수**를 프로퍼티로 갖음
- 전역 객체의 프로퍼티와 메서드는 전역 객체를 가리키는 식별자(window, global)를 생략하여 참조/호출 가능 

### 빌트인 전역 프로퍼티
- 빌트인 전역 프로퍼티는 **전역 객체의 프로퍼티**를 의미 
- Infinity 프로퍼티
    - 무한대를 나타내는 숫자값 Infinity를 가짐
- NaN 프로퍼티
    - 숫자가 아님(Not a Number)을 나타내는 숫자값 NaN을 가짐 
- undefined 프로퍼티
    - 원시 타입 undefined를 값으로 가짐 

### 빌트인 전역 함수
- eval 함수
    - 자바스크립트 코드를 나타내는 문자열을 인수로 전달받음
    - 전달받은 문자열 코드가 표현식이라면 문자열 코드를 런타임에 평가하여 값을 생성
    - 전달받은 인수가 표현식이 아닌 문이라면 문자열 코드를 런타임에 실행 
    - 인수로 전달받은 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행한 다음 마지막 결과값을 반환
    ➡️ **기존 스코프를 런타임에 동적으로 수정**
    ➡️ **eval 함수를 통해 실행되는 코드는 자바스크립트 엔진에 의해 최적화가 수행되지 않으므로 일반적인 코드 실행에 비해 처리 속도가 느림** 

- isFinite 함수
    - 전달받은 인수가 정상적인 유한수인지 검사하여 true, false 반환
        - 인수를 숫자로 변환했을 때 NaN이면 false 반환
        - 인수가 null이면 true 반환(null을 숫자로 변환하면 0)

- parseFloat 함수
    - 전달받은 문자열 인수를 부동 소수점 숫자(실수)로 해석하여 반환
```Javascript 
/**
*
* 전달받은 문자열 인수를 실수로 해석하여 반환한다.
*@param {string} string - 변환 대상 값
*@returns {number} 변환 결과
*/
parseFloat(string)
```

- parseInt 함수
    - 전달받은 문자열 인수를 정수로 해석하여 반환
```Javascript 
/**
*
* 전달받은 문자열 인수를 정수로 해석하여 반환한다.
* @param {string} string - 변환 대상 값
* @param {number} [radix] - 진법을 나타내는 기수 (2~ 36, 기본값 10)
* @ returns {number} 변환 결과
*/
parseInt(string, radix);
```
- endcodeURI/decodeURI
    - encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩
    - 이스케이프 처리 ?
        - 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것 
    - decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩 
```Javascript 
/**
*
*완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
*@param {string} uri- 완전한 URI
*@returns tstring} 인코딩된 URI
*/
encodeURI(uri)

// 완전한 URI
const uri = 'http://example.com?name=0|&9&job=programmer&teacher';

// encodeURI 함수는 완전한 URI 를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EBXAA%A8&job=programmer&teacher
```
> URI 문법 형식 표준에 따르면 URL은 아스키 문자 셋으로만 구성되어야 하며, 한글을 포함한 대부분의 외국어나 아스키 문자 셋에 정의되지 않은 특수 문자의 경우 URL에 포함될 수 없음 ➡️ **이스케이프 처리 필요!**

- encodeURIComponent/decodeURIComponent
    - encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩
    - 인수로 전달된 문자열을 URI 의 구성요소인 쿼리 스트링의 일부로 간주
    - 따라서 쿼리 스트링 구분자로 사용되는 =, ?, & 까지 인코딩 (encodeURI는 인코딩하지 않음 )

### 암묵적 전역 
- 선언하지 않은 식별자에 값을 할당할 경우 자바스크립트는 전역 객체에 프로퍼티를 동적 생성함으로써, 변수 선언이 아닌 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작하는 것을 뜻함 
- 변수가 아니므로 변수 호이스팅이 발생하지 않음 