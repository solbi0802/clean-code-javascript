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

    
   - eqeq 줄이기
   == : 동등연산자 <br>
   === : 엄격한 동등연산 <br>

   '1' == 1 // true <br>
    1 == true // true <br>
   '1' === 1  // false <br>
   1 === true // false <br>

  * eslint eqeqeq 설정해서 실수를 줄이자 <br>


  - 형 변환 주의하기 <br>

  ==  느슨한 검사 => 암묵적인 형 변환 <br>

  12 + ' 문자열 ' // '12 문자열' <br>

  !!'문자열 // true <br>
  !!'' // false <br>

  // 명시적 변환 <br>
  parseInt('9.99', 10); // 9 <br>
  String() <br>
  Boolean() <br>
  Number() <br>

  결론
   ```
   사용자가 형 변환했을 때는 명시적인 변환 (타입)
   JS가 하면 암묵적인 변환(타입)
   따라서, 명시적인 형변환을 활용하자
   ```
   
 - isNaN <br>
   Number.MAX_SAFE_INTEGER <br>
   Number.isInteger <br>

   iaNaN(is Not A Number 숫자가 아니다)
   ````
   isNaN(123) // false 숫자가 숫자가 아니다 => 숫자가 맞다라는 의미
   isNaN(123 + '숫자') // true
   Number.isNaN(123 + '숫자') //  false
   ````
 
  결론 <br>
  ```isNaN은 느슨한 검사이므로, Number.isNaN과 같은 엄격한 검사로 코드를 짜는 것이 좋다.```

 <hr>

 3. 경계
  - min -max <br>
   ```
   function genRandomNumber(min, max) {
    return Math.floor(Math.random() * (max - min + 1))  + min; 
   }
  // 상수 <br>
  genRandomNumber(MIN_NUMBER, MAX_NUMBER) <br>
  ```

 결론 
  ````
  1. 최소, 최대값을 다룬다
  2. 최소, 최대값 포함 여부 결정해야한다 (이상-초과 / 이하-미만)
  3. 혹은 네이밍에 최소값과 최대값 포함 여부를 표현한다.
  ````

- begin-end  <br>

 ````
 function reservationDate(beginDate, endDate) {
  // ...
 }

 reservationDate('YYYY-MM-DD', 'YYYY-MM-DD');
 ````

- first-last  <br>
포함된 양 끝을 의미한다.  <br>
부터 ~ 까지  <br>

````
 const students = ['철수', '영희', '기훈'];
 function getStudents(first, last) {
   ...
 }
 getStudents('철수' , '기훈);
 // dom에서 firstChild, lastChild 개념
 ````

- prefix-suffix (접두사,접미사)  <br>
 ex) javascript setter, getter  <br>
 react useState, useEffect, useRef, ...와 같은 형태  <br>

   suffix  <br>
   파일트리  <br>
   복수 -> s, 단일 -> s없이 작성  <br>

 
- 매개변수의 순서가 경계다  <br>
호출하는 함수의 네이밍과 인자의 순서의 연관성을 고려한다.  <br>

````
function somFunc(someArg, someArg2) {
}
getRandomNumber(1,50);
getDates('2022-01-01', '2022-01-06');
````

1.  매개변수를 2개가 넘지 않도록 만든다.  <br>
2. arguments, rest parameter 고려  <br>
3. 매개변수를 객체에 담아서 넘긴다.  <br>
4. 랩핑 하는 함수 ( 그 함수를 또 호출하는 형태)  <br>
  ````
  function somFunc(someArg1, someArg2, someArg3, , someArg4) {
  }
  function getFunc(someArg1, someArg2) {
   somFunc(someArg1, someArg2)
  }
  ````

<hr/>

4. 분기 <br>
  - 값식문 <br>
    ```
    1. 함수의 매개변수로는 값, 식만 올 수 있다.  <br>
    2. React - JSX 케이스 :  babel을 통해 transformed되면 객체로 변경된다. (if,switch,for문 x)  <br>
    3. 문법을 제대로 아는 게 중요!  <br>
    4. 고차 함수를 활용해야 한다.  <br>
    ```
    
  - 삼항 연산자 다루기 <br>
     3개의 피연산자로 이루어져있음 <br>
     조건 ? 참 [식] : 거짓 [식] 
     ```
     1. 조건이 너무 많다면 switch case를 고려해보자
     2. 괄호를 사용해서 가독성을 높이자.
     3. Nullable한 상황에서도 사용할 수 있다.
     ```
   
 - Truthy & Falsy <br>
   [Truthy : 참 같은 값](https://developer.mozilla.org/ko/docs/Glossary/Truthy)
   [Falsy : 거짓 같은 값](https://developer.mozilla.org/ko/docs/Glossary/Falsy)
   부정 조건을 걸어도 실행이 잘 됨
   
 - 단축 평가 (short-circuit evaluation) <br>
   ```
   /**
   * AND
   */
   true && true && '도달 O'   // '도달 O'
   true && true && '도달 X' // false
   
   /**
   * OR
   */
   false || false || '도달 O'   // '도달 O'
   true || true || '도달 X' // true
   ```
   예시1)
      ```
       function fetchData () {
        // if (state.data) {
        //  return state.data;
        // } else {
        // return 'Fetching...';
        // }
        // return state.data ? state.data : 'Fetching...';
         return state.data || 'Fetching...';  // 간략하게 표기 가능
         }
       ```
     
  
  예시2)
   ```
   function favoriteDog(someDog) {
    // let favoriteDog;
    // if (someDog) {
    //     favoriteDog = someDog;
    // } else {
    //     favoriteDog = '냐옹';
    // }
    return `${(someDog || '냐옹')} 입니다';
    }

  favoriteDog () //  강아지 이름이 없으면 '냐옹' 출력
  favoriteDog('치와와') // ${강아지이름} 입니다 출력
  ```
   예시3)
   ```
   const getActiveUserName = (user, isLogin) => {
   //   if (isLogin) {
   //     if (user) {
   //       if (user.name) {
   //         return user.name;
   //       } else {
   //         return '이름없음';
   //       }
   //     }
   //   }

   if (isLogin && user) {
    return user.name || '이름없음';
    }
  }
   ```
  
 - else if 피하기  <br>
     ```
      const x = 1;
      if (x >= 0) {
      console.log('x는 0과 같거나 크다');
      } else if (x > 0) {
      console.log('x는 0보다 크다');
      }
      // x는 0과 같거나 크다
      
      if (x >= 0) {
      console.log('x는 0과 같거나 크다');
      } else {
      if (x > 0) {
      console.log('x는 0보다 크다');
      }
     }
     ```
   결론 
   ```
   else if문이 파이프라인(Promise .then().then())으로 흐른 다고 생각 x 
   많이 늘어 질 경우, switch case로 바꾸자
   ```
   
 - else 피하기 <br>
    ```
    /**
    * age가 20 미만일 때 특수 함수 실행
    */

    function getHelloCustomer(user) {
     if (user.age < 20) {
       report(user);
      } else {
       return '안녕하세요';  // 20미만 일 떄 if문에서 이미 리턴되어서, else 문을 타지 않음
      }
     ```
    => 수정
     ```
     function getHelloCustomer2(user) {
       if (user.age < 20) {
        report(user);
       }
      return '안녕하세요';
      }
     ```
 - Early Return <br>
     ```
      function loginService(isLogin, user) {
       if (!isLogin) {
        if (checkToken()) {
         if (!user.nickName) {
           return registerUser(user);
          } else {
           refreshToken();
           return '로그인 성공';
         }
        } else {
         throw new Error('No Token');
        }
      }
    }
    ```
    => 수정
     ```
     function loginService(isLogin, user) {
      //Early Return
      /**
       * 함수를 미리 종료
       * 사고하기 편하다
      */

       if (isLogin) {
         return;
        }

        if (!checkToken()) {
         throw new Error('No Token');
        }
        
        if (!user.nickName) {
         registerUser(user);
        }
        refreshToken();
        return '로그인 성공';
       }
      ```
      로직이 하나의 조건에만 의존적으로 작성이 되어 있을 때 사용!

 - 부정 조건문 지양하기 <br>
   숫자인 지 검사할 때 isNaN을 쓰면 혼동됨
   ```
   function isNumber(num) {
     return !Number.isNaN(num) && typeof num === 'number'
   }
   
   if (isNumber(3)) {
     console.log('숫자입니다')
   }
   ```
   결론
   ```
   1. 생각을 여러 번 해야 할 수 있다.
   2. 프로그래밍 언어 자체로 if문이 처음부터 오고 true부터 실행시킨다.
   - 부정 조건 예외
    1. Early Return
    2. Form Validation
    3. 보안 혹은 검사 하는 로직
   ```
   
 - Default Case 고려하기 <br>
   
 - 명시적인 연산자 사용 지향하기  <br>
 - Nullish coalescing operator  <br>
 - 드모르간의 법칙  <br>
