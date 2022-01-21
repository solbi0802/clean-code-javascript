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
     
      ```
      function createElement(type, height, width) {
        const element = document.createElement(type || 'div');
        element.style.height = height || 100;
        element.style.width = width || 100;
        return element;
       }
       createElement();
      ```
      사용자가 실수할 수 있거나, 개발하면서 빼먹을 수 있는 경우를 대비하여 디폴트값을 꼭 설정하자.

 - 명시적인 연산자 사용 지향하기  <br>
      ```
      if ((isLogin && token) || user)
      ``` 
      
     괄호를 활용하여 우선 순위 지정
     
     ```
     function decrement(number) {
       // number --
       // --number
       number = number - 1
     }
     function increment(number) {
       // ++number;
       // number++;
       number = number + 1
     }
     ```
     결론 
     ```
     1. 예측 가능하고 디버깅 하기 쉬운 코드 작성
     2. 괄호를 활용해서 연산자 우선순위 표기
     ```

 - Nullish coalescing operator  <br>
   [참고자료](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)

   예시1
   ```
   function helloWorld(message) {
     if (!message) {
      return 'Hello! World';
     }
     return 'Hello! ' + (message || 'World');
    }
    // message에 undefined일때도 Hello! World 출력 
    // 0이 와도 똑같이 출력됨
   ```
   수정
   ```
   function helloWorld(message) {
     return 'Hello! ' + (message ?? 'World');
    }
   ```
   예시2
   ```
   console.log(null || undefined ?? "foo"); // syntaxError 발생 
   ```
   수정 
   ```
   console.log((null || undefined) ?? "foo"); // foo
   ```
   
 - 드모르간의 법칙  <br>
   
   ```
    true === not true
    false === not false
    
    if (!(A && B)) {
    }
    위 코드는 서로 같음
    if (!A || !B) {
    }
   ```

   ```
   const isValidUser = true;
   const isValidToken = false; 

   // if (!(isValidToken && isValidUser)) {
    // console.log('로그인 실패!');
   // }

   // 수정
    if (!isValidToken || !isValidUser) {
      console.log('로그인 실패!');
     }
   ```
   
    ```
   const isValidUser = false;
   const isValidToken = false; 

   // if (!(isValidToken || isValidUser)) {
    // console.log('로그인 실패!');
   // }

   // 수정
    if (!isValidToken && !isValidUser) {
      console.log('로그인 실패!');
     }
    ```
    
    <hr/>
    
  5. 배열 <br>

   - Javascript의 배열은 객체다 <br>
     ```
     const arr = [1, 2, 3];
     
     arr[3] = 'test';
     arr['property'] = 'string value';
     arr['obj'] = {};
     arr[{}] = [1, 2, 3];
     arr['func'] = function () {
       return 'hello';
     };
     
     arr // [1,2,3, 'test', property : 'string value', obj: {}, '[object Object]' : [1, 2, 3], func: [] ]
     
     const obj = {
       arr : [1, 2, 3]
       3 : 'test',
       property: 'string value',
       obj : {},
       '{}' : [1, 2, 3],
       func : function () {
        return 'hello'
        }
       } 
     obj // { 3: 'test', arr: [1, 2, 3], property: 'string value', obj : {}, '{}': [1, 2, 3], func: [: func] } 
     
     ```
     ```
      const arr = [1, 2, 3];
      console.log(Array.isArray(arr)); // true
     ```
   
   - Array.length <br>
    
     ```
     const arr = [1, 2, 3];
     console.log(arr.length); // 3
     
     arr.length = 10;
     console.log(arr.length); 10 
     console.log(arr); // [1, 2, 3, , , , , , , ]
     ```
     
     ```
     const arr = [1, 2, 3];
     arr[3] = 4;
     console.log(arr.length); // 4
     arr[9] = 10;
     console.log(arr); // [1,2,3,4, , , , , , 10 ]
     console.log(arr.length); // 10
     ```

     arr.length는 마지막 인덱스를 알려주는 용도
     
   - 배열 요소에 접근하기 <br>
       예시1)
        ```
        const array = [1, 2, 3]
       
        function operateTime(input, operators, is) {
          inputs[0].split('').forEach((num} => {
           cy.get('.digit').contains(num).click();
          });
         
          inputs[1].split('').forEach((num) => {
           cy.get('.digit').contains(num).click();
           });
          } 

         // 수정
         const array = [1, 2, 3]
       
         function operateTime(input, operators, is) {
           const [firstInput, secondInput] = inputs
           firstInput].split('').forEach((num} => {
            cy.get('.digit').contains(num).click();
           });
         
           secondInput.split('').forEach((num) => {
            cy.get('.digit').contains(num).click();
            });
           }
           
           // 최종
           const array = [1, 2, 3]
       
           function operateTime(firstInput, secondInput, is) {
             firstInput].split('').forEach((num} => {
              cy.get('.digit').contains(num).click();
           });
         
           secondInput.split('').forEach((num) => {
            cy.get('.digit').contains(num).click();
            });
           } 
           operateTime([1, 2], 1, 2) 
           ```
          
    // 예시2)      
    function clickGroupButton() {
     const confirmButton = document.getElementsByTagName('button')[0];
     const cancelButton = document.getElementsByTagName('button')[1];
     const resetButton = document.getElementsByTagName('button')[2];
     }
     
    // 수정
     function clickGroupButton() {
     const [confirmButton, cancelButton, resetButton] = document.getElementsByTagName('button');
     }
    
   구조 분해 할당 사용
     
   - 유사 배열 객체 <br>
     ```
     const arrayLikeObject = {
      0 : 'HELLO',
      1: 'WORLD',
      length : 2,
     };
     
     const arr = Array.from(arrayLikeObject);
     
     console.log(Array.isArray(arrayLikeObject)); // false
     console.log(Array.isArray(arr)); // true
     ```
     
     ```
     function generatePriceList() {
       for (let i = 0; index < arguments.length; index++) {
        const element = arguments[index];
        // arguments는 유사배열객체. arguments 값이 없어도 배열처럼 처리 됨
        console.log(element); // 100, 200, 300, 400, 500, 600
     }
     
     generatePriceList(100, 200, 300, 400, 500, 600);
     ```
     
   - 불변성 (immutable) <br>

     ```
     const originArray = ['123', '456', '789'];
     const newArray = originArray;
     
     originArray.push(10);
     originArray.push(11);
     originArray.push(12);
     originArray.unshift(0);
     
     console.log(originArray); // [ 0, '123', '456', '789', 10, 11, 12 ]
     console.log(newArray); // [ 0, '123', '456', '789', 10, 11, 12 ]
     ```
     결론 
     ```
     1. 배열을 복사한다.
     2. 새로운 배열을 반환하는 메서드들을 활용한다. (map, filter, slice ..etc)
     ```
      
   - for문 배열 고차 함수로 리팩터링 <br>
     
     예시1)
     ```
     /* 배열 고차 함수
      *
      * 1. 원화 표기
      */
     const price = ['2000', '1000', '3000', '5000', '4000'];
     
     function getWonPrice(priceList) {
      let temp = [];
      
      for (let i = 0; i < priceList.length; i++) {
        temp.push(priceList[i] + '원');
       }
       return temp;
      }
     ```
     예시1 리팩터링)
     ```
     const price = ['2000', '1000', '3000', '5000', '4000'];
     
     function getWonPrice(priceList) {
       return priceList.map((price) => price + '원'):
      }
     ```
     
     예시1-1 조건 추가)
     ```
     /* 배열 고차 함수
      *
      * 1. 원화 표기
      * 2. 1000원 초과 리스트만 출력
      * 3. 가격 순 정렬
      */
     const price = ['2000', '1000', '3000', '5000', '4000'];
     const suffixWon = (price) +> price + '원';
     
     function getWonPrice(priceList) {
       for (let i = 0; i < priceList.length; i++) {
       if ( priceList[i] > 1000) {
         temp.push(priceList[i] + '원');
        }
       }
       return temp;
      }
     ```
          
     예시1-1 리팩터링)
     ```
      const price = ['2000', '1000', '3000', '5000', '4000'];
      const suffixWon = (price) +> price + '원';
      const isOverOneThousand = (price) => Number(price)  > 1000;
      function getWonPrice(priceList) {
       const isOverList = priceList.filter(isOverOneThousand);
       
       return isOverList.map(suffixWon);
      }
      const result = getWonPrice(price)
      console.log(result);
     ```
     
   - 배열 메서드 체이닝 활용하기 <br>
      
      예시)
      ```
      const price = ['2000', '1000', '3000', '5000', '4000'];
      
      const suffixWon = (price) +> price + '원';
      const isOverOneThousand = (price) => Number(price)  > 1000;
      const ascendingList = (a, b) => a - b;
      
      function getWonPrice(priceList) {
       const isOverList = priceList.filter(isOverOneThousand);
       const sortList = isOverList.sort(ascendingList);
       
       return sortList.map(suffixWon);
      }
      const result = getWonPrice(price);
      console.log(result);
      ```
      
      리팩터링)
      ```
      const price = ['2000', '1000', '3000', '5000', '4000'];
      
      const suffixWon = (price) +> price + '원';
      const isOverOneThousand = (price) => Number(price)  > 1000;
      const ascendingList = (a, b) => a - b;
      
      function getWonPrice(priceList) {
       return priceList
       .filter(isOverOneThousand) // filter 원하는 조건에 맞는 배열 리스트 만들기
       .sort(ascendingList) // sort 정렬
       .map(suffixWon); // map 배열 요소들을 다시 정리
      }
      const result = getWonPrice(price);
      console.log(result);
      ```
      
   - map vs forEach <br>
     [forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) <br>
     [map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map) <br>
     ```
     const price = ['1000', '2000', '3000'];
     
     const newPricesForEach = prices.forEach(function (price) {
       return price + '원';
     });
     
     const newPricesMap = prices.map((function (price) {
       return price + '원';
     });
     
     console.log(newPricesForEach); // undefined 
     console.log(newPricesMap); // [1000원', 2000원', '3000원'] 
     ```
     
     forEach는 배열을 순회하면서 배열 요소마다 또다른 함수를 실행시켜주는 역할  => 요소가 루프될 때마다 함수를 실행 <br>
     map은 원본 배열을 조작하지만, 손상하지 않고 원본 배열을 그대로 두고 새로운 배열 생성 <br>
     
   - Continue & Break <br>
     
     ```
     const orders = ['first', 'seconde', 'third'];
     orders.forEach(function(order) {
      if (order === 'second') {
        break;
      }
      console.log(order); // SyntaxError : break 사용 불가
     });
     ```
     
     ```
     const orders = ['first', 'seconde', 'third'];
     try {
       orders.forEach(function(order) {
         if (order === 'second') {
           throw error
         }
         console.log(order); // SyntaxError : break 사용 불가
     });
     } catch (e) {
     }
     
     // for in, for of 사용
     ```

     reduce, map과 같은 배열 고차 함수들은 흐름을 제어하기가 어렵다.
     ![image](https://user-images.githubusercontent.com/26318691/149548180-7269a38e-32f5-492f-897a-4add14fc02cc.png)

     <hr/>

 6. 객체 <br>
  - Shorthand Properties <br>
    css 예시)
     ```
     .before {
      background-color: #000;
      background-image : url(images/bg.png);
      
      margin-top : 10px;
      margin-right: 5px;
      margin-bottom: 10px;
      margin-left: 5px;
      }
     ```
     ```
     .before {
      background:  #000 url(images/bg.png);
      margin : 10px 5px 10px 5px;
      }
     ```
     자바스크립트 예시) 
     ```
     const firstName = 'sb';
     const lastName = 'shin';
     
     const person = {
      firstName : 'sb',
      lastName: 'shin',
      getFullName: function () {
       return this.firstName + ' ' + this.lastName;
       }
      } 
     ```
     
     ```
     const firstName = 'sb';
     const lastName = 'shin';
     
     const person = {
      firstName,
      lastName,
      getFullName () {
       return this.firstName + ' ' + this.lastName;
       }
      } 
     ```
     결론
     ```
     Shorthand Properties
     Concise Method 
     를 사용할 때 왜 축약해서 쓸 수 있는 지 동작에 대해서 이해하고 사용하자.
     ```
  - Computed Property Name <br>
  
    리덕스 예시)  
    ```
    const noop = createAction("INCREMENT'); 
    
    const reducer = handleActions(
    { 
     [noop]: (state, action) => ({ // noop도 Computed PropertyName에 속함
      counter: state.counter + action.payload,
      }),
     },
     { counter: 0 },
    );
    ```
    
  - Lookup Table <br>
    
    예시)
    
    ```
    function getUserType(type) {
     
     if (type === 'ADMIN') {
      return '관리자';
     } else if (type === 'INSTRUCTOR') {
      return '강사';
     } else if (type === 'STUDENT') {
       return '수강생';
     } else {
       return '해당 없음';
     }
    } 
    ```
   
    ```
    // lookup table 적용 
    function getUserType(type) {
    const USER_TYPE = {
     ADMIN: '관리자',
     INSTRUCTOR: '강사',
     STUDENT: '수강생',
     UNDEFINED: '해당 없음'
     };
     return USER_TYPE[type] ?? USER_TYPE[UNDEFINED];
    }
    getUserType('STUDENT'); // 학생
    ```
    Computed Property Name을 활용하여, 불필요한 분기문을 줄일 수 있음.
    
  - Object Destructuring <br>
    ```
    function Person(name, age, location) {
     this.name = name;
     this.age = age ?? 30;  // 넣지 않는 값에 대해 처리
     this.location = location ?? 'korea'; // 넣지 않는 값에 대해 처리
     }
     
     const sb = new Person('sb', 30, 'korea);
    ```
    
    ```
    function Person(name, age, location) {
     this.name = name;
     this.age = age;
     this.location = location;
     }
     
     const sb = new Person({
     name : 'sb', 
     age: 30, 
     location: 'korea'
     });
    ```
    
    ```
    const orders = ['First', 'Second', 'Third'];
    
    const st = orders[0];
    const rd = orders[2];
    
    const [first, thired] = orders
    ```
    
    ```
    const orders = ['First', 'Second', 'Third'];
    const {0: st2, 2: rd2} = orders
    
    const st = orders[0];
    const rd = orders[2];
    
    const [st3, , rd3] = orders
    ```
    
  - Object.freeze <br>
    [참고](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
    ```
    const STAUS = Object.freeze{{
     PENDING: 'PENDING',
     SUCCESS: 'SUCCESS',
     FAIL: 'FAIL',
    });
    
    STAUS.PENDING = 'P2'
    console.log(STATUS); // { PENDING : 'PENDING', SUCCESS : 'SUCCESS', FAIL: 'FAIL' }
    console.log(STAUS.PENDING); // PENDING
    // 객체가 잘 동결이 되었는 지 확인
    Object.isFrozen(STATUS) // true
    Object.isFrozen(STATUS.PENDING) // true
    ```
    ```
    const STAUS = Object.freeze{{
     PENDING: 'PENDING',
     SUCCESS: 'SUCCESS',
     FAIL: 'FAIL',
     OPTIONS: {
      GREEN: 'GREEN',
      RED: 'RED',
      },
    });
    
    Object.isFrozen(Object.OPTIONS); // true
    Object.isFrozen(STAUS.OPTIONS); //false
    
    STAUS.OPTIONS.GREEN = 'G'
    STAUS.OPTIONS.YELLOW = 'Y'
    
    console.log(STATUS.OPTIONS); // { GREEN: 'G', RED: 'RED' }
    ```
    
    객체의 깊은 영역까지는 freeze가 관여할 수 없음.
    
    해결책
    
    ```
    1. 대중적인 유틸 라이브러리
    2. 직접 deepFreeze 유틸 함수 생성
       1. 객체를 순회
       2. 값이 객체인지 확인
       3. 객체이면 재귀
       4. 그렇지 않으면 Object.freeze
    3. 스택오버 플로우
    4. Typescript에 readonly 속성 사용
    ```

  - Prototype 조작 지양하기 <br>
       
    예시) <br>
    ```
    // 기존에 자바스크립트에 내장된 내용을 변경함
    Array.prototype.forEach = function() {
     return 'hello';
    }
    ```
    
    결론
    ```
    1. 이미 JS는 많이 발전했다.
      1-1. 직접 만들어서 모듈화
      1-2. 직접 만든 모듈을 배포 => NPM
    2. JS 빌트인 객체를 건드리지 말자
    ```

  - hasOwnProperty <br>
    
    ```
    const person = {
     name: 'sbshin'
    }
    
    person.hasOwnProperty('name'); // true
    person.hasOwnProperty('age'); // false
    ```
    ![image](https://user-images.githubusercontent.com/26318691/149955453-1b63cfaa-eb58-463d-81a1-866c5f295baf.png)
    
    ```
    const foo = {
     hasOwnProperty: function() {
      return 'hasOwnProperty'; 
     },
     bar: 'strubg',
    };

    foo.hasOwnProperty('bar'); // hasOwnProperty
    Object.prototype.hasOwnProperty.call(foo, 'bar') // true
    ```

  - 직접 접근 지양하기 <br>
    예시)
    ```
    const model = {
     isLogin: false,
     isValidToken: false,
     };
     
     // login, logout 모두 객체에 직접 접근하고 있음
     function login() {
      model.isLogin = true;
      model.isValidToken = true;
      }
      
      function logout() {
       model.isLogin = false;
       model.isValidToken = false;
      }
     
     someElement.addEventListener('click', login);  
    ```
    
    수정)
    ```
    const model = {
     isLogin: false,
     isValidToken: false,
     };
     
     // model에 대신 접근
     function setLogin(bool) {
      model.isLogin = bool
      }
      
     // model에 대신 접근 
     function setValidToken(bool) {
      model.isValidToken = bool
      }
      
     // model에 직접 접근 x
      function Login() {
       setLogin(true);
       setValidToken(true);
      }
     
     // model에 직접 접근 x 
      function Logout() {
       setLogin(false);
       setValidToken(false);
      }
     
     someElement.addEventListener('click', login);  
    ```
    
    결론
    ```
    1. 직접 접근 지양하기
    2. 예측 가능한 코드 작성해서 동작이 예츠 가능한 앱 생성
    ```
    
  - Optional Chaining <br>
    강의 준비중
  - Extends & Mixin <br>
    강의 준비중
    
 <hr/>
 
8. 함수 
  - 함수/메서드/생성자 <br>
    
    ```
    // 함수
     function func() {
      return this;
     }
     
     // 객체의 메서드
     const obj = {
       method() {
        return this;
        },
        conciseMethod: function () {
          return this;
        },
      };
     
     // 생성자 함수 (Class가 있기 때문에 굳이 쓸 필요는 없음)
     function Func() {
       return this;
     }
     
     class Func {
       constructor() {
       }
     }
     
     /* 함수
      * 1급 객체
      * - 변수나, 데이터에 담길 수 있다.
      * - 매개변수로 전달 가능 (콜백 함수)
      * - 함수가 함수를 반환 (고차 함수)
     */
     func();
     
     // 메서드 => 객체에 의존성이 있는 함수, OOP 행동을 의미
     obj.method();
     
     // 생성자 함수 => 인스턴스를 생성하는 역할 => Class로 대체 가능
     const instance = new Func();
    ```
    
  - argument & parameter <br>

    [참고](https://developer.mozilla.org/en-US/docs/Glossary/Parameter)
    - 함수의 parameter는 함수의 정의 부분에 나열된 이름이다. 
    - 함수의 arguments는 함수에 들어가는 real value이다.
    
    ```
    /**
     *  Parameter (Formal Parameter)
     *  형식을 갖춘 매개 변수
     */
     
     function axios(url) {
      // something..
     }
     
     /** Argument (Actual Parameter)
       * 실제로 사용되는 인자
       *
       */
     axios('https://github.com');
     
    ```
   
  - 복잡한 인자 관리하기 <br>
    
    올바른 코드 예시)
    ```
    function toggleDisplay(isToggle) {
     // ...some code
    }
    
    function sum(sum1, sum2) {
     // ...some code
    }
    
    function timer(start, stop, end) {
     // ...some code
    }
    
    function timer(top, right, bottom, left) {
     // ...some code
    } 
    ```
    
    개선해야 될 코드 예시)
    
    ```
    function createCar(name, brand, color, type) {
     return {
      name,
      brand,
      color,
      type,
     };
    }
    ```
    
    개선)
    
    ```
    function createCar(name, { brand, color, type }) {
     return {
      name,
      brand,
      color,
      type,
     };
    }
    
    createCar('차 이름', {
     type: '타입',
     color: '블랙',
     brand: '브랜드'
    });
    ```
    
    인자 관리하기)
    
    ```
    function createCar(name, { brand, color, type }) {
     if (!name) {
      throw new Error('name is a required');
      }
      
     if (!brand) {
      throw new Error('brand is a required');
      }
    }  
    ```
    
  - Default Value <br>
    
    ```
    function createCarousel(options) {
     options = optins || {};
     const margin = options.margin || 0;
     const center = options.center || false;
     const navElement = options.navElement || 'div';
     
     return {
      margin,
      center,
      navElement
     };
    }
    
    createCarousel(); // { margin = 0, center = false, navElement = 'div' }
    ```
    
    ```
    function createCarousel({ margin = 0, center = false, navElement = 'div' } = {}) {
     return {
      margin,
      center,
      navElement
     };
    }
    createCarousel(); // { margin = 0, center = false, navElement = 'div' }
    ```
    
    ```
    const required = (argName) +> {
     throw new Error('required is ' + argName);
    };
    
    function createCarousel(
     items = required('items'),
     margin = 0,
     center = false,
     navElement = 'div'
    } = {}) {
     
     return {
      margin,
      center,
      navElement
     };
    }
    
    console.log(
     createCarousel({
       center: true,
       navElement: 'ul'
      });
     ); // required is items
    ```
    
  - Rest Parameters <br>
    
    예시)
    ```
    function sumTotal() {
     Array.isArray(arguments); // false
     return Array.from(arguments).reduce(
       (acc, curr) => acc + curr,
      );
     }
     
     sumTotal(1, 2, 3, 4, 5, 6, 7, 8, 9, 10); // 55
    ```
    
    코드 개선)
    ```
    function sumTotal(...args) {
     Array.isArray(args); // true
     
     return args.reduce(
       (acc, curr) => acc + curr,
      );
     }
     
     sumTotal(1, 2, 3, 4, 5, 6, 7, 8, 9, 10); // 55
    ```
    
    코드 개선2)
    ```
    function sumTotal(
      initValue,
      bonusValue,
      ...args, // * 나머지 매개변수는 가장 마지막에 들어가야 함
     ) {
      console.log(initValue); // 100
      console.log(bonusValue); // 99
      
     return args.reduce(
       (acc, curr) => acc + curr,
       initValue
      );
     }
     
     sumTotal(100, 99, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10); // 55
    ```

  - void & return <br>
    예시1)
    ```
    function handleClick() {
      return setState(false); // void 함수인데 리턴 값을 지정
     }
     
    function showAlert(message) {
      return alert(message); // void 함수인데 리턴 값을 지정
     }
    ```
    예시1 수정)
     ```
    function handleClick() {
       setState(false);
     }
     
    function showAlert(message) {
      alert(message);
     }
    ```
    
    예시2)
    ```
    function testVoidFunc() {
      const arr = [1, 2];
      return arr.push(10));
     }
    
    testVoidFunc() // 3 (length 반환)
    ```
    
    예시3)
    ```
    // 반환형이 있는 케이스
     function isAdult(age) {
      return age > 19;
     }
     function getUserName(name) {
      return '유저 ' + name;
     }
     
     const isFlag = isAdult(10); // false
    ```
    
  - 화살표 함수 <br>

    예시1)
    ```
    const user = {
     name: 'sb',
     getName: () => {
      return this.name'
      },
     };
     
     user.getName(); // undefined (화살표 함수로는 name에 접근 x)
    ```
    ```
    const user = {
     name: 'sb',
     getName() {
      return this.name'
      },
     };
     
     user.getName(); // sb
    ```
    예시2)
    ```
    const user = {
     name: 'sb',
     getName: () => {
      return this.name'
      },
      newFriends: () => {
      // call, apply, bind 사용 불가
       const newFriendList = Array.from(arguments); // arguments 사용 x
      return this.name + newFriendList;
      },
     };
     // user.newFriends('shin', '신')
    ```
    
    ```
    // 수정
    const user = {
     name: 'sb',
     getName: () => {
      return this.name'
      },
      newFriends: (...rest) => {
      return this.name + newFriendList;
      },
     };
     // user.newFriends('shin', '신')
    ```
    
    예시3)
    ```
     const Person = (name, city) => {
      this.name = name;
      this.city = city;
      };
      
      const person = new Person('sb', 'korea'); // Person is not a constructor 
     ```
     
     ```
     const Person = (name, city) => {
      this.name = name;
      this.city = city;
      };
      
      const person = new Person('sb', 'korea'); // Person is not a constructor 
     ```
    
    예시4)
    ```
    class Parent {
     parentMethod() {
      console.log('parentMethod');
     }
     
     parentMethodArrow = () => {
      console.log('parentmethodArrow');
     }
     
     overrideMethod = () {
      return 'Parent';
     }
     
     // overrideMethod = () => {
      return 'Parent';
     }
     
    }
    
    class Child extends Parent {
     childMethod() {
      super.parentMethod(); 
      // super.parentMethodArrow();
      }
      
      overrideMethod() {
       return 'Child';
      }
    }
    
    new Child().childMethod(); // 정상적으로 호출됨
    //  new Child().childMethod(); 에러 발생!
    // new Child().overrideMethod(); // Parent 출력
    ```
    
    ```
    function* gen() {
     yield () =>  // 사용 불가
    }
    ```
    
    결론
    ```
    1. this 조작법 주의. 화살표 함수는 렉시컬 스코프를 가지므로, 상위의 스코프를 따르게 됨
    2. arguments 객체 사용 불가.
    3. 생성자로 사용할 수 없고 오버라이딩할 수 없다.
    4. yield 키워드 사용 불가
    ```
    
  - Callback Function <br>

    콜백함수는 함수를 다른 함수에 넘겨서 제어권을 위임할 수 있다. <br>
    
    예시)
    ```
    function register() {
     const isConfirm = confirm(
      '회원가입에 성공했습니다.',
     );
     
     if (isConfirm) {
       redirectUserInfoPage();
     }
    }
    
    function login() {
     const isConfirm = alert(
      '로그인에 성공했습니다.',
      );
      
      if (isConfirm) {
        redirectIndexPage();
       }
     } 
    ```
    
    ```
    function confirmModal(message, cbFunc) {
     const isConfirm = confirm(message);
      if (isConfirm && cbFunc) {
       cbFunc();
      }
    }
    
    function register() {
     confirmModal('회원가입에 성공했습니다.', redirectUserInfoPage);  // 함수를 실행시키지 않고 넘겨야 함(redirectUserInfoPage() => X)
    }
    
    function login() {
     confirmModal('로그인에 성공했습니다.',redirectIndexPage);
     } 
    ```

  - 순수 함수 <br>
    
    예시)
    ```
    // 순수함수가 아닌 예
    let num1 = 10;
    let num2 = 20;
    
    function impureSum1() {
     return num1 + num2;
    }
    
    function impureSum2(newNum) {
     return num1 + newNum;
    }
    impureSum1(); // 30
    impureSum1(); // 30
    
    num1 = 30;
    impureSum1(); // 50 (호출할 때마다 일관적인 값을 뱉어야하는데, 일관성이 없음. 비순수함수)
    
    impureSum2(30); //40
    
    num1 = 100;
    
    impureSum2(30); //130     
    ```
    
    ```
    // 순수함수로 변경
    
    let num1 = 10;
    let num2 = 20;
    
    function pureSum(num1, num2) {
     return num1 + num2;
    }
    
    pureSum(10, 20); // 30
    pureSum(130, 100); // 130
    ```
    
    ```
    const obj = { one: 1 };
    
    // 객체,배열은 refrenceType의 값이기때문에, 새롭게 만들어서 반환
    function changeObj(targetObj) {
     targetObj.one = 100;
     
     return targetObj;
     }
     changeObj(obj); // { one : 100 }
     console.log(obj); // { one : 100 } 
    ```
    
    ```
    const obj = { one: 1 };
    function changeObj(targetObj) {
     return { ...targetObj, one: 100 }; // { one: 1 }
     }
     changeObj(obj); // { one : 100 }
     console.log(obj); // { one : 1 } 
    ```
    
  - Closure <br>
    
  - 고차함수 <br>
    강의 준비중
  - Currying <br>
    강의 준비중
