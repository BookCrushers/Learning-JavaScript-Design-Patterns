## Day8

### Elio

#### ✈️ 내용 정리

모둘형이란 의존성이 낮은 기능들이 모듈로써 저장된 형태를 뜻함

이러한 느슨한 결합은 의존성을 제거하여 애플리케이션의 유지보수를 용이하게 만듦

자바스크립트는 ES2015부터 네이티브 모듈을 도입했지만 아래 3가지 방식으로 모듈형 자바스크립트를 사용할 수 있었음

- AMD
- CommonJS
- UMD

#### 스크립트 로더

- 스크립트 로더는 모듈형 자바스크립트를 구현하기 위한 핵심적인 도구
- RequireJs, curl.js
  - https://requirejs.org/docs/download.html

#### AMD

- 모듈과 의존성 모두를 비동기적으로 로드할 수 있도록 설계
- 코드와 모듈 간 긴밀한 결합을 줄여줌
- 자바스크립트 모듈화을 위한 주춧돌 역할을 함
- CommonJS 모듈 형식에 대한 사양 초안으로 시작했으나 도입에 전체적인 합의를 이루지 못해 추가 개발은 amdjs 그룹에게 넘어감
- CommonJS 진영의 모두가 AMD 형식을 지향하지 않기 때문에 AMD 또는 비동기 모듈 지원이라고 부르는 게 적절
- 예시

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script data-main="main" src="require.js"></script>
  </head>
  <body></body>
</html>
```

```js
// main.js
require(["greeting"], function (greeting) {
  const message = greeting.greet("영준", 3, 4);
  console.log(message); // 출력: Hello, 영준! Did you know that 3 + 4 equals 7?
});

// greeting.js
define(["math"], function (math) {
  return {
    greet: function (name, a, b) {
      const sum = math.add(a, b);
      return `Hello, ${name}! Did you know that ${a} + ${b} equals ${sum}?`;
    },
  };
});

// math.js
define([], function () {
  return {
    add: function (a, b) {
      return a + b;
    },
    subtract: function (a, b) {
      return a - b;
    },
  };
});
```

#### CommonJS

- 서버 사이드에서 모듈을 선언하는 간단한 API를 지정하는 모듈 제안
- 케빈 당구르가 2009년에 ServerJS 프로젝트에서 만들어진 이 형식은 CommonJS 그룹에 의해 공식화됨
- 모듈과 패키지, 두 가지에 대한 표준을 정립하려 노력
- CommonJS 모듈은 재사용 가능한 자바스크립트 코드로써 외부 의존 코드에 공개할 특정 객체를 내보냄
- CommonJS은 AMD처럼 모듈을 함수로 감싸는 작업이 필요하지 않음
- exports 변수는 다른 모듈에 내보내고자 하는 객체를 담음
- require 함수는 다른 모듈에서 내보낸 객체를 가져올 때 사용하는 함수
- Node.js 환경에서는 CommonJs가 여전히 기본 형식
  - .cjs
  - package.json의 type이 commonjs인 경우
  - type 항목이 존재하지 않는 경우, .js 확장자를 가진 파일
  - .mjs, .cjs, .json, .node, .js 이외의 확장자를 가진 파일

#### AMD vs CommonJS

- AMD
  - 브라우저 우선 접근 방식 채택
  - 비동기 동작과 간소화된 하위 호환성 선택
  - 파일 I/O의 개념은 없음
  - 객체, 함수, 생성자, 문자열, JSON 등 다양한 모듈 지원
  - 브라우저에서 자체적으로 실행됨
- CommonJS
  - 서버 우선 접근 방식
  - 동기적 작동
  - 전역 변수와의 독립성
  - 미래의 서버 환경 고려

#### UMD: 플러그인을 위한 AMD 및 CommonJS 호환 모듈

- 브라우저와 서버 환경에서 모두 작동할 수 있는 모듈을 원하는 개발자에게는 기존 AMD와 CommonJS의 약점을 해결하는 방안 필요
- 이를 위해 UMD 등장

#### 👀 인사이트

#### CommonJS를 브라우저에서 써보기

기본적으로 commonJS는 node.js 환경에서 실행 가능

이에 따라 번들러 등을 통해 commonJs 모듈을 ES모듈로 변경해주어야 함

```js
// vite.config.js

import { defineConfig } from "vite";
import commonjs from "@rollup/plugin-commonjs";

export default defineConfig({
  plugins: [commonjs()],
});
```

이미 ESM이 표준이지만 cjs는 아래와 같은 이유로 사용하는 경우가 있음

- 호환성
- 동적 require
  - require은 if문 등에서 동적으로 사용 가능
  - import문 정적, import문은 항상 파일의 최상단에 위치해야 하고, 코드 실행 전에 모든 import가 미리 처리됨

#### ESM을 사용하는 것이 좋은 이유

1. **명확한 표준**: ESM은 JavaScript의 공식 표준이므로, ECMAScript 사양에 맞춰져 있어 모든 최신 JavaScript 환경에서 일관되게 동작
2. **성능**: ESM은 브라우저와 Node.js에서 더 효율적으로 작동할 수 있도록 최적화되어 있음. 예를 들어, ESM은 정적 분석을 통해 트리 쉐이킹(tree-shaking)과 같은 최적화를 더 잘 수행
3. **미래지향적**: 최신 도구와 라이브러리들이 ESM을 기본으로 지원하므로, ESM을 사용하면 앞으로의 호환성 문제를 피할 수 있음

### Dennis

#### ✈️ 내용 정리

스크립트 로더

- JavaScript에서 스크립트 로더는 동적으로 JavaScript 파일(스크립트)을 로드하고 관리하는 도구나 코드 패턴
- 주요역할
  - 비동기 로드: 스크립트를 HTML에 미리 포함하지 않고, 필요할 때 비동기적으로 로드
  - 종속성 관리: 특정 스크립트가 다른 스크립트에 의존하는 경우, 올바른 순서로 로드
  - 중복 방지: 동일한 스크립트를 여러 번 로드하지 않도록 방지
  - 성능 최적화: 필요한 스크립트만 로드하여 초기 로딩 시간을 줄이고, 네트워크 리소스를 효율적으로 사용

AMD

- 브라우저에서 모듈 시스템을 사용하기 위해 개발된 모듈 시스템
- RequireJS와 curl.js 라이브러리 활용하여 모듈 시스템 사용
- define 메서드 : 모듈의 정의를 구현
  ```js
    define(
      module_id // optional 생략하면 '익명 모듈'
      [dependencies] //optional
      definition function {} // 모듈이나 객체를 인스턴스화 하는 함수
    )
  ```
- require 메소드 : 의존선 로딩을 처리
  ```js
  require(['foo', 'bar'], function (foo, bar {
    foo.doSomething()
  }))
  ```

CommonJS

- Node.js에서 기본적으로 사용하는 모듈 시스템.
- 동기적으로 모듈을 로드(런타임에 종속성 결정).
- require와 module.exports를 사용.

  ```js
  // 선언
  function foo() {
    console.log("foo");
  }
  exports.foo = foo;
  ```
  ```js
  //import
  var exampleModule = require(".example-10-7");
  ```

AMD vs CommonJS

- AMD
  - 브라우저 우선 접근 방식을 채택
  - 비동기 동작과 간소화된 하위 호환성
  - 파일 I/O에 대한 개념 없음
  - 객체, 함수, 생성자, 문자열, JSON등 다양한 형태의 모듈을 지원
  - 브라우저에서 자체적으로 실행되어 유연한 포맷 지원
- CJS
  - 서버 우선 접근 방식
  - 동기적 작동
  - 전역 변수와의 독립성
  - 미래의 서버 환경 고려
  - 언래핑된 모듈을 지원

#### 👀 인사이트

CJS

- wrapping module

  ```js
  console.log(__filename); // 현재 파일의 절대 경로
  console.log(module.exports); // 모듈의 exports 객체
  ```

  위 코드는 실제로 다음과 같이 실행

  ```js
  (function (exports, require, module, __filename, __dirname) {
    console.log(__filename);
    console.log(module.exports);
  });
  ```

- unwrapping module
  - 위와같은 함수로 감싸지지 않은 상태로 실행되는 모듈 모듈 코드가 별도의 함수 스코프 없이 실행
  ```js
  // example.js (언래핑된 모듈)
  console.log("This code runs in the global scope.");
  ```

CommonJS에서 언래핑된 모듈을 지원하는 이유

- CommonJS는 모듈의 유연성을 높이기 위해 언래핑된 모듈도 지원
