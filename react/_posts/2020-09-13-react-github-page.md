---

title:  "[React] React app을 github page에 배포하는 법"
---

## React app 만들기
- [React 설치방법]({% post_url 2020-09-13-react-create-app %}) 참고

## git repo 생성
- [github](https://github.com)에 접속하여 로그인한다.
- Repositories 옆의 `New`를 눌러 Repository를 만든다.
  - Repository name는 app name과 맞추면 된다.
  - 다른 것은 설정하지 않고 `Create repository`를 클릭한다.
- bash나 cmd에서 react app 디렉토리로 이동한다.
  ```bash
  $ cd myapp
  ```
- USERNAME과 REPONAME은 자신의 설정에 맞게 수정한 후 추가한다.(git이 없으면 [git-scm](https://git-scm.com/downloads)에서 설치한다)
  ```bash
  $ git remote add origin git@github.com:{USERNAME}/{REPONAME}.git
  $ git push --set-origin origin master
  ```

## gh-pages
gh-pages를 설치한다.  

```bash
$ npm i gh-pages
```

## package.json
`package.json`을 수정한다.  
"scripts"에 `deploy`와 `predeploy`를 추가한다.  
`homepage`는 자신의 USERNAME과 APPNAME에 맞게 수정하여 추가한다.  

```json
  ...
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "deploy": "gh-pages -d build",
    "predeploy": "npm run build"
  },
  ...
  },
  "homepage": "https://{USERNAME}.github.io/{APPNAME}"
```

## 배포
`npm run deploy`로 배포한다.  
`deploy` 이전에 `predeploy`가 호출되고 `predeploy`는 `build`를 호출한다.  

```bash
$ npm run deploy
```

`https://{USERNAME}.github.io/{APPNAME}`로 접속하면 앱이 배포되었음을 확인할 수 있다.  


## 참고 사이트
- [노마드코더: ReactJS로 영화 웹 서비스 만들기](https://nomadcoders.co/react-fundamentals/lobby)
