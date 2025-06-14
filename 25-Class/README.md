# 클래스
## 클래스란 무엇인가?
- 자바스크립트의 클래스란 특정한 객체를 만들기 위한 일종의 템플릿과 같은 개념
- 자바스크립트에서 클래스를 활용하여 객체를 만든는데 필요한 데이터나 이를 조작하는 코드를 추상화해 객체 생성을 더욱 편리하게 할 수 있음
```Javascript
class Car {
    //constructor는 생성자
    constructor(name){
        this.name = name
    }
    //메서드 
    honk(){
        console.log(`${this.name}이 경적을 울립니다!`)
    }

    //정적 메서드
    static hello(){
        console.log('저는 자동차입니다');
    }

    //setter
    set age(value){
        this.carAge = value
    }

    //getter
    get age(){
        return this.carAge
    }
}
```

### constructor
- constructor는 생성자로 객체를 생성하는 데 사용하는 특수한 메서드
- 클래스 당 단 하나만 존재할 수 있으며, 생성자에서 별다르게 수행할 작업이 없다면 생략하는 것도 가능함 

### 프로퍼티
- 프로퍼티란 클래스로 인스턴스를 생성할 때 내부에 정의할 수 있는 속성값을 의미 
- 기본적으로 인스턴스 생성 시 constructor 내부에는 빈 객체가 할당돼 있는데 바로 이 빈 객체에 프로퍼티의 키와 값을 넣어서 활용할 수 있게 도와줌 
```Javascript
class Car {
     constructor(name){
        //값을 받으면 내부에 프로퍼티로 할당됨 
        this.name = name
    }
}
```

### getter와 setter
- getter란 클래스에서 무언가 값을 가져올 때 사용, getter를 사용하기 위해서는 get을 앞에 붙여야 하고, 뒤이어서 getter의 이름을 선언해야 함 
- setter는 클래스 필드에 값을 할당할 때 사용. 마찬가지로 set이라는 키워드를 먼저 선언하고, 뒤이어서 이름을 붙이면 됨 

### 인스턴스 메서드
- 클래스 내부에서 선언한 메서드
- 자바스크립트의 prototype에 선언되므로 프로토타입 메서드로 불리기도 함 
    - **프로토타입 체이닝(직접 객체에서 선언하지 않았음에도 프로토타입에 있는 메서드를 찾아서 실행을 도와주는 것) 특성** 때문에 생성한 객체 myCar에서 hello() 메서드를 호출 가능 
```javascript
class Car{
    constructor(name){
        //값을 받으면 내부에 프로퍼티로 할당됨 
        this.name = name
    }
    //인스턴스 메서드 정의
    hello(){
        console.log(`안녕하세요, ${this.name}입니다.`);
    }
}

const myCar = new Car('자동차');

myCar.hello() //안녕하세요, 자동차입니다. 
```

### 정적 메서드
- 클래스의 인스턴스가 아닌 이름으로 호출할 수 있는 메서드 
- 정적 메서드 내부의 this는 클래스로 생성된 인스턴스가 아닌 클래스 자신을 가리키므로 다른 메서드에서 일반적으로 사용하는 this를 사용할 수 없음 
- 정적 메서드는 인스턴스를 생성하지 않아도 사용할 수 있다는 점에서 객체를 생성하지 않더라도 여러 곳에서 재사용이 가능하다는 장점이 있음 
    - 애플리케이션 전역에서 사용하는 유틸 함수를 정적 메서드로 많이 활용 
```javascript
class Car{
    //인스턴스 메서드 정의
    static hello(){
        console.log(`안녕하세요!`);
    }
}

const myCar = new Car('자동차');

myCar.hello() //Uncaugt TypeError: myCar.hello is not a function
Car.hello() //안녕하세요! 
```

### 상속
- 기존 클래스를 상속받아 자식 클래스에서 상속받은 클래스를 기반으로 확장하는 개념
- extends 키워드를 활용해 기본 클래스를 기반으로 다양하게 파생된 클래스를 생성

## 클래스와 함수의 관계
- 클래스는 ES6에서 나온 개념으로 클래스가 작동하는 방식은 자바스크립트의 프로토타입을 활용한다고 볼 수 있음 

