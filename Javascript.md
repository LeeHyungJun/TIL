# Javascript

자바스크립트를 학습하고 기록합니다.

## 타입

#### 기본타입

```html
1. number
2. string
3. boolean
4. Undefined - 변수에 값이 할당되지 않을 때 인터프리터가 undefined로 할당.
5. null - 개발자가 의도적으로 할당하는 값.
```

#### 참조타입 (객체타입)

```html
1. object
2. Array
3. Funtion
```

#### typeof

> 특정 변수가 어떤 타입인지 확인하는 연산자. (반환값은 string)

```html
Usage : typeof myVariable;
```

**예시**

```js
1. typeof 3     === "number"
2. typeof "aa"  === "string"
3. typeof {};   === "object"
4. typeof [];   === "object"
5. typeof function(){}; === "function"
6. typeof null;  === "object"
```

#### instanceof

> A가 B의 인스턴스인지 확인하고 true, false 반환

```html
Usage : A instanceof B
```

**기본형 string / new Stirng("") / String("")**

```js
var color1 = new String("red"); // 객체
var color2 = "red";             // 기본형
var color3 = String("red");     // ToString()함수 사용과 동일.

console.log(color1 == color2);          // true. 형변환이 일어남.
console.log(color1 instanceof Object);  // true. 문자열 객체
console.log(color2 instanceof String);  // false. 기본형
console.log(color3 instanceof String);  // false. ToString() -> (기본형)
```


## 논리연산자

논리연산자를 이용해서 **초기화**를 좀 더 멋진 방법으로 할 수 있다.

>A를 true로 반환 가능하면 A 리턴. 아니면 B리턴.

```js
// Usage : A || B 
var total = (total || 0) + i
```

>A를 true로 반환 가능하면 B 리턴. 아니면 A리턴

```js
// Uasage : A && B

var a = 10;
var b = 20;
var res = a && b;
console.log(res);  //20
```

## 스코프

현재 접근할 수 있는 변수들의 범위.

#### 생성

1. Function, with, catch 만 스코프를 생성한다.
2. with, catch 구문은 인자로 받는 변수들만 내부 스코프에 포함된다.
3. For-loop은 별도의 스코프를 생성하지 않는다.
4. {} 블록은 스코프를 생성하지 않는다.

#### 예시

```js
<script>
    var i, len = 3;
    for(i = 0; i < len; i++){
        document.getElementById("div"+i).addEventListener("click", function(){
            alert(i);
        }, false);
    }
</script>
```

addEventListener()로 콜백함수를 설정할 때 **익명 함수가 선언되면서 스코프가 생성**되어 체인을 만든다.
loop를 돌면서 생성된 3개의 익명함수 스코프는 모두 같은 global scope변수를 바라본다.


#### 지속성

>함수가 선언된 곳이 아닌 전혀 다른 곳에서 함수가 호출될 수 있어서, 해당 함수가 현재 참조하는 스코프를 지속할 필요가 있는 것이다.

#### with 구문의 모호성

with를 사용하면 this없이 접근 가능하나, value가 어떤 value인지 알 수 있을까?

```js
function doSome(value, obj){
    wiht(obj){
        console.log(value);
    }
}
```

## 클로저


#### 내부함수

> 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근이 가능하다.

```js
function outer(){
    var title = 'hello closer'
        return function (){
            alert(title);
        }
}
var inner = outer();    //외부함수 소멸
inner();                //내부함수(익명) 호출
```

return은 함수의 소멸을 의미.
inner() 호출 시, 외부함수의 title변수가 소멸되지 않았다는 것을 확인 할 수 있다. 

#### 활용 예시

>Private 변수를 만들 수 있다. 또한, 클로저를 통해서 각 함수는 자기만의 고유한 값을 보유하고 스코프 체인을 유지하면서 그 체인 안에 있는 모든 변수의 값들을 유지한다.
```js
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
```
count 변수는 outer()함수의 로컬 변수이므로 일반적인 방법으로는 외부에서 접근할 수 없다. 마지 객체지향 언어의 **private 변수** 와 비슷하다. 그런데 count 변수에 접근하는 또 다른 함수(객체)를 outer()함수의 반환값으로 지정하고, 이를 글로벌 영역에 있는 temp 변수에 할당함으로써, outer() 함수 외부에서도 temp변수를 통해 count변수에 접근할 수 있다.

**글로벌 영역에서 temp는 outer()의 반환값에 대한 레퍼런스(포인터)를 가지고 있는 것이다.**

temp2 에 outer()함수를 한번 더 호출하면 별도의 스코프가 생성되어,count 변수가 따로 저장된다.


#### 즉시 호출 함수

>immediate Invoke Function Expression
>익명함수를 생성하고, i를 파라미터로 넘긴다.

```js
1. var func = function(index){ /* 생략 */};
2. var ret = func(i);

// 1+2 한 줄로 표현 
3. ret = (function(index){ /* 생략 */}(i));
```

#### 클로저 단점

1. 클로저는 메모리를 소모한다.
2. 스코프 생성과 이후 변수 조회에 따른 퍼포먼스 손해가 있다.


## 콜백함수

자바스크립트에서는 함수도 **객체**이다. 그래서 우리는 함수 이름 혹은 익명함수를 넘겨주면서 함수를 파라미터로 넘길 수 있다.
