## Day4

### Elio

#### ✈️ 내용 정리

##### 서브클래싱

- 부모 클래스 확장하는 자식 클래스
- 상속받아 새로운 객체를 구현
- 메서드 체이닝
  - 오버라이드된 부모 클래스의 메서드 호출
- 생성자 체이닝
  - 부모 클래스의 생성자 호출

##### 믹스인

- 기능의 확장을 위해 사용

- **자바스크립트의 클래스는 표현식 뿐만 아니라 문이기도 함**

- extends 절은 생성자를 반환하는 임의의 표현식 허용

- | 항목               | 믹스인 패턴                          | 인터페이스                                |
  | ------------------ | ------------------------------------ | ----------------------------------------- |
  | **기능 제공 방식** | 메서드와 속성을 객체나 클래스에 병합 | 메서드와 속성의 정의만 제공               |
  | **다중 적용**      | 가능 (Object.assign 등으로 병합)     | 가능 (다중 인터페이스 구현)               |
  | **강제성**         | 강제되지 않음 (런타임 시 적용)       | 강제 (컴파일 타임에 구현 여부 검증)       |
  | **사용 언어**      | JavaScript                           | TypeScript (JavaScript에서는 명시적 불가) |
  | **충돌 가능성**    | 있음 (메서드 이름 중복 시 덮어씌움)  | 없음 (구현은 클래스에서 처리)             |

- 장점
  - 함수 중복을 줄이고 재사용성 높임
- 단점
  - 프로토타입의 오염과 함수 출처에 대한 불확실성

##### 데코레이터

- 코드 재사용의 다른 방법
- 서브클래싱의 다른 방법
- 기존 클래스에 동적으로 기능 추가
- 객체의 생성보다는 기능의 확장에 초점을 맞춤

##### 의사 클래스 데코레이터

- 인터페이스 개념을 사용
- 인터페이스란 객체가 가져야 할 메서드를 정의하는 방법
- 인터페이스는 자바스크립트 단독 사용 불가

##### 추상 데코레이터

- ```js
  class Interface {
    constructor(className, methods) {
      if (!Array.isArray(methods))
        throw new Error("Interface need methods array");
      this.className = className;
      this.requiredMethods = methods;
  
      methods.forEach((method) => {
        this[method] = function () {};
      });
    }
  
    static ensureImplements(classInstance, interfaceToImplement) {
      for (const method of interfaceToImplement.requiredMethods) {
        if (typeof classInstance[method] !== "function") {
          throw new Error("Implements missing");
        }
      }
    }
  }
  
  const MacBook = new Interface("MacBook", [
    "addEngraving",
    "addParallels",
    "add4GBRam",
    "add8GBRam",
    "addCase",
  ]);
  
  class MacBookPro {
    constructor() {}
    addEngraving() {}
    addParallels() {}
    add4GBRam() {}
    add8GBRam() {}
    addCase() {}
    getPrice() {
      return 900.0;
    }
  }
  
  class MacBookDecorator {
    constructor(macbook) {
      Interface.ensureImplements(macbook, MacBook);
      this.macbook = macbook;
    }
  
    addEngraving() {
      return this.macbook.addEngraving();
    }
    addParallels() {
      return this.macbook.addParallels();
    }
    add4GBRam() {
      return this.macbook.add4GBRam();
    }
    add8GBRam() {
      return this.macbook.add8GBRam();
    }
    addCase() {
      return this.macbook.addCase();
    }
  }
  
  class CaseDecorator extends MacBookDecorator {
    constructor(macbook) {
      super(macbook);
    }
  
    addCase() {
      return `${this.macbook.addCase()}Adding case to macbook`;
    }
  
    getPrice() {
      return this.macbook.getPrice() + 45.0;
    }
  }
  
  const myMacBookPro = new MacBookPro();
  console.log(myMacBookPro.getPrice());
  
  const decoratedMacBookPro = new CaseDecorator(myMacBookPro);
  console.log(decoratedMacBookPro.getPrice());
  
  ```

  - 장점
    - 유연하고 투명하게 사용 가능
    - 새로운 기능으로 감싸져 확장되거나 데코레이트될 수 있으며 베이스 객체 변경될 걱정 없음
    - 수많은 서브클래스에 의존할 필요도 없음
  - 단점
    - 네임 스페이스에 작고 비슷한 객체를 추가하기 떄문에 잘 관리하지 않는다면 애플리케이션의 구조를 무척 복잡하게 만들 수도 있음

##### 플라이웨이트 패턴

- 반복되고 느리고 비효율적으로 데이터를 공유하는 코드를 최적화하는 전통적인 구조적 해결 방법

- 연관된 객체끼리 데이터를 공유하게 하면서 애플리케이션의 메모리를 최소화

- 공통으로 사용되는 부분만을 하나의 외부 객체로 내보내는 것으로 이루어짐

- 사용법

  - 데이터 레이어에서 메모리에 저장된 수많은 비슷한 객체 사이로 데이터 공유
  - DOM레이어에도 적용 가능
    - 비슷한 동작을 하는 이벤트 핸들러를 모든 자식 요소보다는 부모 요소에 등록

- 데이터 공유

  - 내재적 상태
    - 내부 메서드에 필요한 것
    - 없으면 동작하지 않음
  - 외재적 상태
    - 제거되어 외부에 저장 가능

- ```js
  // ES6 클래스에서의 prototype 속성은 configurable(구성 가능)은 하지만
  // writable(쓰기 가능)은 아님
  // 이는 클래스를 정의할 때 JavaScript가 내부적으로 prototype 속성을 읽기 전용으로 설정하기 때문
  
  class InterfaceImplementation {
    static implementsFor(superClassOrInterface) {
      // 함수인지 판단은 클래스인지 판단하는 것
      if (superClassOrInterface instanceof Function) {
        // 기존 prototype에 인터페이스의 prototype을 병합
        const interfaceMethods = Object.keys(superClassOrInterface.prototype);
        interfaceMethods.forEach((method) => {
          if (method !== "constructor" && !this.prototype[method]) {
            this.prototype[method] = superClassOrInterface.prototype[method];
          }
        });
        this.prototype.parent = superClassOrInterface;
      } else {
        // 일반 객체의 경우 프로토타입 체인을 설정
        const interfaceMethods = Object.keys(superClassOrInterface);
        interfaceMethods.forEach((method) => {
          if (!this.prototype[method]) {
            this.prototype[method] = superClassOrInterface.prototype[method];
          }
        });
        this.prototype.parent = superClassOrInterface;
      }
  
      return this;
    }
  }
  
  const CoffeeOrder = {
    serveCoffee(context) {
      console.log("you should override");
    },
    getFlavor() {},
  };
  
  class CoffeeFlavor extends InterfaceImplementation {
    constructor(newFlavor) {
      super();
      this.flavor = newFlavor;
    }
  
    getFlavor() {
      return this.flavor;
    }
  
    serveCoffee(context) {
      console.log(
        `Serving Coffee flavor ${this.flavor} to table ${context.getTable()}`
      );
    }
  }
  
  CoffeeFlavor.implementsFor(CoffeeOrder);
  
  const CoffeeOrderContext = (tableNumber) => ({
    getTable() {
      return tableNumber;
    },
  });
  
  class CoffeeFlavorFactory {
    constructor() {
      this.flavors = {};
      this.length = 0;
    }
  
    getCoffeeFlavor(flavorName) {
      let flavor = this.flavors[flavorName];
      if (!flavor) {
        flavor = new CoffeeFlavor(flavorName);
        this.flavors[flavorName] = flavor;
        this.length++;
      }
      return flavor;
    }
  
    getTotalCoffeeFlavorMade() {
      return this.length;
    }
  }
  
  const testFlyweight = () => {
    const flavors = [];
    const tables = [];
    let ordersMade = 0;
    const flavorFactory = new CoffeeFlavorFactory();
  
    function takeOrders(flavorIn, table) {
      flavors.push(flavorFactory.getCoffeeFlavor(flavorIn));
      tables.push(CoffeeOrderContext(table));
      ordersMade++;
    }
  
    takeOrders("Cappuccino", 2);
  
    for (let i = 0; i < ordersMade; i++) {
      flavors[i].serveCoffee(tables[i]);
    }
  };
  
  testFlyweight();
  // Serving Coffee flavor Cappuccino to table 2
  ```

- 도서관 예시

  - 책 객체에 모든 정보를 담게 되면 책이 많아질수록 메모리에 부담이 됨
  - 하나의 객체에 모든 데이터를 저장하지 않고, 내부 상태와 외부 상태로 나눔
  - 외부 상태를 외부 객체에 저장하고 그것을 공유하는 개념

- 중앙 집중식 이벤트 핸들링

  - 개별적으로 관리되었던 많은 동작을 공유된 하나의 동작으로 바꾸어 메모리를 절약할 수 있게 해준다는 점

#### 👀 인사이트

모든 패턴은 충분한 문서화나 공유를 통해 효과적으로 사용 가능.

***

### Dennis

#### ✈️ 내용 정리
서브클래싱
- 부모 클래스의 속성을 상속받아서 새로운 객체를 생성하는 걸 뜻 함
- 믹스인과 데코레이터 패턴 또한 서브 클래싱의 다른 방법

믹스인
- Mixin은 상속 없이 어떤 객체나 클래스에 재사용 가능한 기능을 추가할 수 있는 객체이다.
- 동적 클래스 생성을 위해서 고차 함수와 익명 Class를 통해서 만들 수 있지만 Object.assign으로도 만들 수 있다.
- 다만, 논쟁의 여지가 남아있는 패턴, 프로토타입 오염과 함수의 추처에 대한 불확실성 초래

데코레이터 패턴
- 코드 재사용을 목표로 하는 구조 패턴
- 클래스에 인스턴스를 만들고 인스턴스에 메소드를 추가하는 듯한 패턴

추상 데코레이터 패턴
- 인터페이스와 함께 사용하여 추상 데코레이터 클래스를 생성하고 확장할 클래스를 만든다.
- 베이스 클래스를 인자로 받아서 확장할 클래스를 사용한다.
- 주의할 점은네임 스페이스에 작고 비슷한 객체를 추가하기 때문에, 구조를 복잡하게 만들 수 있다.

플라이웨이트 패턴
- 반복되고 느리고 비효율적으로 데이터를 공유하는 코드를 최적화하는 전통적인 구조적 패턴
```js
// 믹스인
let sayHiMixin = {
  sayHi() {
    alert(`Hello ${this.name}`);
  },
  sayBye() {
    alert(`Bye ${this.name}`);
  }
};

// 사용법:
class User {
  constructor(name) {
    this.name = name;
  }
}

// 메서드 복사
Object.assign(User.prototype, sayHiMixin);

// 이제 User가 인사를 할 수 있습니다.
new User("Dude").sayHi(); // Hello Dude!
```

#### 👀 인사이트

**익명 클래스의 사용 방법**

**믹스인은 왜 사용하는걸까?**
자바스크립트에서는 단일 상속만 가능하기 때문에 단일 상속으로는 해결할 수 없는 다중 상속이 필요할 때 유용하다.
```js
class Animal {
  eat() {
    console.log('Eating...');
  }
}

class Bird extends Animal {
  fly() {
    console.log('Flying...');
  }
}

class Fish extends Animal {
  swim() {
    console.log('Swimming...');
  }
}

// 새면서도 수영할 수 있는 펭귄은? 상속으로는 해결하기 어려움
class Penguin extends Bird, Fish {} // ❌
```
```js

const FlyMixin = {
  fly() {
    console.log('Flying...');
  }
};

const SwimMixin = {
  swim() {
    console.log('Swimming...');
  }
};

class Penguin {
  eat() {
    console.log('Eating...');
  }
}

Object.assign(Penguin.prototype, SwimMixin);

const penguin = new Penguin();
penguin.eat(); // Eating...
penguin.swim(); // Swimming...
```

**인터페이스를 지원하지 않는 자바스크립트에서 인터페이스를 만드는 법**
Constructor 내부에서 미리 생성한 interface 함수를 통해서 validation 한다.
그냥 타입스크립트 사용하자

**자바스크립트의 데코레이터 **
```js
function logDecorator(func) {
  return function (...args) {
    console.log(`Calling ${func.name} with arguments:`, args);
    return func.apply(this, args);
  };
}

class Calculator {
  constructor(value) {
    this.value = value;
  }

  add(num) {
    return this.value + num;
  }
}

// 특정 메서드에 데코레이터 적용
Calculator.prototype.add = logDecorator(Calculator.prototype.add);

const calc = new Calculator(10);
console.log(calc.add(5)); // Calling add with arguments: [5]

```

최신 문법

```js
function log(target, key, descriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args) {
    console.log(`Calling ${key} with arguments:`, args);
    return originalMethod.apply(this, args);
  };

  return descriptor;
}

class Calculator {
  constructor(value) {
    this.value = value;
  }

  @log
  add(num) {
    return this.value + num;
  }
}

const calc = new Calculator(10);
console.log(calc.add(5)); // Calling add with arguments: [5]

```