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

#### 👀 인사이트

