# clean-code-javascript (클린코드 자바스크립트)

#### [클린코드 자바스크립트 강의](https://www.udemy.com/course/clean-code-js/)를 듣고 정리한 내용입니다.

목차

 1. [변수](#변수)
 2. 타입
 3. 경계
 4. 분기
 5. 배열
 6. 객체
 7. 함수
 8. 에러
 9. Web API
 10. 추상화
 11. 관용과 관습
 12. 도구에 위임

<hr/> 

1. 변수
  - var를 지양하자 <br>
   var는 함수 스코프 <br>
   let, const는 블록 스코프 <br>
   var의 경우 값은 다른데 동일한 변수 설정했을 때 재할당, 재선언까지 계속 가능 -> 위험 요소 증가

 - 전역 공간 사용 최소화 <br>
  최상위 -> 브라우저(window),Node.js(global) <br>
  전역으로 선언하면, 다른 파일에서 선언한 변수가 동일하게 설정했을 때 서로 영향을 끼침 (어디서나 접근 가능)
  
  + 해결 
  ````
  1. 전역변수를 만들 지 않는다. (지역변수만 만듦)
  2. Window, globbal을 조작 x
  3. const, let으로 커버가능
  4. IIFE, Module, Closure, 스코프를 나누는 방법으로 처리
  ````


2. 타입
