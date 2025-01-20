## Day7

### Elio

#### ✈️ 내용 정리

#### 비동기 프로그래밍

- 자바스크립트에서 동기 코드는 블로킹 방식으로 실행
- 호출자에게 결과를 반환하기 전 함수 내부의 코드가 처음부터 끝까지 한 줄씩 실행
- 비동기 코드는 논블로킹 방식으로 실행
- 함수 내부의 코드가 백그라운드에서 실행되며 호출자에게 즉시 결과 반환
- 비동기 코드는 네트워크 요청, 데이터베이스 읽기/쓰기, 또는 기타 I/O 작업

- 방법
  - 콜백
  - 프로미스
  - async await

#### 배경

- 다른 함수의 인수로 전달되어 비동기 작업이 완료된 후 실행
- 중첩된 콜백 구조로 인해 콜백 지옥 초래 가능
- 프로미스를 통해 이를 해결 가능

#### 프로미스 패턴

- 프로미스는 비동기 작업의 결과를 나타내는 객체
- 대기, 완료, 거부의 세 가지 상태를 가짐
- promise 생성자를 통해 만들 수 있으며, 함수를 인수로 받음
- 이 함수는 인수로 resolve, reject를 인수로 받음
- then, catch 메서드를 통해 요청의 결과를 처리 가능

#### 프로미스 메모이제이션

- cache라는 Map을 만들고 데이터가 있을 경우 cache Map에서 데이터를 바로 반환

**프로미스 체이닝과 프로미스 파이프라인의 차이가 무엇인지?**

- 파이프라인 패턴은 여러 비동기 작업을 순차적으로 실행해야 할 때, 각 작업의 결과를 다음 작업에 전달하며 실행하는 방식

#### 프로미스 재시도

```js
function makeRequeestWithRetry(url) {
  let attempts = 0;

  const makeRequest = () =>
    new Promise((resolve, reject) => {
      fetch(url)
        .then(response => response.json())
        .then(data => resolve(data))
        .catch(error => reject(error));
    });

  const retry = error => {
    attempts++;

    if (attempts >= 3) {
      throw new Error('Request failed after 3 attempts');
    }

    console.log(`Retrying request attempt ${attempts}`);
    return makeRequest().catch(retry);
  };

  return makeRequest().catch(retry);
}

const invalidUrl = 'https://invalid-url.example';
const validUrl = 'https://jsonplaceholder.typicode.com/posts/1';


makeRequeestWithRetry(invalidUrl)
  .then(data => {
    console.log('This should not log because the URL is invalid:', data);
  })
  .catch(error => {
    console.error('Expected error:', error.message);
  });

```

#### 프로미스 경쟁

- race 메서드를 사용하면 가장 먼저 완료되는 프로미스의 결과를 반환

#### async/await 패턴

- 비동기 코드를 동기 코드처럼 작성할 수 있게 해주는 javascript의 기능

#### 👀 인사이트

https://github.com/dudwns0921/TIL/blob/master/React/query_gg_laying_the_foundation.md

### Dennis

#### ✈️ 내용 정리

비동기 프로그래밍
- 자바스크립트의 동기 코드는 블로킹(blocking) 방식으로 실행됨
- 그에 반에, 비동기 코드는 논블로킹 (non-blocking) 방식으로 실행된다. 즉, 현재 실행 중인 코드가 다른 작업을 기다리는 동안 백그라운드에서 해당 비동기 코드를 실행 할 수 있다.
- 이런 비동기 작업을 제어하기 위해 자바스크립트는 `async/await` 과 `promise`를 지원한다.
- ES6 이전에는 callback 함수를 통해 비동기 함수들의 타이밍을 제어했다.
- 복잡한 코드에 직면할수록 callback 지옥을 만들어 가독성을 떨어트렸으며 이로인해 promise가 나오게되었다.
- promise를 활용한 다양한 기술들
  - 프로미스 체이닝
  - 프로미스 에러 처리
  - 프로미스 병렬 처리리
  - 프로미스 순차 실행
  - 프로미스 메모이제이션
  - 프로미스 파이프 라인
  - 프로미스 재시도
  - 프로미스 데코레이터 
  - 프로미스 경쟁
- `async/await` 을 사용하여 비동기 코드를 동기 코드처럼 작성할 수 있게 됨
  - 비동기 함수들을 조합하여 동기적을 사용할 수 있음
  - for-await-of 반복문을 사용하여 비동기 반복 가능 객체를 순회할 수 있음 (제너레이터, stream 객체 등등)
  - try-catch 문을 활용하여 에러 처리
  - 대부분 프로미스와 `async/await`을 활용하여 위에서 언급한 기술들을 간결하게 사용 가능


#### 👀 인사이트

파이프라인을 왜 쓰는가

아래 코드는 라이브러리를 사용하여 값을 필터링하고 맵핑하는 함수이다.
문제점은 함수들이 사용하려는 순서와 반대로 작성되어 있어 흐름을 파악하기 어렵다.

```js
const result = Array.from(
  slice(
    map(
      filter(numbers, (v) => v % 2 === 0),
      (v) => v + 1
    ),
    0,
    3
  )
);
```

아래와 같이 파이프라인 연산자 ((TC39 제안)[https://tc39.es/proposal-pipeline-operator/])을 사용하면 함수를 사용하려는 순서대로 처음에 filter가 오는걸 볼 수 있다.

```js
const result =
  numbers |> filter(#, (v) => v % 2 === 0) |> map(#, (v) => v + 1) |> slice(#, 0, 3) |> Array.from;
```

