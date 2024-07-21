---
description: 자바스크립트 기본 함수를 살펴봅니다.
---

# 자바스크립트 기본 - 함수

### 함수

#### 외부 변수

외부 변수는 지역 변수가 없는 경우에만 사용할 수 있습니다.

함수 내부에 외부 변수와 동일한 이름을 가진 변수가 선언되었다면, **내부 변수는 외부 변수를 가립니다.**

```javascript
let userName = 'John';

function showMessage() {
    let userName = "Bob"; // 같은 이름을 가진 지역 변수를 선언합니다.
    
    let message = 'Hello, ' + userName; // Bob
    alert(message);
}

showMessage(); // 내부 변수인 userName 만 사용합니다.

alert( userName ); // 함수는 외부 변수에 접근하지 않습니다. 따라서 값이 변경되지 않고 John 이 출력됩니다.
```

{% hint style="info" %}
**전역 변수**

위 예시의 `userName` 처럼, 함수 외부에 선언된 변수는 _전역 변수(global variable)_ 라고 부릅니다.

전역 변수는 같은 이름을 가진 지역 변수에 의해 가려지지만 않는다면 모든 함수에서 접근할 수 있습니다.

변수는 연관되는 함수 내에 선언하고, 전역 변수는 되도록 사용하지 않는 것이 좋습니다. 비교적 근래에 작성된 코드들은 대부분 전역 변수를 사용하지 않거나 최소한으로만 사용합니다. 다만 프로젝트 전반에서 사용되는 데이터는 전역 변수에 저장하는 것이 유용한 경우도 있으니 이 점을 알아두시기 바랍니다.
{% endhint %}

#### 기본 값

함수 호출 시 매개변수에 인수를 전달하지 않으면 그 값은 `undefined` 가 됩니다.

```javascript
function showMessage(from, text) {
    alert( from + ": " + text );
}
showMessage("Ann");
```

위와 같이 2개의 매개 변수에 대해 하나의 인수만 넣어도 호출이 가능합니다. 두 번째 매개변수에 값을 전달하지 않았기 때문에 `text` 엔 `undefined` 가 할당될 뿐입니다. 따라서 에러 없이 `"Ann: undefined"` 가 출력됩니다.

매개변수에 값을 전달하지 않아도 그 값이 `undefined` 가 되지 않게 하려면 함수를 선언할 때 `=` 를 사용해 '기본값(default value)'을 설정해주면 됩니다.

```javascript
function showMessage(form, text = "no text given") {
    alert ( from + ": " + text );
}

showMessage("Ann"); // Ann: no text given
```

매개변수에 값을 전달해도 그 값이 `undefined` 와 엄격히 일치한다면 기본값이 할당됩니다.

```javascript
showMessage("Ann", undefined); // Ann: no text given
```

아래와 같이 복잡한 표현식도 기본 값으로 설정 가능합니다.

```javascript
function showMessage(from, text = anotherFunction()) {
    // text 값이 없을 때만 anotherFunction() 이 호출됨
}
```

#### 매개변수 기본 값을 설정할 수 있는 또 다른 방법

가끔은 함수를 선언할 때가 아닌 함수 선언 후에 매개변수 기본 값을 설정하는 것이 적절한 경우도 있습니다.

```javascript
function showMessage(text) {
    if (text === undefined) {
        text = '빈 문자열';
    }
    alert(text);
}

showMessage(); // 빈 문자열
```

`if` 문이 아닌 논리 연산자 `||` 을 쓰는 방법도 있습니다.

```javascript
function showMessage(text) {
    text = text || '빈 문자열';
    ...
}
```

이 외에도 모던 자바스크립트 엔진이 지원하는 병합 연산자 `??` 를 사용하면 `0` 처럼 falsy 로 평가되는 값들을 일반 값처럼 처리할 수 있어서 좋습니다.

```javascript
function showMessage(count) {
    alert(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknown
showCount(); // unknown
```

#### 반환 값

```javascript
function sum(a, b) {
    return a + b;
}

let result = sum(1, 2);
alert( result ); // 3
```

지시자 `return` 은 함수 내 어디서든 사용할 수 있습니다. 실행 흐름이 지시자 `return` 을 만나면 함수 실행은 즉시 중단되고 함수를 호출한 곳에 값을 반환합니다.

{% hint style="info" %}
**`return` 문이 없거나 `return` 지시자만 있는 함수는 `undefined` 를 반환합니다.**

```javascript
function doNothing() {}
alert( doNothing() === undefined ); // true

function doReturn() {
    return;
}
alert( doReturn() === undefined ); // true
```
{% endhint %}

{% hint style="warning" %}
**`return` 과 값 사이에 절대 줄을 삽입하지 마세요.**

반환하려는 값이 긴 표현식인 경우, 아래와 같이 지시자 `return` 과 반환하려는 값 사이에 새 줄을 넣어 코드를 작성하고 싶을 수도 있습니다.

```javascript
return
 (some + long + expression + or + whatever * f(a) + f(b))
```

자바스크립트는 return문 끝에 세미콜론을 자동으로 넣기 때문에 이렇게 `return` 문을 작성하면 안 됩니다. 위 코드는 아래 코드처럼 동작합니다.

```javascript
return;
 (some + long + expression + or + whatever * f(a) + f(b))
```

표현식을 여러 줄에 걸쳐 작성하고 싶다면 표현식이 `return` 지시자가 있는 줄에서 시작하도록 작성해야 합니다. 또는 아래와 같이 여는 괄호를 `return` 지시자와 같은 줄에 써줘도 괜찮습니다.

```javascript
return (
 some + long + expression
 + or +
 whatever * f(a) + f(b)
 )
```
{% endhint %}

#### 함수 이름짓기

함수는 어떤 `동작` 을 수행하기 위한 코드를 모아놓은 것입니다. 따라서 함수의 이름은 대개 동사입니다. 함수 이름은 가능한 한 간결하고 명확해야 합니다. 코드를 읽는 사람은 함수 이름만 보고도 함수가 어떤 기능을 하는지 힌트를 얻을 수 있어야 합니다.

함수가 어떤 동작을 하는지 축약해서 설명해 주는 동사를 접두어로 붙여 함수 이름을 만드는 게 관습입니다. 다만, 팀 내에서 그 뜻이 반드시 합의된 접두어만 사용해야 합니다.

`"show"` 로 시작하는 함수는 대개 무언가를 보여주는 함수입니다.

이 외에 아래와 같은 접두어를 사용할 수 있습니다.

* `"get..."` - 값을 반환함
* `"calc..."` - 무언가를 계산함
* `"create..."` - 무언가를 생성함
* `"check..."` - 무언가를 확인하고 불린값을 반환함

{% hint style="info" %}
**함수는 동작 하나만 담당해야 합니다.**

함수는 함수 이름에 언급되어 있는 동작을 정확히 수행해야 합니다. 그 이외의 동작은 수행해선 안 됩니다.

독립적인 두 개의 동작은 독립된 함수 두 개에서 나눠서 수행할 수 있게 해야 합니다. 한 장소에서 두 동작을 동시에 필요로 하는 경우라도 말이죠(이 경우는 제 3의 함수를 만들어 그곳에서 두 함수를 호출합니다).

개발자들이 빈번히 하는 실수를 소개해 보겠습니다.

* `getAge` 함수는 나이를 얻어오는 동작만 수행해야 합니다. `alert` 창에 나이를 출력해 주는 동작은 이 함수에 들어가지 않는 것이 좋습니다.
* `createForm` 함수는 form을 만들고 이를 반환하는 동작만 해야 합니다. form을 문서에 추가하는 동작이 해당 함수에 들어가 있으면 좋지 않습니다.
* `checkPermission` 함수는 승인 여부를 확인하고 그 결과를 반환하는 동작만 해야 합니다. 승인 여부는 보여주는 메세지를 띄우는 동작이 들어가 있으면 좋지 않습니다.

위 예시들은 접두어의 의미가 합의되었다고 가정하고 만들었습니다. 본인이나 본인이 속한 팀에서 접두어의 의미를 재합의하여 함수를 만들 수도 있긴 하지만, 아마도 위 예시에서 사용한 접두어 의미와 크게 차이가 나진 않을 겁니다.

접두어를 붙여 만든 모든 함수는 팀에서 만든 규칙을 반드시 따라야 합니다. 팀원들은 이 규칙을 충분히 이해하고 있어야 하며, 팀원들 사이에 이 규칙이 잘 공유되어야 합니다.
{% endhint %}

#### 함수 == 주석

함수는 간결하고, 한 가지 기능만 수행할 수 있게 만들어야 합니다. 함수가 길어지면 함수를 잘게 쪼갤 때가 되었다는 신호로 받아 들이셔야 합니다.\
함수를 간결하게 만들면 테스트와 디버깅이 쉬워집니다. 그리고 함수 그 자체로 주석의 역할까지 합니다.

같은 동작을 하는 `showPrimes(n)` 이라는 함수 두 개를 만들어 비교해 보겠습니다. 해당 함수는 `n` 까지의 소수(prime numbers)를 출력해줍니다.

* 첫번째 코드

```javascript
function showPrimes(n) {
    nextPrime: for (let i = 2; i < n; i++) {
        for (let j = 2; j < i; j++) {
            if (i % j == 0) continue nextPrime;
        }
    }
    alert(i); // 소수
}
```

* 두번째 코드

```javascript
function showPrimes(n) {
    for (let i = 2; i < n; i++) {
        if (!isPrime(i)) continue;
        alert(i); // 소수
    }
}

function isPrime(n) {
    for (let i = 2; i < n; i++) {
        if (n % i == 0) return false;
    }
    return true;
}
```

두 번째 코드가 더 이해하기 쉽습니다. `isPrime` 함수 이름을 보고 해당 함수가 소수 여부를 검증하는 동작을 한다는 걸 쉽게 알 수 있습니다. 이렇게 이름만 보고도 어떤 동작을 하는지 알 수 있는 코드를 자기 설명적(self-describing) 코드라고 부릅니다.

위와 같이 함수는 중복을 없애려는 용도 외에도 사용할 수 있습니다. 이렇게 함수를 활용하면 코드가 정돈되고 가독성이 높아집니다.

### 함수 표현식

자바스크립트는 함수를 특별한 종류의 값으로 취급합니다. <mark style="color:purple;">**다른 언어에서처럼 "특별한 동작을 하는 구조"로 취급하지 않습니다.**</mark>

함수 표현식으로 함수를 생성해보겠습니다.

```javascript
let sayHi = function() {
    alert( "Hello" );
}
```

함수를 생성하고 변수에 값을 할당하는 것처럼 함수가 변수에 할당되었습니다. 함수가 어떤 방식으로 만들어졌는지에 관계없이 함수는 값이고, 따라서 변수에 할당할 수 있습니다.

함수는 값이기 때문에 `alert` 를 이용하여 함수 코드를 출력할 수도 있습니다.

```javascript
function sayHi() {
    alert( "Hello" );
}
alert(sayHi); // 함수 코드 자체가 보임
```

마지막 줄에서 `sayHi` 옆에 괄호가 없기 때문에 함수는 실행되지 않습니다. 어떤 언어에선 괄호 없이 함수 이름만 언급해도 함수가 실행됩니다. 하지만 자바스크립트는 괄호가 있어야만 함수가 호출됩니다.

자바스크립트에서 함수는 값입니다. 따라서 함수를 값처럼 취급할 수 있습니다. 위 코드에선 함수 소스 코드가 문자형으로 바뀌어 출력되었습니다.

함수는 `sayHi()` 처럼 호출할 수 있다는 점 때문에 일반적인 값과는 조금 다릅니다.\
하지만 그 본질은 값이기 때문에 값에 할 수 있는 일을 함수에도 할 수 있습니다.\
변수를 복사해 다른 변수에 할당하는 것처럼 함수를 복사해 다른 변수에 할당할 수 있습니다.

```javascript
function sayHi() {
    alert( "Hello" );
}

let func = sayHi; // 괄호가 없다는 점에 주의

func();  // 정상적으로 실행
sayHi(); // 정상적으로 실행
```

함수 표현식을 사용해 아래와 같이 정의할 수도 있습니다.

```javascript
let sayHi = function() {
    alert( "Hello" );
}
let func = sayHi;
// ...
```

{% hint style="info" %}
**끝에 세미 콜론은 왜 있나요?**

함수 선언문에는 세미 콜론이 없는데 `sayHi` 에는 왜 있을까요?

```javascript
function sayHi() {
    // ...
}
let sayHi = function() {
    // ...
}
```

이유는 간단합니다.

* `if {}, for {}, function f {}` 같이 중괄호로 만든 코드 블럭 끝엔 `;` 이 없어도 됩니다.
* 함수 표현식은 `let sayHi = ...;` 과 같은 구문 안에서 값의 역할을 합니다. 코드 블록이 아니고 값처럼 취급되어 변수에 할당되죠. 모든 구문의 끝엔 세미 콜론 `;` 을 붙이는게 좋습니다. 함수 표현식에 쓰인 세미 콜론은 함수 표현식 때문에 붙여진게 아니라, 구문의 끝이기 때문에 붙여졌습니다.
{% endhint %}

### 콜백 함수

함수 표현식에 관한 예시를 좀 더 살펴보겠습니다.

매개변수가 3개 있는 함수, `ask(question, yes, no)` 를 작성해보겠습니다. 각 매개변수에 대한 설명은 아래와 같습니다.

* `question` : 질문
* `yes` : "Yes" 라고 답한 경우 실행되는 함수
* `no` : "No" 라고 답한 경우 실행되는 함수

함수는 반드시 `question(질문)` 을 해야 하고, 사용자의 답변에 따라 `yes()` 나 `no()` 를 호출합니다.

```javascript
function ask(question, yes, no) {
    if (confirm(question)) yes()
    else no();
}

function showOk() {
    alert( "동의하셨습니다." );
}

function showCancel() {
    alert( "취소 버튼을 누르셨습니다." );
}

ask("동의하십니까?", showOk, showCancel);
```

**함수 `ask` 의 인수, `showOk` 와 `showCancel` 은 **<mark style="color:purple;">**콜백 함수 또는 콜백**</mark>**이라고 불립니다.**

함수를 함수의 인수로 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백 함수의 개념입니다. 위 예시에선 사용자가 "yse"라고 답한 경우 `showOk` 가 콜백이 되고, "no"라고 대답한 경우 `showCancel` 가 콜백이 됩니다.

아래와 같이 함수 표현식을 사용하면 코드 길이가 짧아집니다.

```javascript
function ask(question, yes, no) {
    if (confirm(question)) yes()
    else no();
}

ask("동의하십니까?",
    function() { alert("동의하셨습니다."); },
    function() { alert("취소 버튼을 누르셨습니다."); }
);
```

`ask(...)` 안에 함수가 선언된 게 보이시나요? 이렇게 이름 없이 선언한 함수는 **익명 함수(anonymous function)** 라고 부릅니다. 익명 함수는 (변수에 할당된게 아니기 때문에) `ask` 바깥에선 접근할 수 없습니다. 위 예시는 의도를 가지고 이렇게 구현하였기 대문에 바깥에서 접근할 수 없어도 문제가 없습니다.

{% hint style="info" %}
**함수는 "동작"을 나타내는 값입니다.**

문자열이나 숫자 등의 일반적인 값들은 데이터를 나타냅니다.

함수는 하나의 **동작(action)** 을 나타냅니다.

동작을 대변하는 값인 함수를 변수 간 전달하고, 동작이 필요할 때 이 값을 실행할 수 있습니다.
{% endhint %}

### 함수 표현식 vs 함수 선언문

함수 표현식과 선언문의 차이에 대해 알아봅시다.

첫 번째는 문법입니다. 코드를 통해 어떤 차이가 있는지 살펴봅시다.

* 함수 선언문: 함수는 주요 코드 흐름 중간에 독자적인 구문 형태로 존재합니다.

```javascript
function sum(a, b) {
    return a + b;
}
```

* 함수 표현식: 함수는 표현식이나 구문 구성(syntax construct) 내부에 생성됩니다. 아래 예시에선 함수가 할당 연산자 `=` 를 이용해 만든 "할당 표현식" 우측에 생성되었습니다.

```javascript
let sum = function(a, b) {
    return a + b;
};
```

두 번째 차이는 자바스크립트 엔진이 **언제 함수를 생성하는지**에 있습니다.

**함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성합니다. 따라서 실행 흐름이 함수에 도달했을 때부터 해당 함수를 사용할 수 있습니다.**

위 예시에서 스크립트가 실행되고, 실행 흐름이 `let sum = function...` 의 우측(함수 표현식)에 도달 했을 때 함수가 생성됩니다. 이때 이후부터 해당 함수를 사용(할당, 호출 등)할 수 있습니다.

하지만 함수 선언문은 조금 다릅니다.

**함수 선언문은 함수 선언문이 정의되기 전에도 호출할 수 있습니다.**

따라서 전역 함수 선언문은 스크립트 어디에 있으냐에 상관없이 어디에서든 사용할 수 있습니다.\
이게 가능한 이유는 자바스크립트의 내부 알고리즘 때문입니다. 자바스크립트는 스크립트를 실행하기 전, 준비 단계에서 전역에 선언된 함수 선언문을 찾고, 해당 함수를 생성합니다. 스크립트가 진짜 실행되기 전 "초기화 단계"에서 함수 선언 방식으로 정의한 함수가 생성되는 것이죠.

스크립트는 함수 선언문이 모두 처리된 이후에서야 실행됩니다. 따라서 스크립트 어디서든 함수 선언문으로 선언한 함수에 접근할 수 있는 것입니다.

```javascript
sayHi("John"); // Hello, John

function sayHi(name) {
    alert( `Hello, ${name}` );
}
```

함수 선언문, `sayHi` 는 스크립트 실행 준비 단계에서 생성되기 때문에, 스크립트 내 어디에서든 접근할 수 있습니다.

그러나 함수 표현식으로 정의한 함수는 함수가 선언되기 전에 접근하는게 불가능합니다.

```javascript
sayHi("John"); // error!

let sayHi = function(name) {
    alert( `Hello, ${name}` );
};
```

세번째  차이점은, 스코프입니다.

**엄격 모드에서 함수 선언문이 코드 블록 내에 위치하면 해당 함수는 블록 내 어디서든 접근할 수 있습니다. 하지만 블록 밖에서는 함수에 접근하지 못합니다.**

```javascript
let age = prompt("나이를 알려주세요.", 18);

if (age < 18) {
    function welcome() {
        alert("안녕!");
    }
} else {
    function welcome() {
        alert("안녕하세요!");
    }
}

// 함수를 나중에 호출합니다.
welcome(); // Error: welcome is not defined
```

함수 선언문은 함수가 선언된 코드 블록 안에서만 유효하기 때문에 이런 에러가 발생합니다.

함수 표현식을 사용하면 위와 같은 상황에서도 함수 사용이 가능합니다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

let welcome;

if (age < 18) {
    welcome = function() {
        alert("안녕!");
    };
} else {
    welcome = function() {
        alert("안녕하세요!");
    };
}

welcome(); // 제대로 동작합니다.
```

물음표 연산자 `?` 를 사용하면 위 코드를 좀 더 단순화할 수 있습니다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

let welcome = (age < 18) ?
  function() { alert("안녕!"); } :
  function() { alert("안녕하세요!"); };

welcome(); // 제대로 동작합니다.
```

{% hint style="info" %}
**함수 선언문과 함수 표현식 중 무엇을 선택해야 하나요?**

함수 선언문을 이용하는 걸 먼저 고려하는 것이 좋다. 함수 선언문으로 함수를 정의하면, 함수가 선언되기 전에 호출할 수 있어서 코드 구성을 좀 더 자유롭게 할 수 있습니다.

함수 선언문을 사용하면 가독성도 좋아집니다.

그러나 어떤 이유로 함수 선언 방식이 적합하지 않거나, 조건에 따라 함수를 선언해야 한다면 함수 표현식을 사용해야 합니다.
{% endhint %}

### 화살표 함수 기본

함수 표현식보다 단순하고 간결한 문법으로 함수를 만들 수 있는 방법이 있습니다.

바로 화살표 함수(arrow function)를 사용하는 것입니다. 화살표 함수라는 이름은 문법의 생김새를 차용해 지어졌습니다.

```javascript
let func = (arg1, arg2, ...argN) => expression
```

이렇게 코드를 작성하면 인자 `arg1..argN` 를 받는 함수 `func` 이 만들어집니다. 함수 `func` 는 화살표(`=>`) 우측의 `표현식(expression)` 을 평가하고, 평가 결과를 반환합니다.

아래 함수의 축약 버전이라 볼 수 있습니다.

```javascript
let func = function(arg1, arg2, ...argN) {
    return expression;
}
```

아래는 좀 더 구체적인 예시입니다.

```javascript
let sum = (a, b) => a + b;
/* 위 화살표 함수는 아래 함수의 축약 버전입니다.
let sum = function(a, b) {
    return a + b;
};
*/
alert( sum(1, 2) ); // 3
```

* 인수가 하나밖에 없다면 인수를 감싸는 괄호를 생략할 수 있습니다.

```javascript
let double = n => n * 2;
alert( double(3) ); // 6
```

* 인수가 하나도 없을 땐 괄호를 비워놓으면 됩니다. 다만, 이 때 괄호는 생략할 수 없습니다.

```javascript
let sayHi = () => alert("안녕하세요!");
sayHi();
```

화살표 함수는 함수 표현식과 같은 방법으로 사용할 수 있습니다.

아래 예시와 같이 동적으로 함수를 만들 수 있습니다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

let welcome = (age < 18) ?
    () => alert('안녕') :
    () => alert("안녕하세요!");
    
welcome();
```

함수 본문이 한 줄인 간단한 함수는 화살표 함수를 사용해서 만드는게 편리합니다.

#### 본문이 여러 줄인 화살표 함수

평가해야 할 표현식이나 구문이 여러 개인 함수가 있을 수도 있습니다. 이 경우 역시 화살표 함수 문법을 사용해 함수를 만들 수 있습니다. 다만, 이때는 중괄호 안에 평가해야 할 코드를 넣어주어야 합니다. 그리고 `return` 지시자를 사용해 명시적으로 결과값을 반환해 주어야 합니다.

```javascript
let sum = (a, b) => {
    let result = a + b;
    return result;
}

alert( sum(1, 2) ); // 3
```
