---
id: 5
title: "TypeScript + Node.js Express 프로젝트 설정하기"
subtitle: "TypeScript 초보자 프로젝트 설정해 보기"
date: "2021.02.09"
tags: "TypeScript"
---
안녕하세요, 박현상입니다. 요즘 저는 TypeScript와 Node.js Express에 대해서 공부하고 있습니다.  
어떻게 TypeScript와 Node.js Express를 어떻게 프로젝트 설정하고 TypeScript와 Node.js Express를 빌드하고 실행하는지에 대해서 알아 보겠습니다.

# TypeScript 프로젝트를 설정하기 전에 필요한 건?
TypeScript 프로젝트를 설정하기 전에 필요한 것은 역시 [Node.js](https://nodejs.org/en/)와 [npm](https://www.npmjs.com/)이 필요합니다.  
TypeScript를 사용하기 전에 JavaScript에 대한 기본적인 문법에 대해서 알고 있으셔야 합니다. JavaScript를 사용해 보시고 TypeScript에 대해서 공부하시기 전에 JavaScript 문법을 모르시면 [생활코딩-JavaScript](https://opentutorials.org/course/743) 강의를 보고 오시는 것을 추천 드립니다.

# 본격적으로 TypeScript 프로젝트 설정하기
TypeScript를 본격적으로 프로젝트 설정해 보겠습니다.

```shell
$ mkdir TypeScript-Project
$ cd TypeScript-Project
```
기본적인 프로젝트 폴더를 생성하여서 프로젝트 폴더에 들어가 주시면 됩니다.
## TypeScript Package 설치하기
```shell
$ npm init -y
$ npm install -g typescript
$ tsc --init
$ npm install -d ts-node
$ npm install -d ts-node-dev
$ npm install -d --save nodemon
$ npm install -d --save express
$ npm install @types/exprss
$ npm install -d @types/node
```
```npm init -y```를 통해서 npm 패키지에 관한 정보와 버전을 설정할 수 있는 ```package.json```를 만들겠습니다.  
```tsc --init```를 통해서 TypeScript 프로젝트에 필요한 ```tsconfig.json```를 생성 시켜 주시면 됩니다.

그리고 각종 필요한 패키지들을 설치해 주시면 됩니다. 기존에 있던 JavaScript로 Node.js와 Node.js Express를 설치하시게 되면 안 될 수도 있습니다. TypeScript에서는 ```@types/exprss```와 ```@types/node```를 설치해 주시면 됩니다.  

### 기존 JavaScript는 어떻게 실행 되었을까?
기존에 JavaScript의 경우에는 추가 빌드가 필요없이 바로 실행을 하실 수 있었습니다.  
예를 들어 보겠습니다.

```js
// JavaScript => main.js
console.log("Hello World!);
```

기존에 코드는 JavaScript 파일은 편하게 정의가 되어 있습니다. 따라서 추가적인 빌드가 없이 실행할 수 있습니다. 실행 방법은 아래와 같습니다.

```shell
$ node main.js
Hello World!
```
위와 같이 JavaScript 파일을 Node.js를 통해서 간편하게 실행 시킬 수 있으십니다.

### TypeScript를 어떻게 빌드와 실행 시킬 수 있을까?
```json
  "scripts": {
    "start": "ts-node-dev --respawn src/index.ts",
    "dev": "nodemon --exec ts-node src/index.ts",
    "bulid": "tsc"
}
```

```package.json```에 ```script```에 ```start, dev, bulid``` 명령어를 위 코드를 추가해 주시면 손 쉽게 빌드하고 실행시킬 수 있습니다.

```shell
$ npm run build
$ npm run dev
$ npm run start
```

이제 ```package.json```에 명령어를 잘 추가하셨다면 빌드와 실행이 완벽하게 될 겁니다.

```npm run bulid```를 통해서 TypeScript 코드를 JavaScript 코드로 변환할 수 있습니다.  

이제 실행시켜 보겠습니다. 실행은 간단합니다, 두가지 방법이 있습니다. ```ts-node-dev```를 통해서 TypeScript 코드를 바로 실행 시킬 수 있습니다. ```npm run dev```를 터미널에 입력하시게 되면 TypeScript 코드로 바로 실행할 수 있습니다.  

하지만 안전한 방법은 TypeScript를 안전하게 JavaScript로 빌드해서 실행시키는 방법이 있습니다. ```nodemon```를 이용해서 실행시킬 수 있습니다. ```npm run start```를 터미널에 입력하시면 TypeScript에서 JavaScript로 변환된 JavaScript 파일을 실행 시킬 수 있습니다.

오늘은 TypeScript와 Node.js Express를 사용하는 방법에 대해서 알아 보았습니다. 