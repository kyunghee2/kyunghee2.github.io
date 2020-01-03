---
layout: post
title:  "React Introduce"
date:   2020-01-02
# desc: ""
keywords: "react"
categories: [React]
tags: [React]
icon: icon-react
---

## React

> 리액트(React)는 페이스북이 개발하고 있는 UI 라이브러리 입니다.
>
> 웹 페이지 내부의 각 부분을 컴포넌트로 다룰 수 있게 해주며, 컴포넌트를 HTML태그로 작성할 수 있게 해주는 JSX를 사용하며, 가상 DOM(Virtual DOM)을 사용해 렌더링 성능을 극대화 함

### 개발환경

- VS Code
- VS Code - Extensions 탭 - simple react snippets 설치
- Node.js 설치 (react 자동 빌드환경 설정시 필요)

![image-20191227220318622](/static/assets/img/blog/react/image-20191227220318622.png)



### 리액트 시작하기

```react
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>

  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.38/browser.min.js"></script>
<body>
  <div id="root"></div>

  <script type="text/babel">
  //리액트로 DOM의 내용을 변경
  ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
  </script>
</body>
</html>
```



### 리액트 컴포넌트 생성

1. HTML 파일에 DOM 요소 추가

   ```html
   <!-- ... existing HTML ... -->
   
   <div id="like_button_container"></div>
   
   <!-- ... existing HTML ... -->
   ```

   

2. 스크립트 태그 추가

   - 사이트를 배포할 때는 "development.js"를 "production.min.js"로 대체
   
   ```html
    ...
     <!-- React를 실행. -->
     <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
     <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
   
     <!-- 만든 React 컴포넌트를 실행. -->
     <script src="like_button.js"></script>
   
   </body>
   ```

3. React 컴포넌트 생성

   - like_button.js 파일생성

   ```react
   'use strict';
   
   const e = React.createElement;
   
   class LikeButton extends React.Component {
     constructor(props) {
       super(props);
       this.state = { liked: false };
     }
   
     render() {
       if (this.state.liked) {
         return 'You liked comment number ' + this.props.commentID;
       }
   
       return e(
         'button',
         { onClick: () => this.setState({ liked: true }) },
         'Like'
       );
     }
   }
   
   // Find all DOM containers, and render Like buttons into them.
   document.querySelectorAll('.like_button_container')
     .forEach(domContainer => {
       // Read the comment ID from a data-* attribute.
       const commentID = parseInt(domContainer.dataset.commentid, 10);
       ReactDOM.render(
         e(LikeButton, { commentID: commentID }),
         domContainer
       );
     });
   ```

   

- index.html 전체소스

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Add React in One Minute</title>
  </head>
  <body>

    <h2>Add React in One Minute</h2>
    <p>This page demonstrates using React with no build tooling.</p>
    <p>React is loaded as a script tag.</p>

    <p>
      This is the first comment.
      <!-- We will put our React component inside this div. -->
      <div class="like_button_container" data-commentid="1"></div>
    </p>

    <p>
      This is the second comment.
      <!-- We will put our React component inside this div. -->
      <div class="like_button_container" data-commentid="2"></div>
    </p>

    <p>
      This is the third comment.
      <!-- We will put our React component inside this div. -->
      <div class="like_button_container" data-commentid="3"></div>
    </p>

    <!-- Load React. -->
    <!-- Note: when deploying, replace "development.js" with "production.min.js". -->
    <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>

    <!-- Load our React component. -->
    <script src="like_button.js"></script>

  </body>
</html>
```

### 리액트와 JSX의 관계

#### JSX 태그 내부에 변수를 넣는 방법

```react
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.38/browser.min.js"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
  const title = "서예"
  const imgUrl = "http://www.dobongn.kr/wp-content/uploads/1/cfile1.uf.11726F334F432C1815EEF9.jpg"
  //jsx로 요소 정의
  const msg = <div>
                <h1>{title}</h1>
                <p><img src={imgUrl} /></p>
              </div>
  //render()로 렌더링
  const elm  =document.getElementById("root")
  ReactDOM.render(msg,elm)
  </script>
</body>
</html>
```

#### JSX로 스타일 속성 지정하기

- 스타일 속성을 객체로 지정

```
const obj = {prop1:value1, prop2:value2, prop3:value3...}
const dom = <tag style={obj}...</tag>
```

- 실습

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.38/browser.min.js"></script>
</head>
<body>
  <div id="root"></div>

  <script type="text/babel">
  ReactDOM.render(
  getDOM(),
  document.getElementById('root')
);
//요소를 반환하는 함수
function getDOM(){
  const css1 ={
    "color":'red',
    "background-color":'#f0f0ff',
    "font-size":'2em'
  }
  const css2={
    color:'blue',
    backgroundColor:'#fff0f0',
    fontSize:'2em'
  }
  return (
    <div>
      <p style={css1}>죄는 미워하되 사람은 미워하지 말라</p>
      <p style={css2}>이것 또한 지나가리라</p>
    </div>
  )
}
  </script>
</body>
</html>
```



### 가상 DOM 

> 가상 DOM은 DOM의 상태를 메모리에 저장하고 변경 전과 변경 후의 상태를 비교한 뒤 필요한 최소한의 내용만 반영하는 기능 입니다.

> DOM 변경을 최소한으로 만들어 주므로 성능 상승시켜줌

### 리액트로 컴포넌트 만들기

- 컴포넌트(Component)란 특정 기능을 가진 범용적인 '부품'을 나타내는 용어. 소프트웨어 개발을 할 때는 일반적으로 수많은 컴포넌트를 조합하게 됩니다.

#### 리스트 컴포넌트

```react
...
<script type="text/babel">
  //리스트컴포넌트 정의
  
  class RList extends React.Component {
    render(){
      const items = this.props.items.split(",")
      const itemsObj = items.map(
        (e)=>{
          return <li>{e}</li>
        })
      let title = this.props.title
      if(!title) title="LIST"
      //렌더링할 내용을 반환
      return (
        <div>
          <h3>{title}</h3>
          <ul>{itemsObj}</ul>
        </div>
      )
    }
    
  }
  //컴포넌트 출력
  ReactDOM.render(
      <RList title="Colors" items="Red,Green,Blue,White" />,
      document.getElementById('root')
    )
  </script>
```



#### 화살표 함수로 컴포넌트 정의

```react
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.38/browser.min.js"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
  const TitleParts = (props) => (
    <div style={{backgroundColor:'blue',color:'white'}}>
      <h3>{props.title}</h3>
    </div>
  )
  const ContentParts = (props) => (
    <div>
      <div>줄거리:{props.body}</div>
    </div>
  )
  //메인 컴포넌트
  const Book = (props) => (
    <div>
      <TitleParts title={props.title}/>
      <ContentParts body={props.body}/>
    </div>
  )
  //리액트로 DOM의 내용을 변경
  ReactDOM.render(
    (<div>
      <Book title='삼국지' body='옛날 중국 이야기'/>
      <Book title='민수기' body='옛날 이스라엘 이야기'/>
      <Book title='멋진신세계' body='미래 이야기'/>
    </div>),
    document.getElementById('root')
  )
  </script>
</body>
</html>
```

### 이벤트의 구조와 구현

- 리액트에서 클릭 이벤트 지정하는 방법

```
<div onClick={clickHandler}>Click Me</div>
```

- 클릭 이벤트 구현

```react
...
<body>
  <div id="root"></div>
  <script type="text/babel">
  //컴포넌트 정의
  class Hello extends React.Component {
    render(){
      //이벤트 정의
      const clickHandler = (e) =>{
        const name = this.props.name
        window.alert(`${name}님 안녕하세요`)
      }
      //클릭이벤트 지정
      return (
        <div onClick={clickHandler}>Click Me</div>
      )
    }
  }
  //컴포넌트 사용
  ReactDOM.render(
    <Hello name='헬로'/>,
    document.getElementById('root')
  )
  </script>
</body>
```

- 체크박스 컴포넌트 구현

```react
...
<body>
  <div id="root"></div>
  <script type="text/babel">
  class CBox extends React.Component {
    //생성자
    constructor(props){
      super(props)
      //상태초기화
      this.state = {checked:false}
    }
    render(){
      let mark = '□'
      let bstyle = {fontWeight:'normal'}
      if(this.state.checked){
        mark = '■'
        bstyle = {fontWeight:'bold'}
      }
      const clickHandler = (e)=>{
        const newValue = !this.state.checked
        this.setState({checked: newValue})
      }
      return (
        <div onClick={clickHandler} style={bstyle}>
          {mark}{this.props.label}
        </div>
      )
    }
      
  }
    const dom = <div>
      <CBox label="Apple"/>
      <CBox label="Banana"/>
      <CBox label="Orange"/>
      <CBox label="Mango"/>
    </div>

    ReactDOM.render(dom, document.getElementById('root'))
  </script>
</body>
```

![image-20191228000652127](/static/assets/img/blog/react/image-20191228000652127.png)

### 리액트 도구를 사용한 자동빌드

- create-react-app 설치

```bash
$ npm install -g create-react-app
```
- hello 프로젝트 생성
```bash
$ create-react-app hello
```

- 어플리케이션 실행

```bash
$ cd hello
$ npm start
```



![image-20191228001210411](/static/assets/img/blog/react/image-20191228001210411.png)

- 디렉터리 구조
  - node_modules : 설치한 모듈이 들어있는 디렉터리
  - src: 프로그램 소스코드
  - public:기본 골격 파일

![image-20191228001411592](/static/assets/img/blog/react/image-20191228001411592.png)

### 프로그램 공개하기

- 다음 명령어를 실행해 공개 전용 파일을 생성
- build 라는 디렉터리가 생성되며 해당 디렉터리 내부에 다양한 파일이 압축된 상태로 생성됨

```bash
$ npm run build
```

- 빌드가 제대로 되었는지 확인하려면 웹서버가 필요합니다. 다음과 같은 명령어로 serve명령어를 설치

```bash
$ npm install -g serve

$ serve -s build #build디렉터리를 서버의 루트 디렉터리로 실행
```

![image-20191228120930970](/static/assets/img/blog/react/image-20191228120930970.png)



### 컴포넌트의 라이프사이클

![image-20191228230711298](/static/assets/img/blog/react/image-20191228230711298.png)

#### 컴포넌트 생성과 DOM에 마운트

- constructor(props) - 객체가 생성될 때
- componentWillMount() - 컴포넌트가 DOM에 마운트되기 직전
- render() - 컴포넌트가 렌더링될 때
- componentDidMount() - 컴포넌트가 DOM에 마운트된 직후

#### 컴포넌트 업데이트

- componentWillReceiveProps(nextProps) - 컴포넌트의 프로퍼티가 변경될 때
- shouldComponentUpdate(nextProps, nextState) - 컴포넌트의 외관을 변경해도 좋을지 판단할 때
- componentWillUpdate() - 컴포넌트 업데이트 되기 직전
- render() - 컴포넌트가 렌더링될 때
- componentDidUpdate()

- componentWillUnmount() - 컴포넌트가 DOM에서 제거될때