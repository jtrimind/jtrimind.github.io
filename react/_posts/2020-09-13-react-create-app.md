---

title:  "[React] React 설치방법"
---

## Node
nodejs를 설치한다.  
- [Node.js 홈페이지](https://nodejs.org/ko/)

설치가 확인된 것을 확인하고 싶으면 `node -v`를 입력해서 설치된 버전을 확인한다.  
```bash
$ node -v
v12.18.3
```

## NPM
nodejs를 설치하면 npm도 같이 설치된다.  
npm이 설치되었는지 확인하려면 아래 명령어를 입력한다.  

```bash
$ npm -v
6.14.6
```

## NPX
npm을 이용하여 npx를 설치한다.  

```bash
$ npm install npx -g
```

npx가 설치되었는지 확인하려면 아래 명령어를 입력한다.  
```bash
$ npx -v
6.14.6
```

## create-react-app
`myapp`이라는 이름으로 리액트 앱를 설치해본다.  
```bash
$ npx create-react-app myapp
```

## react app 실행
리액트 앱이 있는 디렉토리로 이동하여 `npm start`로 실행시킨다.  
http://localhost:3000/ 에 접속하면 리액트 앱이 동작한다.  
```bash
$ cd myapp
$ npm start
```

## 참고 사이트
- [노마드코더: ReactJS로 영화 웹 서비스 만들기](https://nomadcoders.co/react-fundamentals/lobby)
