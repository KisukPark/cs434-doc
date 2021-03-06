# Chapter 3. 함수

## Contents
* [작게 만들어라!](#작게-만들어라!)
* [한 가지만 해라!](#한-가지만-해라!)
* [함수 당 추상화 수준은 하나로!](#함수-당-추상화-수준은-하나로!)
* [서술적인 이름을 사용하라!](#서술적인-이름을-사용하라!)
* [함수 인수](#함수-인수)
* [부수 효과(Side Effect)를 일으키지 마라!](#부수-효과(Side-Effect)를-일으키지-마라!)
* [명령과 조회를 분리하라!](#명령과-조회를-분리하라!)
* [오류 코드보다 예외를 사용하라!](#오류-코드보다-예외를-사용하라!)
* [반복하지 마라](#반복하지-마라)
* [결론 ](#결론)
---
### 작게 만들어라!
* 함수는 작게 만드는 것이 좋다.
* if-else, while 문 안의 블록에서는 함수를 호출하는 형태로 작게 만들 수 있다.

### 한 가지만 해라!
* 함수는 한 가지를 해야 한다.

### 함수 당 추상화 수준은 하나로!
* 함수 안에서는 추상화 수준이 하나인 단계만을 수행해야 한다.
* 코드를 위에서 아래로 이야기처럼 읽혀야 좋다.

### 서술적인 이름을 사용하라!
* 코드를 읽으면서 짐작했던 기능을 그대로 수행한다면 좋은 코드이다.
* 함수가 작고 단순해야 좋은 이름을 고르기 쉽다.
* 이름은 길어도 좋다. 길고 서술적인 이름이 짧고 어렵운 이름 혹은 주석 보다 낫다.
* 함수 이름 자체로 동작이 **짐작** 가능해야 한다. 그러려면 모듈 내 일관성이 있어야 한다.
* ex) `testableHtml, SetupTeardownInclude, calculateCommissonPay`

### 함수 인수
* 함수 인수는 적을 수록 이해하기 쉽다.
* 플래그(Bool) 인수는 최대한 피하라. true/false 에 따라 복수개의 동작을 한다는 의미니까. 대신 함수를 나눠라.
* 단항 함수
  1. 질문 `fileExists(string)`
  1. 변환 후 반환 `fileOpen(string)`
  1. 이벤트 `passwordAttemptFailed`
* 이항 함수
  1. 한 값을 표현하는 경우
  1. 자연적인 순서가 있는 경우 `Point(x, y)`
* 인수가 많아진다면 **인수 객체**를 고려해라. 인수 객체를 사용하기 위해선 적절한 이름으로 맥락을 부여하므로 나쁜 게 아니다.
* 좋은 함수 이름을 사용해야한다.
  1. 단항 함수는 함수와 인수가 동사/명사 쌍을 이룬다. `write(name), writeField(name)`
  1. 함수 이름에 인수 이름을 넣으면 순서를 기억할 필요가 없다. `assertEquals` -> `assertExpectedEqualsActual`

### 부수 효과(Side Effect)를 일으키지 마라!
* 함수 이름에서 짐작되지 않는 동작을 하지 마라.
* 동작을 서술할 수 있게 이름을 변경하든, 함수를 나눠라.
* 함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식이 낫다.

### 명령과 조회를 분리하라!
* 함수는 **(1)수행** 하거나 **(2)답하거나** 둘 중 하나만 해야 한다.
  ```java
  // Bad) set 이 값을 저장함과 동시에 성공 여부를 답하고 있다.
  if (set("username", "unclebob"))...

  // Good) 명령과 조회를 분리한 경우
  if (attributeExits("username")) {
    setAttribute("username", "unclebob");
  }
  ```

### 오류 코드보다 예외를 사용하라!
* 오류 코드를 사용하다 보면 새로운 코드를 추가하기 부담스러워 의미에 맞지 않더라도 코드를 재사용하게 된다. 예외는 Exception 클래스에서 파생되므로 그런 부담이 없다.
* 오류 처리도 한 가지 작업이므로 try-catch 를 별도로 빼는 게 낫다.
  ```java
  // Bad) try-catch 와 로직이 섞인 경우
  private void openFile(String name) {
    try { ... } catch (error) {...}
  }
  private void closeFile(File file) {
    try { ... } catch (error) {...}
  }
  
  // Good) try-catch 를 별도로 분리한 경우. 각각 함수 안에서는 try-catch 를 제거
  private void handleFile(String name) {
    try {
      File file = openFile(name);
      closeFile(file);
    } catch (error) {
      ...
    }
  }  
  ```

### 반복하지 마라
* 반복이 발생하면 코드가 길어지고, 유지보수가 힘들어지고, 한 곳만 수정하여 버그가 생길 수 있다.
---
## 결론
* 함수 선언부를 찾아보는 행위는 코드를 보다가 주춤하는 것과 같다. 피해야 한다. 
* 소프트웨어를 작성하는 행위는 글짓기와 비슷하다.
* 프로그래밍의 기술은 언제나 언어 설계의 기술이다.
* 대가 프로그래머는 시스템을 (구현할) 프로그램이 아니라 (풀어갈) 이야기로 여긴다.
* 우리의 목표는 시스템이라는 이야기를 분명하고 명확하고 쉽게 풀어가는 것이다.
