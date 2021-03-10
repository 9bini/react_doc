# JSX 소개

``` javascript
const element = <h1>Hello, world!</h1>;
```

* 태그 문법은 문자열도, HTML도 아닙니다.  
* JSX라 하며 JavaScript를 확장한 문법입니다.
* UI가 어떻게 생겨야 하는지 설명하기 위해 * React와 함께 사용할 것을 권장합니다. 
* JSX라고 하면 템플릿 언어가 떠오를 수도 있지만, JavaScript의 모든 기능이 포함되어 있습니다.
* JSX는 React “엘리먼트(element)” 를 생성합니다

## JSX란?

React에서는 이벤트가 처리되는 방식, 시간에 따라 state가 변하는 방식, 화면에 표시하기 위해 데이터가 준비되는 방식 등 렌더링 로직이 본질적으로 다른 UI 로직과 연결된다는 사실을 받아들입니다.

React는 별도의 파일에 마크업과 로직을 넣어 기술을 인위적으로 분리하는 대신, 둘 다 포함하는 “컴포넌트(component)”라고 부르는 느슨하게 연결된 유닛으로 관심사를 분리(Separation of concerns)합니다.
> 컴포넌트
> 
> 기존의 코딩 방식에 의한 개발에서 벗어나 소프트웨어 구성단위(module)를 미리 만든 뒤 필요한 응용 기술을 개발할 때 이 모듈을 조립하는 기술을 말한다. 

> 관심사를 분리(SoC)(Separation of concerns)
> 
> 컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙으로, 각 부문은 개개의 관심사를 해결한다.

react는 JSX 사용시 더욱 도움이 되는 에러 및 경고 메시지를 표시할 수 있게 해줍니다.

## JSX에 표현식 포함하기

```javascript
// name이라는 변수를 선언한 후 중괄호로 감싸 JSX 안에 사용
const name = 'Josh Perez'
const element = <h1>Hello, {name}</h1>;

ReactDom.render(
    element,
    document.getElementByid('root')
)
```

JSX의 중괄호 안에는 유효한 모든 JavaScript 표현식을 넣을 수 있습니다.

```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

가독성을 좋게 하기 위해 JSX를 여러 줄로 나눴습니다. 필수는 아니지만, 이 작업을 수행할 때 자동 세미콜론 삽입을 피하고자 괄호로 묶는 것을 권장합니다.

## JSX도 표현식입니다

컴파일이 끝나면, JSX 표현식이 정규 JavaScript 함수 호출이 되고 JavaScript 객체로 인식됩니다.

즉, JSX를 if 구문 및 for loop 안에 사용하고, 변수에 할당하고, 인자로서 받아들이고, 함수로부터 반환할 수 있습니다.

```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

## JSX 속성 정의

속성에 따옴표("")를 이용해 문자열 리터럴을 정의할 수 있습니다.

```javascript
const element = <div tabIndex="0"></div>;
```

중괄호({}, Brace)를 사용하여 어트리뷰트에 JavaScript 표현식을 삽입할 수도 있습니다.

```javascript
const element = <img src={user.avatarUrl}></img>;
```

### 주의

* 어트리뷰트에 JavaScript 표현식을 삽입할 때 중괄호 주변에 따옴표를 입력하지 마세요
* 따옴표(문자열 값에 사용) 또는 중괄호(표현식에 사용) 중 하나만 사용하고, 동일한 어트리뷰트에 두 가지를 동시에 사용하면 안 됩니다.
* JSX는 HTML보다는 JavaScript에 가깝기 때문에, React DOM은 HTML 어트리뷰트 이름 대신 camelCase 프로퍼티 명명 규칙을 사용합니다.

## JSX로 자식 정의

태그가 비어있다면 XML처럼 /> 를 이용해 바로 닫아주어야 합니다.

```javascript
const element = <img src={user.avatarUrl} />;
```

JSX 태그는 자식을 포함할 수 있습니다.

```javascript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## JSX는 주입 공격을 방지합니다

JSX에 사용자 입력을 삽입하는 것은 안전합니다.

```javascript
const title = response.potentiallyMaliciousInput;
// 이것은 안전합니다.
const element = <h1>{title}</h1>;
```

* 기본적으로 React DOM은 JSX에 삽입된 모든 값을 렌더링하기 전에 이스케이프(Escape) 처리 하므로, 애플리케이션에서 명시적으로 작성되지 않은 내용은 주입되지 않습니다.
* 모든 항목은 렌더링 되기 전에 문자열로 변환됩니다. 
* 이런 특성으로 인해 XSS (cross-site-scripting) 공격을 방지할 수 있습니다.


> 이스케이프(Escape)
> 
> 어떤 부호나 언어 또는 기존의 패턴에서 탈출하는 것.

## JSX는 객체를 표현합니다

Babel은 JSX를 `React.createElement()` 호출로 컴파일합니다.
다음 두 예시는 동일합니다.

```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()`는 버그가 없는 코드를 작성하는 데 도움이 되도록 몇 가지 검사를 수행하며, 기본적으로 다음과 같은 객체를 생성합니다.

```javascript
// 주의: 다음 구조는 단순화되었습니다
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

이러한 객체를 “React 엘리먼트”라고 하며, 화면에서 보고 싶은 것을 나타내는 표현이라 생각하면 됩니다.
React는 이 객체를 읽어서, DOM을 구성하고 최신 상태로 유지하는 데 사용합니다.

> Tip
> ES6 및 JSX 코드가 올바르게 표시되도록 편집기에 “Babel” 언어 설정을 사용하는 것을 권장합니다.