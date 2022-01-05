# clean-code-javascript (클린코드 자바스크립트)

#### [클린코드 자바스크립트 강의](https://www.udemy.com/course/clean-code-js/)를 듣고 정리한 내용입니다.

목차

 1. 변수
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
  
   해결 
   ````
   1. 전역변수를 만들 지 않는다. (지역변수만 만듦)
   2. Window, globbal을 조작 x
   3. const, let으로 커버가능
   4. IIFE, Module, Closure, 스코프를 나누는 방법으로 처리
   ````
  - 임시변수를 만들지 말자 <br>
    이유 : 명령형으로 가득한 코드, 로직이 나오게 되서, 어디서/어떻게 잘못되었는 지 디버깅 하기 어렵다. <br>
     추가적인 코드를 작성하고 싶은 유혹에 빠지기 쉽다 (코드 유지 보수 어려움) <br>

    해결
    ````
    함수 나누기
    바로 반환하기
    고차 함수 사용(map, filter, reduce)
    선언형 프로그래밍
    ````

  - 호이스팅 주의하기 <br>
    호이스팅? 런타임시 선언이 최상단으로 끌어 올려짐

    문제점 : 코드 작성시 예측하지 못한 실행결과가 노출된다.

    해결
    ````
    var를 사용 하지 않는다.  => let,const 지향
    함수도 호이스팅 된다는 사실을 잊지말자 => 함수 표현식 사용
     ````
     
     <hr>

2. 타입
  - 타입 검사 <br>
    typeof <br>
     PRIMITIVE vs REFERENCE (object, array, date, function ....) <br>
      ````
      function myFunc () {}
      typeof myFunction  // function

      classs Myclass {}
      typeof MyClass  // function

      const str = new String('문자열')
      typeof str  // object

      typeof null  // object (언어적 오류)
     ````
     instanceof 연산자 (객체의 프로포타입 체인을 검사)
     ````
      function Person(name, age) {
       this.name = name;
       this.age= age;
       }
      
      const p = {
      name : 'sb',
      address: 25 
      }  
      
      p instanceof Person // true 
      
      const sb = new Person('sb', 20)
    
      p instanceof Person // false

     ````
     Array, function, Date 타입 검사


     ````
     const arr = []
     const func = function() {}
     const date = new Date()

     arr instanceof Object  // true
     func instanceof Object // true
     date instanceof Object // true

     // 최상위에 오브젝트가 있어서 오브젝트 타입으로 여겨짐

     Object.prototype.toString.call('string')    // '[object String]'

    Object.prototype.toString.call(date)    // '[object Date]'
    
    ````

    결론 
    ````
    1. 자바스크립트는 동적인 타입을 가진 언어여서, 타입 검사가 어렵다.
    2. 상황에 맞게 타입 검사를 잘 찾아서 하자
    3. Primitive vs Reference 차이를 이해
    ````

   - undefined & null <br>
     ```
     !null //true
     !!null // false
     null ==== false // false
     !null === true // true
     nul + 123 // 123
     
     let var;
     var // undefined
     typeof var // undefined (선언했지만 값은 정의되지 않고 할당 x) 
     undefined + 10 // NaN
     ```

   결론
   ````
   undefined와 null은 값이 없거나(명시적 표현) 정의되지 않은 상태이다.
   null은 0에 가깝고, object타입이며, undefined는 NaN이며 undefined 타입이다.
   ````

    

