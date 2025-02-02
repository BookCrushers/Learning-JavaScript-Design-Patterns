# 인사이트

## Day1

### Elio

#### 내용 정리

- 디자인 패턴
  - 역사
    - 크리스토퍼 알렉산더라는 건축가의 초창기 작품
    - 특정 디자인 구조를 반복하면 최적의 효과를 얻을 수 있다는 걸 깨달음
    - 두 명의 건축가와 패턴 언어를 만듦
    - 1990년 무렵, 소프트웨어 엔지니어들은 알렉산더의 원칙을 디자인 패턴에 담기 시작
  - 정의
    - 소프트웨어 설계에서 반복되는 문제와 주제에 적용할 수 있는 재사용 가능한 템플릿
  - 장점
    - 검증됨
    - 재사용 가능
    - 알아보기 쉬움
    - 큰 문제 방지
    - 종합적인 해결책 제시
    - DRY
    - 의사소통 원활
    - 커뮤니티 선순환 유발
  - 프로토 패턴
    - 패턴성 검증을 통과하지 못한 패턴
    - 좋은 패턴의 특징
      - 특정 문제 해결 가능
      - 명쾌한 해결책이 없음
      - 확실한 기능만을 말함
      - 관계를 설명
      - 반복성
        - 목적 적합성
        - 유용성
        - 적용 가능성
  - 구조
    - 규칙 형태로 패턴을 제시
      - 아래 세 가지 사항의 관계성을 생각
        - 컨텍스트
        - 집중 목표
        - 구성
    - 구성요소
      - 이름
      - 설명
      - 컨텍스트 개요
      - 문제 제시
      - 해결 방법
      - 설계 내용
      - 구현 방법
      - 시각적 설명
      - 예제
      - 필수 연계
      - 관계성
      - 알려진 용도
      - 토론
  - 안티 패턴
    - 수많은 전역 변수
    - eval 실행
    - Object 클래스 프로토타입 수정
    - 인라인 자바스크립트
    - document.write

#### 인사이트

- 앞의 챕터는 개념 정리라 깊은 내용은 다루고 있지 않음

- 안티 패턴에 대해 깊게 이야기해보면 좋을 것 같음

  - eval이 안 좋은 이유

    - https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/eval#eval%EC%9D%84_%EC%A0%88%EB%8C%80_%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80_%EB%A7%90_%EA%B2%83!

  - Object 클래스의 프로토타입 수정

    - 모든 객체의 상위 프로토타입이기 때문에 이를 수정하면 Javascript의 모든 객체에 영향을 미침

    - [속성 상속](https://developer.mozilla.org/ko/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#속성_상속)

      JavaScript 객체는 속성을 저장하는 동적인 "가방"과 (**자기만의 속성**이라고 부릅니다) 프로토타입 객체에 대한 링크를 가집니다. 객체의 어떤 속성에 접근하려할 때, 그 객체 자체 속성 뿐만 아니라 객체의 프로토타입, 그 프로토타입의 프로토타입 등 프로토타입 체인의 종단에 이를 때까지 그 속성을 탐색합니다

  - 자바스크립트 인라인 사용

    - 유지보수 어려움
    - 보안 문제
      - HTML 파일의 인라인 스크립트를 변경하여 악성 코드 삽입 가능
    - 성능 문제
      - 별도의 스크립트 파일로 작성하면 브라우저 캐싱 가능
    - 테스트 및 디버깅 어려움
    - 코드 재사용성 감소

***

## Dennis

#### 내용 정리
- 패턴은 반복되는 문제를 설계적으로 해결하기 위해 나타났다.
- 새로운 패턴 제안은 있을 수 있으나 채택되기 위해서는 커뮤니티와 개발자의 여러 차례 검증을 받아야한다.
- 이는 패턴성 검증이라고 하며 **세가지 법칙**을 충족시켜야 디자인패턴으로 인정받을 수 있다.
  - 패턴성 검증을 받지 못한 패턴을 ***프로토 패턴***이라 칭한다.
  - 세가지 법칙은 목적 적합성, 유용성, 적용 가능성을 일컫는다.


#### 인사이트

건축으로 부터 시작된 패턴이라는 개념이 소프트웨어 엔지니어들을 통해서 추상적인 소프트웨어의 세계에 적용되었다는 점이 놀랍다
