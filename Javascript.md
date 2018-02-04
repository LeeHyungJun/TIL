# Javascript

자바스크립트를 학습하고 기록합니다.

## 타입

#### 기본타입

1. Number
2. String
3. Boolean
4. undefined - 변수에 값이 할당되지 않을 때 인터프리터가 ubdefined로 할당.
5. null - 개발자가 의도적으로 할당하는 값. (typeof를 사용하면 null은 object)

#### 참조타입(객체타입)

1. object
2. Array - typeof 반환값 : object
3. Funtion  - typeof 반환값 : function


## 콜백함수

자바스크립트에서는 함수도 **객체**이다. 그래서 우리는 함수 이름 혹은 익명함수를 넘겨주면서 함수를 파라미터로 넘길 수 있다.


## 논리연산자

논리연산자를 이용해서 **초기화**를 좀 더 멋진 방법으로 할 수 있다.

1. A || B 
>A를 true로 반환 가능하면 A 리턴. 아니면 B리턴.

        var total = (total || 0) + i

2. A && B
>A를 true로 반환 가능하면 B 리턴. 아니면 A리턴

        var a = 10;
        var b = 20;
        var res = a && b;
        console.log(res);  //20


## 스코프

현재 접근할 수 있는 변수들의 범위.

#### 생성

1. Function, with, catch 만 스코프를 생성한다.
2. with, catch 구문은 인자로 받는 변수들만 내부 스코프에 포함된다. 
3. For-loop은 별도의 스코프를 생성하지 않는다.
4. {} 블록은 스코프를 생성하지 않는다.

#### 지속성

>함수가 선언된 곳이 아닌 전혀 다른 곳에서 함수가 호출될 수 있어서, 해당 함수가 현재 참조하는 스코프를 지속할 필요가 있는 것이다.

#### with 구문의 모호성

value가 어떤 value인지 알 수 있을까?

        function doSome(value, obj){
                wiht(obj){
                        console.log(value);
                }
        }

## 클로저


#### 내부함수

> 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근이 가능하다.

        function outer(){
                var title = 'hello closer'
                return function (){
                        alert(title);
                }
        }

        var inner = outer();    //외부함수 소멸
        inner();                //내부함수(익명) 호출

return은 함수의 소멸을 의미.
inner() 호출 시, 외부함수의 title변수가 소멸되지 않았다는 것을 확인 할 수 있다. 

#### 활용 예시

>Private 변수를 만들 수 있다.

        function outer(count){
                return{
                        increase : function(){
                                return ++count;
                        },
                        decrease : function(){
                                return --count;
                        }
                };
        }
        var temp = outer();
        console.log(temp.increase());   //1
        console.log(temp.increase());   //2
        console.log(temp.decrease());   //1
        
        var temp2 = outer();
        console.log(temp2.increase());  //?

temp2 에 outer()함수를 한번 더 호출하면 별도의 스코프가 생성되어, count 변수가 따로 저장된다.



#### 즉시 호출 함수

>immediate Invoke Function Expression
익명함수를 생성하고, i를 파라미터로 넘긴다.

        1. var func = function(index){ /* 생략 */}
        2. var ret = = func(i);

        3. ret = (function(index){ /* 생략 */}(i));

