# React는 무엇인가요?

React는 state가 없거나 state를 저장하는 사용자 인터페이스를 쉽게 추론하고, 쉽게 구성하고, 쉽게 유지하도록 만들어주는 JavaScript 도구입니다. React는 React 노드라고 불리는 HTML과 유사한 노드들을 이용해 평서문처럼 선언하고 UI를 React 컴포넌트라고도 알려진 UI 컴포넌트로 나누는 수단을 제공합니다. React 노드는 결국에는 UI 렌더링을 위한 형태로 변환됩니다. (예: HTML/DOM, canvas, svg 등)

제가 React가 무엇인지에 대해 말로 표현하려 시도하며 장황하게 말할 수도 있겠지만, 그냥 무엇인지 보여 드리는 것이 더 좋을 것 같습니다. 아마 이 과정은 30,000 피트로부터 React와 React 컴포넌트를 정신없이 여행하는 것 같겠죠. 제가 이 섹션에서 React를 보여줄 텐데, 아직 디테일을 모두 이해하려고 하지 마시길 바랍니다. 이어지는 개요에서 공개된 디테일을 자세히 풀어내기 위해 이 책 전체가 있는 것이기 때문이죠. 그냥 지금부터는 큰 개념을 잡도록 따라오기만 하시면 됩니다.

## React를 이용해 `<select>`와 비슷한 UI 컴포넌트 만들기

아래는 자식인 HTML의 `<option>` 요소들을 캡슐화하고 있는 HTML의 `<select>` 요소입니다. HTML의 `<select>` 요소의 생성과 기능은 이미 익숙하길 바랍니다.

> [source code](https://jsfiddle.net/s2pxp36L/#tabs=html,result)

브라우저가 위의 요소들의 트리를 분석하면, 선택될 수 있는 아이템들의 원문 리스트를 포함하고 있는 UI를 만듭니다. 브라우저가 무엇을 생성하는지 보려면, JSFiddle 위에 있는 "Result" 탭을 클릭하세요.

HTML의 `<select>`를 UI 컴포넌트로 바꾸기 위해, 배후에서 브라우저, 즉 [DOM](http://domenlightenment.com/)과 [그림자 DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Shadow_DOM)이 함께 일하고 있습니다. `<select>` 컴포넌트는 사용자가 선택을 하게 하고, 그 선택한 값을 state로 저장한다는 걸 기억하세요. (예: "Volvo"를 선택하면 당신은 "Mercedes" 대신 "Volve"를 선택한 것입니다.)

React를 이용하면, React 노드들을 사용해 결국에는 HTML DOM의 HTML 요소들이 되는 React 컴포넌트를 만들어 커스텀 `<select>`를 만들 수 있어요.

그럼 React를 이용해 UI 컴포넌트 같은 우리만의 `<select>`를 만들어 봅시다.

## React 컴포넌트 정의하기

아래에서는 `MySelect`라는 React 컴포넌트를 만들기 위해 `React.createClass` 함수를 불러서 React 컴포넌트를 만들고 있습니다.

보시다시피, `MySelect` 컴포넌트는 약간의 스타일과 빈 React 노드 요소인 `<div>`로 구성되어 있습니다.


```js
var MySelect = React.createClass({ //MySelect 컴포넌트 정의
    render: function(){
        var mySelectStyle = {
            border: '1px solid #999',
            display: 'inline-block',
            padding: '5px'
        };
        // JSX 안에서 JS 변수를 참조하기 위해서 {}를 사용
        return <div style={mySelectStyle}></div>; //JSX로 표현된 react div 요소
    }
});
```

저기의 `<div>`는 HTML 같은 태그이고, JavaScript에서는 [JSX](https://facebook.github.io/jsx/)라고 불립니다. JSX는 실제 HTML 요소, 커스텀 요소와 텍스트 노드로 연결될 수 있는 React 노드를 표현하기 위해 React가 쓰는 선택적인 맞춤 JavaScript 문법이에요. JSX를 이용해 정의된 React 노드가 HTML 요소와 1:1 매칭이 된다고 생각하면 안됩니다. 분명 [차이](https://facebook.github.io/react/docs/dom-differences.html)가 있고 몇몇의 [깨달음](https://facebook.github.io/react/docs/jsx-gotchas.html)도 있습니다.

JSX 문법은 ES5 JS 엔진이 분석하기 위해서는 반드시 JSX에서 실제 JavaScript로 변환되어야 합니다. 위의 코드가 변환되지 않는다면, 당연히 JavaScript 에러를 일으킬 것입니다.

JSX를 실제 JavaScript 코드롤 변환하는 데에 사용되는 공식 도구는 [Babel](http://babeljs.io/)이라고 불립니다.

Babel(또는 비슷한 무언가) 후에는 위의 코드에서 JSX의 `<div>`를 실제 JavaScript 코드로 변환합니다. 실제 JavaScript는 이처럼 생겼습니다:

```javascript
return React.createElement('div', { style: mySelectStyle });
```

이것 대신 말이죠:

```javascript
return <div style={mySelectStyle}></div>;
```

당분간 당신은 React 코드에서 HTML 같은 태그를 적으면 결국에는 ES6 문법에 따라 실제 JavaScript로 반드시 변환된다는 것만 기억하면 됩니다.

이 시점에서 `<MySelect>` 컴포넌트는 빈 React 노드 요소인 `<div>`로 구성되어 있습니다. 이건 조금 보잘 것 없는 컴포넌트이니, 바꿔봅시다.

저는 이제 `<MyOption>`이라 불리는 또 다른 컴포넌트를 정의하고 `<MyOption>` 컴포넌트를 `<MySelect>` 컴포넌트 안에서 사용할 것입니다. (구성이라고도 알려져 있죠.)

아래 React 컴포넌트인 `<MySelect>`와 `<MyOption>`를 정의한 새로운 JavaScript 코드를 검토해보세요.

```javascript
var MySelect = React.createClass({
    render: function(){
        var mySelectStyle = {
            border: '1px solid #999',
            display: 'inline-block',
            padding: '5px'
        };
        return ( //<MyOption> 컴포넌트를 담고 있는 JSX로 표현된 react div 요소
            <div style={mySelectStyle}>
                <MyOption value="Volvo"></MyOption>
                <MyOption value="Saab"></MyOption>
                <MyOption value="Mercedes"></MyOption>
                <MyOption value="Audi"></MyOption>
            </div>
        );
    }
});

var MyOption = React.createClass({  //MyOption 컴포넌트 정의
    render: function(){
        return <div>{this.props.value}</div>; //JSX로 표현된 react div 요소
    }
});
```

`<MyOption>` 컴포넌트가 `<MySelect>` 컴포넌트 안에서 어떻게 쓰였는지, 그리고 둘 다 JSX를 이용해 생성되었다는 점에 유의하셔야 합니다.

## React의 속성/props 이용해 컴포넌트의 옵션 전달하기

`<MyOption>` 컴포넌트는 `{this.props.value}` 표현을 담고 있는 하나의 `<div>`로 구성되어 있다는 점을 보세요. `{}` 괄호는 JavaScript 표현식이 사용되고 있다는 것을 JSX에게 알려줍니다. 다시 말해서, `{}` 안에는 JavaScript를 쓸 수 있습니다.

`{}` 괄호는 `<MyOption>` 컴포넌트로부터 전달된 특징이나 속성에 접근권(예: `this.props.value`)을 얻기 위해 사용됩니다. 바꿔 말하면, `<MyOption>` 컴포넌트가 렌더될 때, HTML 같은 속성(예: `value="Volvo"`)으로 전달된 `value` 옵션은 `<div>` 안에 위치하게 됩니다. 이런 HTML처럼 보이는 속성들은 React의 속성/[props](https://facebook.github.io/react-native/docs/props.html)으로 간주됩니다. React는 이것들을 이용해 컴포넌트에게 state가 없음/불변 옵션을 전달합니다. 이런 경우에, 우리는 간단하게 `value` prop을 `<MyOption>` 컴포넌트에게 전달합니다. JavaScript 함수에게 인수가 전달되는 방식과 다르지 않게 말이죠. 그리고 사실, 그것이 실제로 JSX 뒤에서 일어나고 있는 일입니다. 

## 컴포넌트를 가상 DOM에 렌더한 다음 HTML DOM에 렌더하기

현재 우리가 쓴 JavaScript는 딱 두 개의 React 컴포넌트만 정의하고 있습니다. 우린 아직 이 컴포넌트들을 가상 DOM 그리고 이어서 HTML DOM에 실제로 렌더해보지 않았습니다.

그걸 하기 전에, 여태까지 우리가 한 일은 JavaScript를 이용해 두 개의 React 컴포넌트를 정의한 것이 전부라는 점을 짚고 넘어가겠습니다. 이론 상으로는, 우리가 지금까지 쓴 JavaScript는 UI 컴포넌트의 정의일 뿐입니다. 이것은 엄밀히 말해서 꼭 DOM으로 가지 않아도 됩니다. 또는 심지어 가상 DOM으로도 말이죠. 이것과 똑같은 정의가 원칙적으로는 이 컴포넌트를 [네이티브 모바일 플랫폼](https://github.com/facebook/react-native) 또는 [HTML canvas](https://github.com/Flipboard/react-canvas)에 렌더하기 위해 사용될 수도 있습니다. 하지만 그게 가능하다고 해도 그걸 하지는 않을 것입니다. React는 DOM, 프론트엔드 어플리케이션과 [심지어 웹 플랫폼](https://facebook.github.io/react/docs/top-level-api.html#reactdomserver)까지 초월할 수 있는 UI를 구조화하는 패턴이라는 것을 유의하세요.

그럼 이제 `<MySelect>` 컴포넌트를 가상 DOM에 렌더해봅시다. 그럼 결국 가상 DOM은 HTML 페이지 내부의 실제의 DOM에서도 렌더해줄 것입니다.

아래의 JavaScript에 제가 마지막 줄에 `ReactDOM.render()` 함수를 부르는 부분을 추가한 걸 보시길 바랍니다. 여기서 저는 우리가 렌더하고 싶은 컴포넌트(예: `<MySelect>`)와 HTML DOM에 이미 있는 HTML 요소의 참조(예: `<div id="app"></div>`)를 `ReactDOM.render()` 함수에 전달하고 있습니다. 이 참조는 제가 React `<MySelect>` 컴포넌트를 렌더하고 싶은 위치입니다.

"Result" 탭을 클릭하면 우리의 커스텀 React `<MySelect>` 컴포넌트가 HTML DOM에 렌더된 것을 볼 수 있을 것입니다.

> [source code](https://jsfiddle.net/zp86ez31/#tabs=js,result,html,resources)

기억하세요. 저는 React에게 컴포넌트를 어디서부터 렌더해야 할지와 어떤 컴포넌트부터 시작해야 하는지를 말해준 것일 뿐입니다. 그러면, React는 시작하는 컴포넌트(예: `<MySelect>`)내에 포함되어 있는 자식 컴포넌트들(예: `<MyOption>`)을 모두 렌더할 것입니다.

잠깐만요, 혹시 이런 생각을 하고 있을지도 모르겠습니다. 우리가 어쨌든 실제로 `<select>`는 만들지 않았다고 말입니다. 우리는 텍스트 문자열의 정적/state가 없는 리스트를 생성한 것일 뿐입니다. 다음으로 그걸 고칠 예정입니다.

다음으로 넘어가기 전에, 한가지 짚고 넘어가고 싶습니다. 실제 DOM으로 `<MySelect>` 컴포넌트를 가져가기 위한 암시적인 DOM 상호작용은 전혀 적지 않았습니다. 다시 말해서, 이 컴포넌트를 만들면서 jQuery 코드는 호출된 적이 없습니다. 실제 DOM을 다루는 건 모두 React 가상 DOM에 의해 이루어졌습니다. 사실 React를 쓸 때, 당신이 하는 일은 가상 DOM을 서술하는 것입니다. 그럼 React는 당신을 위해 이 가상 DOM을 가지고 가서 실제 DOM을 만드는 데에 사용합니다.

## React의 state 사용하기

우리의 `<MySelect>` 컴포넌트가 네이티브의 `<select>` 요소를 따라하기 위해서는 우리가 state를 추가해야 합니다. 만약 선택한 값을 state로 저장할 수 없다면, 결국 좋은 건 커스텀 `<select>` 요소겠죠.

컴포넌트가 정보의 스냅샷을 포함하고 있다면, 일반적으로 state가 개입됩니다. 우리의 커스텀 `<MyOption>` 컴포넌트에 관해서, 이것의 state는 현재 선택된 텍스트거나 아무 텍스트도 선택되지 않았다는 사실입니다. state는 일반적으로 사용자의 이벤트(예: 마우스, 키보드, 클립보드 등)나 네트워크 이벤트(예: AJAX)와 관련되어 있고, UI가 다시 렌더해야 할 때 그 값을 사용합니다. (예: 값이 바뀌면, 다시 렌더합니다)

state는 UI 컴포넌트를 만드는 가장 상위의 컴포넌트에서 주로 발견됩니다. React의 `getInitialState()` 함수를 이용하면, `getInitialState`가 호출되었을 때 state 오브젝트를 반환(예: `return {selected: false};`)해 컴포넌트의 기본 state를 `false`(예: 아무것도 선택되지 않음)로 설정할 수 있습니다. `getInitialState` [라이프 사이클 메소드](https://facebook.github.io/react/docs/react-component.html#the-component-lifecycle)는 컴포넌트가 탑재되기 전에 한 번 호출됩니다. 반환 값은 `this.state`의 초기값으로 사용됩니다.

제가 컴포넌트에 state를 덧붙이는 것에 맞춰 아래의 코드를 고쳤습니다. 제가 코드에서 변한 부분이 눈에 잘 띄도록 JavaScript 주석을 추가했으니, 꼭 읽으시길 바랍니다.

```javascript
var MySelect = React.createClass({
    getInitialState: function(){ //selected를 추가, 기본 state
        return {selected: false}; //this.state.selected = false;
    },
    render: function(){
        var mySelectStyle = {
            border: '1px solid #999',
            display: 'inline-block',
            padding: '5px'
        };
        return (
            <div style={mySelectStyle}>
                <MyOption value="Volvo"></MyOption>
                <MyOption value="Saab"></MyOption>
                <MyOption value="Mercedes"></MyOption>
                <MyOption value="Audi"></MyOption>
            </div>
        );
    }
});

var MyOption = React.createClass({
    render: function(){
        return <div>{this.props.value}</div>;
    }
});

ReactDOM.render(<MySelect />, document.getElementById('app'));
```

기본 state 설정이 있으니, 다음으로 사용자가 옵션을 클릭하면 불리는 `select`라는 콜백 함수를 추가할 것입니다. 이 함수 안에서, (`event`라는 매개 변수를 통해) 선택 받은 옵션의 텍스트를 가져오고, 그걸 이용해 현재 컴포넌트에서 어떻게 `setState`를 할지 결정합니다. 제가 `select` 콜백으로 전달된 `event`의 디테일을 이용하고 있다는 걸 기억하세요. 이 패턴은 당신이 jQuery와 경험이 한 번이라도 있다면 친숙해 보일 것입니다.


```javascript
var MySelect = React.createClass({
    getInitialState: function(){
        return {selected: false};
    },
    select:function(event){//추가된 select 함수
        if(event.target.textContent === this.state.selected){//선택된 것 제거
            this.setState({selected: false}); //state 갱신
        }else{//선택된 것 추가
            this.setState({selected: event.target.textContent}); //state 갱신
        }   
    },
    render: function(){
        var mySelectStyle = {
            border: '1px solid #999',
            display: 'inline-block',
            padding: '5px'
        };
        return (
            <div style={mySelectStyle}>
                <MyOption value="Volvo"></MyOption>
                <MyOption value="Saab"></MyOption>
                <MyOption value="Mercedes"></MyOption>
                <MyOption value="Audi"></MyOption>
            </div>
        );
    }
});

var MyOption = React.createClass({
    render: function(){
        return <div>{this.props.value}</div>;
    }
});

ReactDOM.render(<MySelect />, document.getElementById('app'));
```

우리의 `<MyOption>` 컴포넌트가 `select` 함수에 접근 권한을 얻기 위해서, 우리는 props를 이용해 `<MySelect>` 컴포넌트에서 `<MyOption>` 컴포넌트로 참조를 전달해야 합니다. 이것을 하기 위해서 우리는 `<MyOption>` 컴포넌트에 `select={this.select}`를 추가합니다.

그것이 준비되면, 우리는 `<MyOption>` 컴포넌트에 `onClick={this.props.select}`를 추가할 수 있습니다. 우리가 한 일은 `select` 함수를 부르는 `click` 이벤트를 연결한 것일 뿐이라는 점이 분명하게 설명되었길 바랍니다. React는 실제 DOM에서 일어나는 클릭 핸들러를 연결하는 일을 당신을 대신해서 처리해줍니다.

```javascript
var MySelect = React.createClass({
    getInitialState: function(){
        return {selected: false};
    },
    select:function(event){
        if(event.target.textContent === this.state.selected){
            this.setState({selected: false});
        }else{
            this.setState({selected: event.target.textContent});
        }   
    },
    render: function(){
        var mySelectStyle = {
            border: '1px solid #999',
            display: 'inline-block',
            padding: '5px'
        };
        return (//props를 사용해 <MyOption>의 select 콜백에 참조 전달
            <div style={mySelectStyle}>
                <MyOption select={this.select} value="Volvo"></MyOption>
                <MyOption select={this.select} value="Saab"></MyOption>
                <MyOption select={this.select} value="Mercedes"></MyOption>
                <MyOption select={this.select} value="Audi"></MyOption>
            </div>
        );
    }
});

var MyOption = React.createClass({
    render: function(){//select 콜백을 호출할 이벤트 핸들러를 추가
        return <div onClick={this.props.select}>{this.props.value}</div>;
    }
});

ReactDOM.render(<MySelect />, document.getElementById('app'));
```

이 모든 걸 한 덕분에, 우리는 이제 옵션 하나를 클릭하는 것으로 state를 설정할 수 있습니다. 다시 말해서, 옵션을 클릭하면 이제 `select` 함수가 실행되서 `MySelect` 컴포넌트의 state를 설정할 것입니다. 하지만 컴포넌트의 사용자는 이것들이 일어나는지 알 수 없습니다. 왜냐하면 우리가 한 일은 컴포넌트 안에서 state가 관리되게끔 우리의 코드를 갱신한 것뿐이기 때문이죠. 지금은 무엇이 선택되었는지 시각적인 피드백이 전혀 없습니다. 이걸 고쳐봅시다.

다음으로 해야 할 일은 현재 state를 `<MyOption>` 컴포넌트로 전달해 컴포넌트의 state에 따라 시각적으로 반응할 수 있게 하는 것입니다.

다시 props를 사용해, 모든 `<MyOption>` 컴포넌트에 property로 `state={this.state.selected}`를 위치시켜, `selected` state를 `<MySelect>` 컴포넌트에서 `<MyOption>` 컴포넌트로 넘겨주겠습니다. 이제 우리는 state(예: `this.props.state`)와 옵션의 현재 값(예: `this.props.value`)을 알기 때문에 state가 주어진 `<MyOption>` 컴포넌트의 값과 일치하는지 확인할 수 있습니다. 일치하는 경우에는 이 옵션이 선택된 것이라는 걸 알 것입니다. 이것을 구현하기 위해서, state가 현재 옵션의 값과 일치하면 JSX `<div>`에 스타일이 입혀진 선택된 state(예: `selectedStyle`)를 추가하는 간단한 `if` 선언문을 적으면 됩니다. 일치하지 않는다면, `unSelectedStyle` 스타일이 붙은 React 요소를 반환합니다.

> [source code](https://jsfiddle.net/L1z9za23/#tabs=js,result,html,resources)

새로운 기능을 확인하려면, 위에 있는 "Result" 탭을 클릭하고 React 선택 컴포넌트를 사용하세요.

우리의 React UI 선택 컴포넌트가 당신이 기대했던 만큼 예쁘거나 완성도 있는 건 아니겠지만, 이 모든 것들이 무엇을 가르쳐주기 위한 것인지 당신이 이해하고 있을 것이라고 생각합니다. React는 당신이 구조 트리(예: 컴포넌트들의 트리) 안에서 state가 없거나 state를 저장하는 UI 컴포넌트를 추론하고 구성하고 유지하는 것을 도와주는 도구입니다.

가상 DOM의 역할로 넘어가기 전에, 당신이 꼭 JSX나 Babel을 쓰지 않아도 된다고 강조하고 싶습니다. 항상 이런 도구들을 건너뛰고 그냥 직접 JavaScript를 적을 수 있습니다. 아래에서 JSX가 Babel을 통해 변환된 후의 최종 코드 상태를 보여주고 있습니다. 당신이 JSX를 쓰지 않기로 결정했다면, 제가 이 섹션들을 통해 적은 코드 대신 스스로 아래의 코드를 적어야 합니다.

```javascript
var MySelect = React.createClass({
  displayName: 'MySelect',

  getInitialState: function getInitialState() {
    return { selected: false };
  },
  select: function select(event) {
    if (event.target.textContent === this.state.selected) {
      this.setState({ selected: false });
    } else {
      this.setState({ selected: event.target.textContent });
    }
  },
  render: function render() {
    var mySelectStyle = {
      border: '1px solid #999',
      display: 'inline-block',
      padding: '5px'
    };
    return React.createElement(
      'div',
      { style: mySelectStyle },
      React.createElement(MyOption, { state: this.state.selected, select: this.select, value: 'Volvo' }),
      React.createElement(MyOption, { state: this.state.selected, select: this.select, value: 'Saab' }),
      React.createElement(MyOption, { state: this.state.selected, select: this.select, value: 'Mercedes' }),
      React.createElement(MyOption, { state: this.state.selected, select: this.select, value: 'Audi' })
    );
  }
});

var MyOption = React.createClass({
  displayName: 'MyOption',

  render: function render() {
    var selectedStyle = { backgroundColor: 'red', color: '#fff', cursor: 'pointer' };
    var unSelectedStyle = { cursor: 'pointer' };
    if (this.props.value === this.props.state) {
      return React.createElement(
        'div',
        { style: selectedStyle, onClick: this.props.select },
        this.props.value
      );
    } else {
      return React.createElement(
        'div',
        { style: unSelectedStyle, onClick: this.props.select },
        this.props.value
      );
    }
  }
});

ReactDOM.render(React.createElement(MySelect, null), document.getElementById('app'));
```

## 가상 DOM의 역할 이해하기

저는 대부분의 사람들이 일반적으로 React에 관해 이야기를 시작할 때 언급하는 부분에서 이 정신없던 여행을 마무리하려 합니다. 즉, React 가상 DOM의 가치에 대해 이야기하며 React의 개요를 마치겠습니다.

눈치채셨겠지만, 우리가 커스텀 select UI를 생성하기 위한 과정 중 실제 DOM과 유일하게 상호작용했던 때는 HTML 페이지에서 우리의 UI 컴포넌트를 어디로 렌더할지 `ReactDOM.render()` 함수에게 말해줄 시점이었습니다. (예: 이것을 `<div id="app"></div>`에 렌더하라) 아마 이것이 React 컴포넌트의 트리로부터 React 어플리케이션을 만들 때 당신이 실제 DOM과 가진 유일한 상호작용일 것입니다. 그리고 여기에 상당수의 React의 가치가 담겨있습니다. React를 이용하면, 당신은 jQuery 코드를 적을 때 해봤던 것처럼 DOM에 대해 정말 생각하지 않아도 됩니다. React는 전부, 혹시 아니라면 대부분의 암시적인 DOM 상호작용을 당신의 코드로부터 제거해 DOM에 대해 완전히 추상적 개념을 적용하며 jQuery를 대체합니다. 물론 이것이 유일한 이득, 심지어는 최고의 이득이 아닙니다.

DOM이 가상 DOM으로 완전히 추상화되었고, 이는 state가 변하면 실제 DOM을 갱신하는 가혹한 퍼포먼스 패턴을 감안한 결과라고 할 수 있습니다. 가상 DOM은 state와 props를 기반으로 UI 변화를 계속 파악하고 있고, 그 후 실제 DOM과 비교해 UI를 갱신하는데 필요한 최소한의 변화만 수행합니다. 다시 말해서, 실제 DOM은 state 또는 props가 변하면 필요한 최소한의 변화만 가지고 패치하게 됩니다.

실시간으로 이런 성능 기준에 맞는 갱신을 이해하는 것은 종종 성능 기준에 맞는 DOM 차이와 관련해 잘 모르는 부분을 분명히 해줍니다. 아래의 움직이는 이미지는 우리가 이번 챕터에서 만든 UI 컴포넌트의 용도를 보여주고 있습니다. (예: state 바꾸기)

![](images/XFEJxkXPVs.gif "images/XFEJxkXPVs.gif")

UI 컴포넌트가 state를 바꾸면 실제 DOM은 최소한으로 필요한 변화만 이루어지고 있다는 걸 기억하시기 바랍니다. 우리는 React가 그들의 역할을 하고 있다는 걸 알 수 있습니다. 왜냐하면 초록색 윤곽/배경이 있는 부분만이 실제 DOM에서 실제로 갱신되고 있는 부분이기 때문이죠. UI 컴포넌트의 전체는 각 state가 변할 때마다 갱신되고 있지 않습니다. 변경이 필요한 부분만 갱신되고 있죠.

확실하게 짚고 넘어가겠습니다. 이건 혁신적인 개념은 아니에요. 조심스럽게 제작하고 성능 기준에 맞게끔 설계된 jQuery 코드로도 같은 효과를 낼 수 있습니다. 하지만 React를 이용하면, 있을지는 모르겠지만, 아주 가끔만 이것에 대해 생각하면 됩니다. 가상 DOM이 당신을 위해 모든 퍼포먼스 작업을 해주고 있습니다. 그런 의미에서, 이것은 가능한 jQuery/DOM 추상화의 최고의 유형입니다. 당신이 DOM에 대해 전혀 걱정하거나 코드를 적지 않아도 되는 유형이죠. 당신은 DOM과 암시적으로 상호작용하지 않아도 되고 모든 일은 배후에서 일어나게 됩니다.

React가 jQuery 같은 무언가에 대한 필요성을 거의 제거한 사실에 담겨있는 React 가치에 대해 생각하며, 이제 이 개요를 떠날 때가 되었다고 생각할 수 있겠네요. 그리고 암묵적인 jQuery 코드와 비교하면 가상 DOM은 분명 엄청난 이점이지만, React의 가치가 가상 DOM에만 있는 것은 아닙니다. 가상 DOM이 있기에 더 좋은 것일 뿐입니다. 간단히 말하자면, React의 가치는 UI 컴포넌트 트리를 생성할 수 있는 간단하고 지속가능한 패턴을 제공한다는 사실에 기초한 것입니다. 재사용할 수 있는 React 컴포넌트만을 이용해 당신의 어플리케이션의 인터페이스 전체를 정의하는 것으로 UI를 프로그래밍하는 것이 얼마나 간단해질 수 있는지 한 번 상상해보세요.
