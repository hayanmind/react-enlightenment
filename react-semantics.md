# React 의미론

React의 메카니즘을 깨우치기 전에, 배우는 과정을 가속화할 수 있도록 먼저 몇가지 용어들을 정리하고 넘어가 봅시다.

아래는 React에 대해 이야기할 때 사용되는 가장 빈번한 용어들과 그 정의를 나열해놓았습니다.

#### Babel
[Babel](https://babeljs.io/)은 JavaScript ES\* (예: JS 2016, 2016, 2017)를 ES5로 변환해줍니다. Babel은 향후의 ES* 코드를 적고 JSX를 ES5 코드로 변환시키는 도구로 React팀이 정한 도구입니다.

***

#### Babel CLI
Babel은 [Babel CLI](https://babeljs.io/docs/usage/cli/)라 불리는 CLI 도구와 함께 제공됩니다. 커맨드 라인에서 파일들을 컴파일하기 위해 Babel CLI가 사용됩니다.

***

#### Component Configuration Options / Component Specifications (컴포넌트 환경설정 옵션)
`React.createClass()` 함수에게 (오브젝트 형태로) 전달되어 React 컴포넌트의 인스턴스가 되는 환경설정 인수들입니다.

***

#### Component Life Cycle Methods (컴포넌트 라이프 사이클 메소드)
컴포넌트 이벤트들의 하위 집단으로, 의미론적으로 다른 컴포넌트 환경설정 옵션들과 구분됩니다. (예: `componentWillUnmount`, `componentDidUpdate`, `componentWillUpdate`, `shouldComponentUpdate`, `componentWillReceiveProps`, `componentDidMount`, `componentWillMount`) 이런 메소드들은 컴포넌트가 살아있는 동안 특정 시점에 실행됩니다.

***

#### Document Object Model / DOM
"문서 객체 모델(DOM)은 HTML, XML과 SVG 문서를 위한 프로그래밍 인터페이스입니다. DOM은 문서의 구조화된 표현을 트리처럼 제공합니다. DOM은 트리로의 접근을 허용하는 메소드를 정의하고, 그것으로 문서 구조, 스타일과 내용을 바꿀 수 있게 합니다. DOM은 문서 표현을 다양한 properties와 메소드를 가지고 있는 노드와 오브젝트들의 구조화된 그룹처럼 제공합니다. 노드는 각각에 붙어 있는 이벤트 핸들러를 가질 수 있고, 한 번 이벤트가 트리거되면 이벤트 핸들러가 실행됩니다. 근본적으로, DOM은 웹 페이지를 스크립트나 프로그래밍 언어와 연결합니다." - [MSD](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

***

#### ES5
ECMAScript 표준의 5번째 버전입니다. [ECMAScript 5.1 edition](https://www.ecma-international.org/ecma-262/5.1/)는 2011년 6월에 완결되었습니다.

***

#### ES6/ES 2015
ECMAScript 표준의 6번째 버전입니다. JavaScript 2015 또는 ECMAScript 2015라고도 알려져 있습니다. [ECMAScript 6th edition](http://www.ecma-international.org/ecma-262/6.0/index.html)는 2015년 6월에 완결되었습니다.

***

#### ECMAScript 2016 / ES7
ECMAScript 표준의 7번째 버전입니다. [ECMAScript 7th edition](http://www.ecma-international.org/ecma-262/7.0/index.html)는 2016년 6월에 완결되었습니다.

***

#### ES\*
JavaScript의 현재 버전과 Babel 같은 도구를 이용해 오늘날에도 쓸 수 있는 잠재적인 이후 버전들을 대표하기 위해 사용됩니다. 당신이 "ES*"를 본다면, 거의 확실하게 ES5, ES6와 ES7도 함께 보일 것입니다.

***

#### JSX
[JSX](https://jsx.github.io/)는 선택적인 XML 같은 ECMAScript 문법 확장자입니다. JSX는 JavaScript 파일에서 HTML 같은 트리 구조를 정의할 때 사용됩니다. JavaScript 파일에서의 JSX 표현은 JavaScript 엔진이 그 파일을 분석하기 전에 반드시 JavaScript 문법으로 변환되어야 합니다. Babel은 JSX 표현을 변환하는데 일반적으로 사용되고 추천됩니다.

***

#### Node.js
[Node.js](https://nodejs.org/)는 JavaScript를 작성하기 위한 오픈소스, 크로스 플랫폼 런타임 환경입니다. 런타임 환경은 [Google's V8 JavaScript engine](https://developers.google.com/v8/)를 이용해 JavaScript를 해석합니다.

***

#### npm
[npm](https://www.npmjs.com/)은 Node.js 커뮤니티에서 시작된 JavaScript를 위한 패키지 매니저입니다.

***

#### React Attributes/Props
어떤 의미에서는 props가 React 노드를 위한 환경설정 옵션으로 생각할 수 있고, 다른 의미에서는 HTML 속성처럼 생각할 수 있습니다.

Props는 다양한 역할을 가지고 있습니다:

1. Props는 HTML 속성이 될 수 있습니다. prop이 알려져 있는 HTML 속성과 일치한다면, DOM의 마지막 HTML 요소로 추가됩니다.
2. `createElement()`로 전달된 Props은 `React.createElement()` 인스턴스의 인스턴스 property처럼 prop 오브젝트에 저장된 값이 됩니다. Props는 주로 컴포넌트로의 입력값으로 사용됩니다.
3. 몇몇의 특별한 props는 부작용이 있습니다. (예: [`key`](https://facebook.github.io/react/docs/multiple-components.html#dynamic-children), [`ref`](https://facebook.github.io/react/docs/more-about-refs.html), 그리고 [`dangerouslySetInnerHTML`](https://facebook.github.io/react/tips/dangerously-set-inner-html.html))

***

#### React
[React](https://facebook.github.io/react/)는 선언형으로 효율적이고 유연한 사용자 인터페이스를 작성하기 위한 오픈소스 JavaScript 라이브러리입니다.

***

#### React Component
React 컴포넌트는 `React.createClass()`(ES6 클래스를 사용한다면, `React.Component`)를 부르는 것으로 생성됩니다. 이 함수는 환경을 설정하고 React 컴포넌트를 생성하는 데 사용되는 옵션의 오브젝트를 인수로 받습니다. 가장 흔한 환경설정 옵션은 React 노드를 반환하는 `render` 함수입니다. 그러므로 React 컴포넌트를 하나 또는 그 이상의 React 노드/컴포넌트를 포함하고 있는 추상적인 개념으로 생각해도 됩니다.

***

#### React Element Nodes / `ReactElement`
`React.createElement();`으로 생성된 가상 DOM의 HTML 또는 커스텀 HTML 요소 노드를 나타내는 것입니다.

***

#### React Nodes
React 노드(예: 요소와 텍스트 노드)는 React에서의 기본적인 오브젝트 타입입니다. React 노드는 `React.createElement('div');`를 이용해 생성할 수 있습니다. 다시 말해서, React 노드는 DOM 노드와 자식들의 DOM 노드를 대표하는 오브젝트입니다. 이것들은 가볍고, 상태를 저장하지 않으며, 변하지 않고, 가상인 DOM 노드의 표현입니다.

***

#### React Node Factories
특정 타입 property를 가진 React 요소 노드를 생산하는 함수입니다.

***

#### React Stateless Function Component
컴퍼넌트가 state 없이 순수하게 props 단독의 결과일 때, React 컴포넌트 인스턴스를 만들 필요 없이 컴포넌트를 순수 함수처럼 사용할 수 있습니다.

```
var MyComponent = function(props){
    return <div>Hello {props.name}</div>;
};

ReactDOM.render(<MyComponent name="doug" />, app);
```

***

#### React Text Nodes / `ReactText`
`React.createElement('div',null,'a text node');`으로 생성된 가상 DOM의 텍스트 노드를 나타내는 것입니다.

***

#### Virtual DOM
브라우저 DOM의 효율적인 다시 렌더하기(예: JavaScript의 차이 비교하기)에 사용되는 인메모리 React 요소/컴포넌트들의 JavaScript 트리입니다.

***

#### Webpack
[Webpack](https://webpack.github.io/)은 의존성이 있는 모듈(.js, .css, .txt 등)을 가지고 이 모듈들을 대표하는 정적인 asset을 만들어내는 모듈 로더와 번들러입니다.
