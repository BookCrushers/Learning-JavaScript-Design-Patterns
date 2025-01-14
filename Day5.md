## Day4

### Elio

#### ✈️ 내용 정리

#### 행위 패턴

- 관찰자 패턴
- 중재자 패턴
- 커맨드 패턴

#### 관찰자 패턴

- 한 객체가 변경될 때 다른 객체들에게 변경되었음을 알릴 수 있게 해주는 패턴

- 변경된 객체는 누가 자신을 구독하는지 알 필요 없이 알림을 보낼 수 있음

- 한 객체를 관찰하는 여러 객체들이 존재하며, 주체의 상태가 변화하면 관찰자들에게 자동으로 알림을 보냄

- 구성 요소
  - 주체
    - 관찰자 리스트 관리
    - 추가와 삭제를 가능케함
  - 관찰자
    - 주체의 상태 변화 알림을 감지하는 update 인터페이스 제공
  - 구체적 주체
    - 상태 변화에 대한 알림을 모든 관찰자에게 전달
    - ConcreteObserver의 상태를 저장
  - 구체적 관찰자
    - concreteSubject의 참조를 저장
    - 관찰자의 update 인터페이스를 구현하여 주체의 상태 변화와 관찰자의 상태 변화 일치하도록 함

  - 관찰자 패턴 예시

    - ```js
      class ObserverList {
        constructor() {
          this.observerList = [];
        }
      
        add(obj) {
          return this.observerList.push(obj);
        }
      
        count() {
          return this.observerList.length;
        }
      
        get(index) {
          if (index > -1 && index < this.observerList.length) {
            return this.observerList[index];
          }
        }
      
        indexOf(obj, startIndex) {
          let i = startIndex;
      
          while (i < this.observerList.length) {
            if (this.observerList[i] === obj) {
              return i;
            }
            i++;
          }
          return -1;
        }
      
        removeAt(index) {
          this.observerList.splice(index, 1);
        }
      }
      
      class Subject {
        constructor() {
          this.observers = new ObserverList();
        }
      
        addObserver(observer) {
          this.observers.add(observer);
        }
      
        removeObserver(observer) {
          const index = this.observers.indexOf(observer, 0);
          this.observers.removeAt(index);
        }
      
        notify(context) {
          const observerCount = this.observers.count();
          for (let i = 0; i < observerCount; i++) {
            this.observers.get(i).update(context);
          }
        }
      }
      
      class Observer {
        constructor() {}
      
        update() {}
      }
      
      class ConcreteObserver extends Observer {
        constructor(element) {
          super();
          this.element = element;
        }
        update(checked) {
          this.element.checked = checked;
        }
      }
      
      class ConcreteSubject extends Subject {
        constructor(element) {
          super();
          this.element = element;
        }
      }
      
      const addObserverButton = document.querySelector("#addNewObserver");
      const observersContainer = document.querySelector("#observersContainer");
      const mainCheckbox = document.querySelector("#mainCheckbox");
      
      const concreteSubject = new ConcreteSubject(mainCheckbox);
      
      mainCheckbox.addEventListener("input", (e) => {
        concreteSubject.notify(e.target.checked);
      });
      
      addObserverButton.addEventListener("click", () => {
        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        observersContainer.append(checkbox);
      
        const observer = new ConcreteObserver(checkbox);
        concreteSubject.addObserver(observer);
      });
      
      ```

- 관찰자 패턴과 발행/구독 패턴의 차이점

  - 실제 자바스크립트 환경에서는 발행/구독 패턴이라는 변형된 패턴이 널리 사용됨
  - 관찰자 패턴에서는 이벤트에 대한 알림 받기를 원하는 관찰자 객체가 이벤트를 발생시키는 주체 객체에 알림 대상으로 등록되어야 함
  - 발행/구독 패턴에서는 이벤트 알림을 원하는 구독자와 이벤트를 발생시키는 발행자 사이에 토픽/이벤트 채널을 둠
  - 구독자에게 필요한 값이 포함된 커스텀 인자를 전달할 수 있음
  - 핵심은 발행자와 구독자가 독립적
  - 시스템 구성 요소들간의 느슨한 결합을 도모

- 발행/구독 패턴 장점

  - 애플리케이션의 결합도를 낮춤
  - 코드의 관리와 재사용성을 높임

- 단점

  - 결합도가 낮아지는 만큼 추적이 어려움

- 발행/구독 패턴 구현하기

  - ```js
    class PubSub {
      constructor() {
        this.topics = {};
        this.subUid = -1;
      }
    
      publish(topic, args) {
        if (!this.topics[topic]) {
          return false;
        }
    
        const subscribers = this.topics[topic];
        let len = subscribers ? subscribers.length : 0;
    
        while (len--) {
          subscribers[len].func(topic, args);
        }
    
        return this;
      }
    
      subscribe(topic, func) {
        if (!this.topics[topic]) {
          this.topics[topic] = [];
        }
    
        const token = (++this.subUid).toString();
        this.topics[topic].push({
          token,
          func,
        });
    
        return token;
      }
    
      unsubscribe(token) {
        for (const m in this.topics) {
          if (this.topics[m]) {
            for (let i = 0; j < this.topics[m].length; i++) {
              if (this.topics[m][i].token === token) {
                this.topics[m].splice(i, 1);
                return token;
              }
            }
          }
        }
      }
    }
    const pubsub = new PubSub();
    
    const messageLogger = (topics, data) => {
      console.log(`Logging: ${topics}: ${data}`);
    };
    
    const subscription = pubsub.subscribe("inbox/newMessage", messageLogger);
    
    pubsub.publish("inbox/newMessage", "hello world");
    ```


#### 중재자 패턴

- 중재자 패턴에 대한 설명이 부정확
- 객체들이 서로 직접 통신하지 않고, 중재자 객체를 통해 상호작용하도록 하는 패턴
- 장점
  - 구성 요소간의 결합도를 낮춤
  - 재사용성을 높임

#### 커맨드 패턴

- 메서드 호출, 요청 또는 작업을 단일 객체로 캡슐화하여 추후에 실행할 수 있도록 해줌

- 실행할 동작과 해당 동작을 호출할 객체를 연결

- 장점

  - 실행 지점을 유연하게 조절 가능
  - 명령을 실행하는 객체와 명령을 호출하는 객체 간의 결합을 느슨하게 하여 구체적인 객체 변경에 대한 유연성 향상

- ```js
  const CarManager = {
    requestInfo(model, id) {
      return `The information for ${model} with ID ${id} is foobar`;
    },
  
    buyVehicle(model, id) {
      return `You have successfully purchased Item ${id}, a ${model}`;
    },
  
    arrangeViewing(model, id) {
      return `You have booked a viewing of ${model} (${id})`;
    },
  };
  
  CarManager.execute = function (name) {
    return (
      CarManager[name] &&
      CarManager[name].apply(CarManager, [].slice.call(arguments, 1))
    );
    // 메서드가 존재하면, .apply(CarManager, arguments)로 호출
    // 여기서 [].slice.call(arguments, 1)은 전달받은 arguments 객체에서 첫 번째 인자를 제외한 나머지를 배열로 변환
    // apply는 함수의 호출 문맥과 인자를 배열로 전달
    // 첫 번째 인자는 this, 그리고 인자로 전달할 배열
    // [].slice.call(arguments, 1)는 유사 배열 객체에 slice를 적용하여 배열로 변환하는 기술
  };
  
  console.log(CarManager.execute("arrangeViewing", "Ferrari", "14523"));
  ```

#### 👀 인사이트

- 이벤트 집합 패턴과 중재자 패턴의 차이
  - 이벤트
    - 둘 다 이벤트를 사용
    - 이벤트 집합 패턴은 그 자체로 이벤트를 처리하기 위한 목적을 가짐
    - 다만 중재자 패턴은 자바스크립트에서 구현을 단순화하기 위해 사용할 뿐 반드시 이벤트를 다룰 필요는 없음
    - 중재자 객체에 대한 참조를 하위 객체에 전달하는 등의 처리가 가능
  - 서드 파티 객체
    - 둘 다 상호작용을 간소화하기 위해 서드파티 객체 사용
    - 이벤트 집합 패턴 자체는 이벤트 발행자와 구독자에 대해 서드 파티 객체
    - 중재자 패턴 또한 다른 객체에 대한 서드 파티 객체
    - 차이점은 애플리케이션 로직과 워크플로가 어디에 구현되어있는지의 차이
    - 이벤트 집합 패턴에서 모든 로직과 워크플로는 이벤트를 발생시키는 객체와 처리하는 객체에 구현됨
    - 그에 반해 중재자 패턴에서 모든 로직과 워크폴로는 중재자 내부에 집중
  - 이벤트 집합 패턴을 사용해야 할 때
    - 직접적인 구독 관계가 많아질 경우
    - 전혀 관련 없는 객체들 간의 소통이 필요할 때
  - 중재자 패턴을 사용해야 할 때
    - 두 개 이상의 객체가 간접적인 관계를 가지고 있고 비즈니스 로직이나 워크플로에 따라 상호작용 및 조정이 필요한 경우
- next가 왜 중재자 패턴일까?
  - 구성요소들이 미들웨어
  - 미들웨어들은 next를 통해서만 상호작용
  - 미들웨어들끼리 직접적으로 연결되지 않고 next를 통해 느슨하게 연결되어있음


***

### Dennis

#### ✈️ 내용 정리
#### 👀 인사이트
