# 1주차 과제 정리

## let vs var vs const

* var,let 은 선언시 자동으로  __undefined__ 값이 할당 되어서 초기화가가 이루어 진다.
  ```javascript
  var a;
  let b;
  console.log(a,b); // 'undefined' 출력
  ```

* const 는 자동으로 초기화를 해주지 않으므로 __선언시 할당까지__ 꼭 해주어야 한다.
   ```javascript
   const a;
   console.log(a); // 오류 발생
   ```
* let, const 는 [호이스팅](https://developer.mozilla.org/ko/docs/Glossary/Hoisting) 발생 시 선언만 호이스팅 되어서 초기화가 일어나지 않아 __[TDZ](https://developer.mozilla.org/ko/docs/Glossary/DMZ)__ 상태가 된다.
    ```javascript
    console.log(a);
    let a = 1;  // 오류가 발생
    ```
    ```javascript
    console.log(a);
    const b = 1; //역시 오류 발생
    ```
* var 은 호이스팅이 발생해도 초기화 까지 진행 되어서 DMZ 상태가 되지 않는다.
    ```javascript
    console.log(a);
    var = 1;  // 'undefined' 출력
    ``` 
* var 은 __재할당, 재선언 모두 가능__ 하고 let __재선언 할 수 없으며__ const 는 __둘 다 할수없다.__
    ```javascript
    var a = 1;
    var a = 2;
    a = 3; // 가능
   ``` 
   ```javascript
   let a = 1;
   a = 2; // 재할당 가능
   let a = 3; //재선언 불가능
   ```

 * 추가로 var 는 윈도우 객체에 생성이 된고 , let 은 생성이 되지 않는다 (함수도 저장)

 * var 는 __함수 스코프 범위__ 를 가지고 let 은 __블록 스코프 범위__ 를 가진다.
     ```javascript
     function example(){
       for(i = 0; i <= 5; i++){
        if(i === 1){
            var a = 'hi';
            let b = 'hello';
        }
       }
       console.log(a); // 'hi' 출력
       console.log(b); // 오류 출력 x
     }
     console.log(a); // 오류 출력 X
     ```
* var 의 특징 중 전역변수로 보이는 변수는 함수 안에 있어도 전역변수로 만들어준다
     ```javascript
    function example(){
      z = 'hi';
    }
    example();
    console.log(z); // 'hi' 출력 
     ```
  > let , const , var 없이 선언하면 자동으로 var로 선언이 되고 전역변수로 만들어주는 특성이다

## 여러 반복문들과 break, continue

### 기본 for 문
   ```javascript
   for ([initialization]; [condition]; [final-expression]){
        statement
   }
   ```
   * initialization : 식 또는 변수 선언, 카운터 변수를 초기화 할때 사용
   * condition : 조건식이 들어가는 부분으로 참이면 반복문을 계속 실행한다
   * statement : condition 부분이 참일때 수행할 명령
   * final-expressin : statement 실행 후 codition 평가하기 이전에 실행 되는 문 ( 주로 카운터 변수를 증감 시킬 때 사용하고 ',' 로 여러개의 동작을 실행 할 수 있다.)

* 각각의 initialization, condition, final-expression 부분은 생략 될 수 있다
  ```javascript
  [initialization]
  
  for(;;){
    if([condition]){
      statement;
    }
    [final-expression]
  }

### 다른 형태에 for in 문
* 기본 형태
  ```javascript
  for (const variable in object) {
  statement;
  }
  ```
  * variale : 각각의 property의 name 값이 저장 된다
  * object : 객체를 담고 있는 변수명
  * statement: 수행되는 명령문

* for in enumerable string properties 에 관해서만 사용 할 수 있다 

  *  모든 자바스크립트 객체는 3개의 특성으로 분류 할 수 있는데 각각

    * Enumerable or non-enumerable --> Enumerable 속성 역시 iterable 속성 처럼 내장 프로퍼티에 저장 되므로 유무를 통해 판단 할 수 있다

    * String or symbol ---> 문자열인지 symbol 인지

    * Own property or inherited property from the prototype chain ---> 자기 자신의 프로퍼티 인지 상속 받은 프로퍼티 인지


* 객체의 property 를 순환하며 출력으로는 property 값이 나온다

* 출력 되어서 나오는 순서가 무작위 여서 반복문에서는 사용 되지 않는다

### 또 다른 형태 for of 문

* 기본 형태
  ```javascript
  for (const variable of iterable) {
    statement;
  }
  ```
  * variable 객체 property 에서 value 값이 매 변수로 저장 돼서 할된다

  * iterable : 순환가능한 객체에 대해서만 사용 할 수 있다

  * statement 수행되어야 할 명령

* __iterable. 순환가능 객체라는 것은__ Symbol.iterator 를 통해 @@iterator 키가 있는 것을 뜻한다(보통 Array, Map, Set, String, TypedArray, arguments 등이 있다)

* console.dir() ----> 객체의 모든 속성을 볼 수 있게 해준다

* 임의로 iterable 속성을 만들고 싶을때
  ```js
  넣고싶은 이름[Symbol.iterator] = function() {
    return {
      current: this.from,
      last: this.to,
      next() {
        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      }
    };
  };
  ```
  위에 코드를 넣어주면 된다!

### for in 과 for of 에서 key 와 value 를 선언할때 let 대신 const 를 쓰는 이유

* for of 와 for in 이 실행 되는 과정에서 한번 싸이클을 돌때 독립적인 valuable Environmnet 를 가지므로 const 로 할당 해 줘도 아무런 문제가 발생하지 않는 것입니다.

  ```js
  const arr = ['가' ,'나', '다'];    
  for (const value of arr ){        //  출력은 독립적인 스코프로 3번 실행
    console.log(value);             // {'가'},{'나'},{'다'} 실제로 {} 로 스코프 된다는 것은 아님
  } 
  ```

### break 와 continue

* break 문은 현재 수행 중인 반복문, switch 문, 또는 label 문을 종료하고, 그 다음 코드로 넘겨주는 역할을 해줌

* continue 문은 현재 실행 중인 명령을 종료하고 반복문의 처음으로 돌아가서 루프문의 다음 코드를 실행합니다.



## if...else 문, Ternary operator(삼항 연산자), Switch 문

### if...else

* 기본 형태
  ```javascript
    if (condition) {
       statement1
    } else {
       statement2
      }
  ```
  * condition 이 truthy 일 경우 statement1 을 실행하고 falsy 일 경우 statement2 를 실행 시켜준다.
  
* 여러번 중첩해서 사용 가능 하다

* 주의 해야 할 경우
   ```javascript
   const b = new Boolean(false);
   if (b) {
  console.log("b is truthy");   // b 가 truthy 여서 'b is truth' 가 출력 됨 
  }
  ```
   * 이 경우 new 연산자의 역할이 새로운 객체를 만들어주는 역할을 하는데 뒤에 오는 Boolean(false)를 property 로 받아 하나의 객체가 되어서 b 가 truthy 로 되는 것이다

 * if 조건문 안에서 할당표현식을 쓸 때는 ()로 한번더 묶어주어야 한다
   ```javascript
   let x = 1;
   let y = 2;
   if ((x = y) && x === 2){
    console.log('hi')        //'hi' 가 출력되어 나온다
   }
   ```

### Ternary operator(삼항 연산자) : ?

* 기본형태
  ```javascript
  condition ? exprIfTrue : exprIfFalse
  ```
  * codition : 조건식이 오고 truthy 와 falsy 를 반환 해준다

  * exprIfTrue : 조건식이 truthy 일 경우 실행되는 명령

  * exprIfFalse : 조건식이 falsy 일 경우 실행되는 명령

* 삼항 연산자를 변수에 할당 수 있어서 null 값 등 처리에 용이하다
  ```javascript
  const name = person ? person.name : "stranger";
  console.log(`Howdy, ${name}`);
  ```
* 삼항 연산자 역시 중첩해서 사용가능하다!

### switch 문

* 기본형태
  ```javascript
  switch (expression) {
  case caseExpression1:
    statements
  case caseExpression2:
    statements
  // …
  case caseExpressionN:
    statements
  default:
    statements
  }
  ```
  * expression : 기준이 돼서 각각의 caseExpression 값들과 비교 될 대상을 나타낸다

  * caseExpression : expression 과 비교해서 일치 하면 다음 명령문을 수행하고 불일치 하면 다음 case 문으로 넘겨준다

  * default : 아무런 case 문들과 일치하지 않았을 때 실행해줄 명령을 나타낸다

* fall-through : 명령문 안에 break 를 넣어주지 않아 멈추지 않고 끝까지 명령문들이 실행 되는 것을 일컫는다

* 풀쓰루(fall-through)가 발생하면 더 이상 expression 과 caseExpression 을 비교하지 않고 __명령문들만 수행__ 하면서 코드가 실행된다
  ```javascript
  switch(1){
  case 1:
    console.log('a');
  case 2:
    console.log('b');
  case 3:
    console.log('c');
  default:
    console.log('d');       // 'a' 'b' 'c' 'd' 전부 출력
  }
  ```

* 풀쓰루(fall-through)를 이용해서 여럭 case 문들을 이어 붙일 수 있다


## function, 화살표 함수, Rest parameter, Default parameter
### function 

* 기본구조
  ```javascript
  function 함수이름(parameter)[
    수행할 코드
  ]
  ```

* return 으로 반환값을 명시해주지 않으면 undefined 가 출력된다.

* 선언문 함수 와 표현식 함수가 있다
 
  * 선언문 함수는 만들어주자마자 선언과 초기화 까지 한번에 이루어 지고 

  * 표현식 함수는 let, var , const 로 어떤 변수에 할당 되므로 그 특성을 따른다

  * 표현식 함수는 일급 객체의 성질을 가진다(다른 일반 객체들에 적용 가능한 연산을 모두 지원하는 객체를 말함)

    * 조건

    1. 변수나 자료구조에 저장 할 수 있다.

    2. 함수의 매개변수에 전달할 수 있다.

    3. 함수의 반환값으로 사용 할 수 있다.

    4. 무명의 리터럴로 생성 할 수 있다.   ----> 🧐 조사 필요   

* 함수 내부에서 선언된 변수를 __지역변수__ 라 하고 외부에 있는 변수에 접근 할 수 있다
  ```javascript
  let a = 1;
  function hi(){
    a = 2;
  };
  hi();      // 만들고 호출을 꼭 해주어야 한다!
  console.log(a);  // 2가 출력 된다
  ```

* 함수 내부에서 외부 변수를 재선언 하면 외부변수를 참조한게 아닌 지역변수를 새로 선언한것으로 간주한다
  ```javascript
  let a = 1;
  function hi(){
    let a = 2;
    console.log(a);   // 함수 안에선 2 출력
   };
  hi();     
  console.log(a);  // 1이 출력 된다
  ```

* parameter vs arguments
 
  * parameter(파라미터) : 함수 만들때 ()안에 쓰이는 것으로 매개변수를 칭한다

  * arguments(인수) : 호출된 함수에서 parameter 에 전달되는 값으로 보통 사용잦가 넣는 값을 일컫는다

* parameter 에서 기본값을 설정 할 수 있다(값을 주지 않았을 때 undefined 대신 넣어 줄 값을 정함)
  ```javascript
  function sum(a,b = 1){
    return a + b;
  }
  sum(5) // 6이 출력 돼서 나온다 b 값을 주지 않아도 자동 1대입
  ```

  * parameter = ~~ 형태로 넣어주면 된다
  
  * 다른 방식으로 함수식에 if 문으로 써주어도 되고 || 연산자로 설정 해 줘도 된다



* return 은 함수내에서 break 역할도 해서 코드 중간에 써주면 즉시 함수를 빠져나온다
 
  ```javascript
  function hi(){
    for(let i = 0; i <= 5; i++){
      console.log('hi');
      if(i === 3){
        return;
      }
    }
  }
  hi(); // 'hi' 가 4번 출력되고 함수가 종료된다 
  ```

* return 으로 반환 값을 넣어줄떄 같은 코드 줄에 있거나 () 묶어서 줄바꿔주어야 한다

  ```javascript
  function hi(){
    return (
      console.log('hi'),  // , 로 이어주어야 한다
      console.log('hello');
    )
  }
  hi();  // 'hi' 'hello' 둘다 출력
  ```
* 함수 이름 지어주기

  * show로 시작하는 함수 - 대개 무언가를 보여주는 함수입니다.

  * "get…" – 값을 반환함

  * "create…" – 무언가를 생성함

  * "create…" – 무언가를 생성함

* 일반적으로 함수 하나에는 한가지 기능만 담겨있어야 한다

* 함수객체에서는 다음과 같은 속성을 가지고 있을 수 있다

  * arguments 객체

  * caller 프로퍼티

  * length 프로퍼티

  * name 프로퍼티

  * __proto__접근자 프로퍼티

  * prototype 프로퍼티         ----> 추후 조사 필요 🧐



### 화살표 함수

* 기본형태
  ```javascript
  (param1, paramN) => {
  statements
  }
  ```
* 표현식으로 변수명 줄 때는 let, const 를 이용해서 선언을 해주어야한다

* 화살표 함수에선 statements 부분이 짧아지면 블록을 생략하고 쓸 수있고

    * 블록을 생략 했을 경우 식이 간단할 경우 암묵적으로 반환값으로 만들어준다(명령 부분을 블록문 안에 쓰고 싶을 경우엔 반환값이 있을 경우 __꼭 return__ 을 줘야한다 )
  
* 화살표 함수에서 __this 바인딩__ 과 __arguments__ 없기 떄문에 __클로져__ 에 의해 상위스코프(렉시컬환경)를 참조하게 된다.
    ```jsx
    let obj = {
      name: 'home',
      variable : () => {
        this.name = 'vaco'; 
      }
    }
    obj.variable();                // ----질문 후 정리
    console.log(obj.name)     
    ```
    this 바인딩, arguments 객체, 클로져 는 잠시 있다가 설명

* 화살표 함수는 생성자로 사용 할 수 없습니다. -> 추후에 추가

* 화살표 함수에서 parameter 와 '=>' 사이 줄바꿈이 불가능 함

  ```js
  const hi = (a, b)
  =>{return a + b};        //오류가 발생 한다
  ```
  
* 화살표 우선 순위가 낮다
  
  ```js
  let hi;
  
  hi = callback || () => {};   // 의도치 않은 결과가 나올 수 있음
  ```

* 일반함수에선 파라미터에 변수이름을 중복으로 사용 할 수 있지만 (화살표 함수에서는 사용 할 수 없다)
  * 일반함수에서 사용 할 경우 뒤에 매개변수만 인식 돼서 2번 사용 된다.

## this 바인딩, arguments 객체, 클로져

### this 바인딩
* this 바인딩은 호출 되는 상황에 따라 달라지는데 

  1. 일반 호출 함수의 경우
      ```javascript
      function A(){
        console.log(this);  // 이경우는 this 바인딩은 window를 가르킴
      }
      ```
  2. 메서드 호출 할때
        ```javascript
        const obj = {
        value:~~
        function x(){
            console.log(this); // this 바인딩은 obj를 가르킴 (상위 객체)
        }
        }
        ```
    3. 생성자 함수 호출 할떼
        ```jsx
        function MakeObj(place){
          this.whereAmI = place
        }
        const obj = new MakeObj('vaco');
        console.log(obj.whereAmI) // 'vaco' 출력
        ```
    4. `.apply()` `.call()` `.bind()` 메서드에 의한 간접 호출

        ```jsx
        function hi(a,b){
          this.name = a + b;
        }
        obj = {
          name : 'null'
        }
        hi.apply(obj,['va','co']);
        console.log(obj.name);    // 'vaco' 출력
        hi.call(obj,'vava','coco')
        console.log(obj.name);    // 'vacvaoco' 출력

        ```
        * bind() 같은 경우 함수를 생성해주는 메서드 이기 떄문에 직접호출 해서 바인딩 하면 안되고 변수에 담아서 그 변수를 호출해야 한다
### arguments 객체

* 정의 : 함수에 전달되는 인수들을 유사배열 형태로 나타내준다. (내장 프로퍼티로 저장)

* 위에서 정리 했듯이 화살표 함수는 arguments 가 없다 (클로져 현상 발생)
  
    ``` js
    function hi(a,b){
      const hihi = () => {
            return arguments[0] + arguments[1]; //hi 함수에서 가져옴
          }
      return hihi()
    }
    ```

* arguments 가 없는 함수에서 가공 할 수있는 3가지 방법이 있다

  1. Array.from()

  2. Array.prototype.slice.call()

  3. ...arguments --> spread operator 를 사용

  ```js
  const args = Array.prototype.slice.call(arguments);
  const args = Array.from(arguments);
  const args = [...arguments];
  ```

### 클로져

* 정의 : 함수와 렉시컬 환경(만들어 질때 상위스코프)의 조합을 의미

   * 위에서 나온 arguments, this 바인딩이 없는 함수인 경우 발생

   * 간단히 말하면 arguments, this 바인딩에 접근해야 하는데 함수에 arguments 객체나 this 바인딩이 없는 경우 상위 객체에 arguments 객체, this 바인딩을 참조는 것을 의미
  ```js
   const obj = {
      name: 'vaco',
      hihi: function hi(a,b){
              const hello = (c,d) => {
                console.log(arguments[0] + this.name);
              }
              hello();
            }
    } 
   obj.hihi('goto');  //'gotovaco' 출력~!
  ```
다시 함수로 돌아가서

### 즉시 실행 함수

* 정의 : 함수 선언과 동시에 즉시 호출되는 함수를 즉시 실행 함수라고 한다.

* 한번 즉시 실행 하면 다시 호출 할 수 없다

* 구조
  ```js
  (function (){
    ~~
  }())  
  ```
   -> 익명함수 전체를 괄호로 씌우고 맨 뒤에 () 붙여준다


### rest parameter & default parameter
* rest parameter : 정해지지 않은 parameter 를 정해주어 __배열형태__ 로 인수들을 받을 수 있다.

  ```js
  function hi(...rest){
    let sum = 0;
    for( const value of rest){
      sum += value;
    }  
    return sum; 
  }
  ```

* rest parameter 는 하나만 쓸 수 있고 마지막에만 와야한다
  ```js
   function sum (a, ...rest, b){      // 오류 발생
    ~~~
   }  
  ```
  ```js
   function sum (a, ...rest){         // 정상 작동
    ~~~
   }
  ```

  __rest parameter 이름 정해주는 것 필수!!!__<br>
  __reet parameter 진짜 배열 형태로 저장된다!!!__ 

* default parameter : 사용자가 인수를 빼 먹을 경우 기본값을 정해줄 수 있음


  ```js
  function hi(a,b = 'vaco'){
    console.log(a + b);
  }
  hi('goto'); // 'gotovaco' 출력
  ```
  * undefined 는 default 영향을 받고 (나머지 null, "" 등은 영향을 받지 않는다)

  * 함수가 호출 될 때마다 default parameter 초기화가 이루어진다
  ```js
  function x ( a,b = []){
    b.push(a);
    return b;
  }

  x(1);
  x(2);
  x(3);
  ```

## Spread operator & Nullish operator & Optional Chaning

### Spread operator ("...")

* 배열이나 문자열 등 __iterable!!!__ 한 특성을 가진 것들에서 여러개의 인수를 받고 싶을때 펼쳐서 한번에 넣을 수 있게 해준다.(*양 끝을 날려준다고 생각*)

  ```js
  const x = [1, 2, 3];

  Math.max(x); // NaN 
  Math.max(...x); // 3 출력;
  ```

* spread operator 의 여러가지 활용법

  1. apply()를 대체 할 수 있다 (가공법)
  
      ```js
      function sum(a,b,c){
        return a + b + c;
      }
      const x = [1,2,3]; 
      sum.apply(None, x);  // 기존 apply()를 이용한 가공
      sum(...x); // spreaad operator 를 이용한 가공
      ```

  2. new 에서의 생성자 호출할때 인수로 배열은 apply()통한 접근이 안되지만 spread operator 를 통한 접근은 가능하다

     ```js
     const anyDay = [2024,07,01];
     let myBirthDay = new Date(...anyDay); // Date() 날짜륾 만들어주는 생성자 함수
     ```
  3. 배열 리터럴에서 전개,복사,연결할 때

    * 전개
      ```js
      const x = ['다','라','마'];
      const y = ['가', '나', ...x,'바'];
      console.log(y); //['가','나','다','라','마','바']
      ```
    * 복사 (1차원 깊이)
      ```js
      const x = ['가','나','다'];
      const y = [...x];
      console.log(y);   //['가','나','다'] 출력 : 겉만 같음
      ```
    * 연결 
      ```js
      const x = ['가','나','다'];
      const y = ['라','마','바'];
      const z = [...x,...y];
      console.log(z)   //['가','나','다','라','마','바']
      ```
      __각각의 경우 메서드로 존재 하지만 spread operator 통해서도 가능하다.__
    
* __"..." (spread operator) vs "..." (rest parameter) 기능적으로 다르므로 꼭 주의!!!!__
  * "..." (spread operator) : 간단히 _펼친다_ 라는 의미

  * "..." (rest parameter)  : 간단히 배열로 _압축한다_ 라는 의미

### nullish operator ('??')

* 정의 : 피연사값을 비교한는데 앞에 있는 피연산자 값이 "null, undefined" 이면 뒤에 오는 피연자 값을 반환하고 앞서 오는 피연자가 "null, undefined" 아니면 앞에  오는 피연자값을 반환 해준다
  
* "??" vs "||" vs "&&" 비교
  * ?? (nullish operator) 
    ```
    여기서 값은 null 과 undefined 를 제외한 모두
    A (값)                  ?? B(값)                   ---> A (값) 반환

    A (값)                  ?? B(null or undefined)    ---> A (값) 반환    앞에 값이 있으면 뒤 보지도 않고 앞 피연자 반환 
    -----------------------------------------------------------------------------------------------------
    A (null or undefined)  ?? B(값)                    ---> B(값) 반환

    A (null or undefined)  ?? B(null or undefined)    ---> falsy 반환
    ```

  * || (Logical Or)

    ```
    A(truthy) ||  B(falsy)   --->  A(truthy) 반환    

    A(truthy) ||  B(trueth)  --->  A(truthy) 반환    앞 의 truthy 값이 오면 뒤 보지도 않고 앞 피연산자 반환
    ------------------------------------------------------------------------------------------
    A(falsy)  ||  B(truthy)  --->  B(truthy) 반환    truthy 값 반환
   
    A(falsy)  ||  B(falsy)   --->  B (falsy) 반환    둘 중에 truthy 값이 없으면 뒤 피연산자 반환
    ```

  * && (Logical And)

    ```
    A(falsy)  &&  B(truthy)  --->  A (falsy) 반환    
    
    A(falsy)  &&  B(falsy)   --->  A (falsy) 반환   앞 의 falsy 값이 오면 뒤 보지도 않고 앞 피연산자 반환
    ----------------------------------------------------------------------------------------
    A(truthy) &&  B(falsy)   --->  B (falsy) 반환   falsy 값 반환

    A(truthy) &&  B(truthy)  --->  B(truthy) 반환   둘 중에 falsy 값이 없으면 뒤 피연자자 반환
  
* nullish operator 사용 예시 🙋‍♂️ 질문

* nullish operator("??") 는 OR 연산자("||"), AND연산자와("&&") 사용 할 수없다

* nullish operator("??") vs optional chaining("?.")

  * nullish operator("??") : null, undefined 처리 할 떄 사용

  * optinal chaining("?.") : null, undefined 일 수 있는 객체의 속성에 접근 할 때 사용

### Optionalchaining ("?.")

* 중첩된 객체에서 깊숙히 있는 객체의 속성 값을 읽을 때 사용
  
  * 예시
    ```js
    let vaco = {
      ken : {
        like : {
          food : 'hamberger',
          hobby : 'fishing',
          animal : undefined 
        },
        age : 19
      },
      subo : {
        like : {
          food : 'noodel',
          hobby : undefined,
          animal : 'cat'
        },
        age : 29
      }
     }
     console.log(vaco.who.name.food);
     console.log(vaco?.ken?.like?.food);
    ```
   * optional chaining 없이 접근하면 어디서 undefined 때문에 오류가 나오는지 각각 여러번 판별해야 하는 반면
   * optional chaining 사용해서 찾으면 오류 없이 중간에 undefinded 가 있으면 undefinded 반환하기 때문에 에러를 막을 수 있다. 🙋‍♂️질문

## try..catch..finally, throw

### try..catch

* 기본 구조
  ```js
  try {
        try_statements
      }
      [catch (exception_var) {
        catch_statements
      }]
      [finally {
        finally_statements
      }]
  ```   
  * try_statements : 실행될 선언들

  * catch_statements : try문에서 오류가 발생 했을 경우 실행될 선언들

  * exception_var : catch 블럭에서의 오류 객체를 담는곳

  * finally_statements : try 선언이 완료된 이후에 실행된 선언들. 이 선언들은 __오류 발생 여부와 상관없이 실행__ 된다
  
* 위에서 말한 오류 객체는 2가지 property 를 가진다 name 과 message
  ```js
  try {
    hihi;
  } catch (e){ 
    console.log(e.name);      // 'ReferenceError' 출력    
    console.log(e.message);   // 'hihi is not defined' 출력
    console.log(e);           // error : hihi is not defined 출력
  }
  ```

* syntaxerror 에 경우에서는 처리 하지 못한다 (처리 할 수 있는 경우가 있다)

* 동기적으로 작동한다
  ```js
  try {
    setTimeout(function() {
      hello!;
    }, 1000);
  } catch (e) {
    alert( "작동 멈춤" );
  }
  ```
  * setTimeout 에 있는 익명함수는 몇 초 뒤에 실행되므로 js엔진이 이미 try...catch 문을 떠나서 잡을 수 없다(잡고 싶으면 내부에 써야 함)

### throw

* 정의 : try 문 안에 사용 돼서 catch(e) 에 parameter 에 인수로 전달해주는 역할을 하고 임의로 오류를 발생시킬 수 있다.
  ```js
  try{
    if (true){
      throw ('hihi');
    }
  } catch(e){
    console.log(e)  // 'hihi' 출력
  }
  ```
* 보통 name 과 message 둘다 넣어 줘야 된다.(객체)
  ```js
  let message = '~'   //  ~ 에 메세지 넣어주고 아니면 직접 밑에 넣어주어도 된다. 
  let error = new Error(message);  // 이름을 Error 로 해준다
  let error = new SyntaxError(message);  // 이름을 SyntaxError 로 해준다
  let error = new ReferenceError(message);  // 이름을 ReferenceError

* 에러를 다시 던질때 사용 (모든 에러를 통일해서 처리 하지 않고 나누어서 처리하고 싶을 경우 catch 문 안에서 다시 던진다)
  ```js
  function hi (){
    try {
      hihih();
    } catch(e) {
      if (!(e instanceof SyntaxError)){    // 질문 🙋🙋🙋 SyntaxError 가 발생하면 try catch 에서 처리를 못한다고 알고 있는데 저렇게 쓰는게 의미가 있나요? 
        throw e ;
      } else {
        console.log('hi');
      }
    }
  }
  try {
    hi();
  } catch {
      console.log('hello');
  }
  ```




### finally

* 정의 : 항상 실행 되는 구문으로 에러가 없으면 try 가 끝나고 실행 에러가 있으면 catch 실행이 끝난 후 실행 된다

* 형태는 try...catch....finally 순으로 들어갑니다
 
   ```js
   try {
      ... 코드를 실행 ...
    } catch(e) {
      ... 에러 핸들링 ...
    } finally {
      ... 항상 실행 ...
    }
    ```
 
 * 순서가 return 보다 빨라서 값이 반환 되기 전에 finally 구문이 먼져 실행되고 그다음 return 이 반환 됩니다
    
    ```js
    function hihi() {

      try {
        return 1000;          // -----> 2) 그 다음 return 1000 반환 

      } catch (e) {
        /* ... */
      } finally {
        console.log('hi');    // ----> 1) 먼져 'hi' 출력  
      }
    }
 * catch 를 생략하고 try....fanilly 로 에러는 처리하지 않고 시작한 프로세스가 마무리를 확실히 하고 싶은 경우에 사용
   
   ```js
   function sum(a,b){
     let result = a + b;
     try{
      a = a + 5;            
      let                 // 오류는 처리하지 않고 그대로 출력
     }
     finally {
      return result;      // 하지만 결과값은 받고 싶음
     }
   }
   ```

