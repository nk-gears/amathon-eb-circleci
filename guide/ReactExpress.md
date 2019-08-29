# React + Express Part 1

이번 세션에서는 **Front-End**와 **Back-end**를 하나의 repository에 두고 간단한 웹을 구성해볼게요.

## 0️⃣ 프로젝트 시작하기
우리가 만들고자하는 **Application의 구조**는 다음과 같습니다. 

```
amathon
|__ client
    |__ ...
|__ server.js
|__ package.json
|__ package-lock.json
```

위 구조에 따라 만들어보도록 합시다.

- Front-end: [create-react-app](https://github.com/facebook/create-react-app)을 통해 React App 구성

- Back-end: [express server](https://expressjs.com/ko/)

<br>

[세션 시작 전 가이드](../guide/Git.md)에서 생성한 repository의 경로로 이동해주세요. 이제, NodeJS로 app을 세팅하고 필요한 dependencies를 설치해보겠습니다. 각자의 기호에 따라 yarn을 사용하거나 npm을 사용하세요. 저는 yarn을 사랑합니다. [(npm vs yarn cheat sheet)](https://shift.infinite.red/npm-vs-yarn-cheat-sheet-8755b092e5cc)

```shell
# 초기화
$ yarn init
또는
$ npm init

# Dependencies 설치
$ yarn add express cors
또는
$ npm install express cors --save

# server.js 생성
$ touch server.js
```

> 🚧 window에서는 touch 명령어를 사용할 수 없습니다. 따라서, 텍스트 에디터 (vscode) 혹은 해당 폴더 경로에서 새 파일 `server.js` 를 만들어주세요. 

<br>

## 1️⃣ Back-end (Express server)

Express server가 동작하도록 방금 생성한 **server.js**에 간단히 코드를 작성해봅시다.

**server.js**

```js
const express = require('express');
const PORT = process.env.HTTP_PORT || 4001;
const cors = require('cors');
const app = express();
app.use(cors())

app.listen(PORT, () => {
  console.log(`Server listening at port ${PORT}.`);
})
```

<br>

필요한 패키지들을 **devDependencies**로 설치해줍시다.

```shell
$ yarn add @babel/cli @babel/core @babel/node @babel/preset-env nodemon --dev
또는
$ npm install @babel/cli @babel/core @babel/node @babel/preset-env nodemon --save-dev
```

> 🚧 설치하는 과정에서 오류가 날 경우, npm 버전이 최신 버전인지 확인하고, global로 설치해보세요.`npm i -g nodemon`
>

<br>

#### 🤙 Babel?

>  Babel은 자바스크립트 표준인 ECMAScript(이하 ES)의 최신 문법으로 작성된 코드를 실행할 수 있도록 이전 버전 문법으로 변환해주는 트랜스파일러입니다. ES6/ES7 코드를 ECMAScript5 코드로 트랜스파일링해줍니다.   

<br>

#### 🤙 Nodemon?

>  Nodemon이란 디렉토리내의 파일이 수정될 경우, 자동으로 애플리케이션을 재시작해주는 도구입니다.

<br>

**package.json**

```json
{
	...,
	"scripts": {
  	...,
    "start": "nodemon --exec babel-node server.js"
  },
  ...
}
```

<br>

터미널에서 `yarn start` (혹은 `npm start`) 를 입력하면, console에 `Server listening at port 4001` 라고 적힌 것을 확인할 수 있습니다.

> 🚧 `[nodemon] Internal watch failed: watch ENOSPC` 와 같은 오류가 날 경우, [여기](https://github.com/seohyun0120/amathon-eb-circleci/issues/6)를 참고해주세요.

<br>
<br>

## 2️⃣ Front-end (CRA)

이제 클라이언트를 세팅해보도록 합시다. 해당 프로젝트 root 디렉토리내에서 `client`라는 디렉토리를 만들고, `create-react-app` 을 통해 **React App**을 만들어봅시다.

<br>

#### 🤙 CRA(create-react-app) ?

>  페이스북에서 만든 react 웹 개발용 boilerplate입니다. 직접 환경을 세팅할 필요없이 간단한 앱을 만들 수 있습니다.

<br>

```bash
# 현재 경로: ~/amathon

$ yarn create react-app client
또는
# npm 5.2+ 
$ npx create-react-app client
```

![1](./pic/1.png)

(가이드 사진마다 root 경로가 조금씩 다를 수 있습니다. 2번 이상 test하며 가이드를 수정했기에 다를 수 있어요 😀 본인이 프로젝트를 시작한 경로에서 잘 따라와주시면 됩니다!)

````bash
$ cd client
$ yarn start
또는
$ npm start
````

<br>

`localhost:3000` 에서 다음과 같은 화면이 뜬다면 성공입니다.

> 🚧  `sh: 1: react-scripts: not found` 와 같은 에러 발생할 경우, client 폴더 내의 nodeModules를 삭제하고 client를 다시 시작해주세요.

![2](./pic/2.png)

<br>

조금 더 심플하게 만들기 위해 필요없는 파일은 지워보도록 하겠습니다. **client** 폴더 내의 필요없는 파일을 전부 지워서, 아래와 같은 구조만 남겨주세요.

```
client
├── README.md
├── node_modules
├── package.json
├── yarn.lock
├── .gitignore
├── public
│   └── index.html
└── src
    ├── App.css
    ├── App.js
    ├── index.css
    └── index.js
```

![3](./pic/3.png)

<br>

코드를 조금 심플하게 수정해봅시다.

**client/public/index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

<br>

**client/src/App.js**

```js
import React from 'react';
import './App.css';

class App extends React.Component {
  render() {
    return (
      <div className="App">
        <h1>Hello Stranger?</h1>
      </div>
    )
  }
}

export default App;
```

<br>

**client/src/App.css**

```css
.App {
  text-align: center;
}
```

<br>

**client/src/index.js**

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```
<br>

```shell
$ yarn start
또는
$ npm start
```

<br>

![4](./pic/4.png)
다음과 같은 화면이 나온다면 성공입니다! 기본 세팅은 완료했으니, 본격적으로 Simple React App을 만들어봅시다.  [React + Express Part 2](./ReactExpress_2.md)로 이동해봅시다.

