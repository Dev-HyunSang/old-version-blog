---
id: 4
title: "ESLint는 무엇인가요?"
subtitle: "요즘 핫한 ESLint는 무엇일까?"
date: "2021.02.03"
tags: "JavaScript"
---
안녕하세요, JavaScript와 TypeScirp를 공부하면서 ESLint에 대해서 궁금하고 무엇인지 조금은 알고 있지만 더 알기 위해서 이런 글을 써서 공부해 볼려고 합니다.  

**ESLint**는 ES + Lint입니다. ES는 EcmaScript입니다, 즉 자바스크립트를 의미합니다. Lint는 보푸라기라는 뜻이면서 프로그래밍 쪽에서는 에러가 있는 코드에 표시를 달아놓는 것을 의미합니다.  
즉 ESLint는 자바스크립트 문법 중 에러가 있는 곳에 표시를 달아놓는 도구를 의미합니다.

ESLint는 사용자가 직접 정의한대로 코드를 점검하고, 에러가 있으면 표시해줍니다. 또 문법 에러뿐만 아니라 코딩 스타일도 정할 수 있습니다.  
회사에서 협업을 할 때 좋습니다. **팀원끼리 협업을 하는 것과 코딩 스타일은 무슨 상관이 있을까요?** 사람들은 자기만의 코딩 스타일이 있기 때문에 여러 사람이 같이 코딩을 하는 경우에는 차이가 많이 발생하게 됩니다. 이럴 때 팀에서 하나의 코딩 스타일을 적용하고 ESLint에 설정해두면 마치 한 사람이 코딩을 한 것 같은 결과를 얻을 수 있습니다.  

이렇게 코드와 코딩 스타일을 점검해 주는 툴은 ESLint 외에도 [JSHint](https://jshint.com/about/), [JSLint](https://jslint.com/help.html), [JSCS](https://jscs-dev.github.io/)등이 있지만 ESLint가 요즘 대세입니다! 예전에는 JSLint였는데 점점 시대가 변하면서 다른 것으로 변하는 것 같습니다. ESLint도 언젠가는 다른 도구로 대체될 수도 있습니다, 하지만 지금은 ESLint의 시대입니다. ESLint가 뜨는 이유는 바로 **확장성** 때문입니다. 다양한 **플러그인**을 사용할 수 있기 때문에 새로운 규칙을 추가할 수도 있고, 손쉽게 다른 회사나 사람의 설정을 도입할 수 있습니다.

```shell
npm install -g eslint eslint-config-airbnb-base eslint-plugin-import
```

개발용으로만 사용되고 eslint 명령어를 사용하해야 하기 때문에 npm에서는 ```-g``` 글로벌 옵션으로 설치하시면 됩니다.  
```eslint-config-airbnb-base```는 그냥 플러그인의 예시로 넣어보았습니다. airbnb-base를 사용하려면 필요합니다.  
**플러그인**은 몇 가지 규칙을 추가해줍니다. 설치 완료 후 ESLint의 설정을 담은 ```.eslintrc```를 만듭니다. ```.eslintrc```에서는 플로그인이나 규칙에 대한 정의를 JSON 형식으로 넣어줍니다.

# 참고하면 좋은 글이나 출처
[**ZeroCho Blog-ESLint**](https://www.zerocho.com/category/JavaScript/post/583231719a87ec001834a0f2)를 참고하여서 작성 하였습니다.