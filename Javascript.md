# Javascript

자바스크립트를 학습하고 기록합니다.

## 타입

#### 기본타입

```html
1. number
2. string
3. boolean
4. Undefined - 변수에 값이 할당되지 않을 때 인터프리터가 ubdefined로 할당.
5. null - 개발자가 의도적으로 할당하는 값.
```

#### 참조타입 (객체타입)

```html
1. object
2. Array
3. Funtion
```

#### typeof

```js
1. typeof 3     === "number"
2. typeof "aa"  === "string"
3. typeof {};   === "object"
4. typeof [];   === "object"
5. typeof function(){}; === "function"
6. typeof null;  === "object"
```

#### instanceof

기본형 문자열과 문자열 객체 비교

```js
var color1 = new String("red");
var color2 = "red";

console.log(color1 == color2);          // true. '==' 연산자 -> 형변환이 일어남.
console.log(color1 instanceof String);  // true.
console.log(color1 instanceof Object);  // true.
console.log(color2 instanceof String);  // false.
```

```js
console.log(color1 === color2); // false; 타입까지 비교.
console.log(color1.constructor === String); // true. 생성자 함수
console.log(color2.constructor === String); // ture. ?!.
```


## 논리연산자

논리연산자를 이용해서 **초기화**를 좀 더 멋진 방법으로 할 수 있다.

1. A || B 
>A를 true로 반환 가능하면 A 리턴. 아니면 B리턴.

```js
var total = (total || 0) + i
```

2. A && B
>A를 true로 반환 가능하면 B 리턴. 아니면 A리턴

```js
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
var i, len =3;

</script>
```

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

>Private 변수를 만들 수 있다.
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

temp2 에 outer()함수를 한번 더 호출하면 별도의 스코프가 생성되어,
count 변수가 따로 저장된다.

#### 즉시 호출 함수

>immediate Invoke Function Expression
>익명함수를 생성하고, i를 파라미터로 넘긴다.

```js
1. var func = function(index){ /* 생략 */};
2. var ret = func(i);
3. ret = (function(index){ /* 생략 */}(i));
```

## 콜백함수

자바스크립트에서는 함수도 **객체**이다. 그래서 우리는 함수 이름 혹은 익명함수를 넘겨주면서 함수를 파라미터로 넘길 수 있다.
