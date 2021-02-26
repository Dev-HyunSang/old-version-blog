---
id: 7
title: "Vue.js 인스턴스 라이프 사이클"
subtitle: "Vue.js 인스턴스 라이프 사이클이 무엇일까?"
date: "2021.02.25"
tags: "Vue.js"
---
안녕하세요! 요즘 Vue.js를 공부하고 있습니다. 옛날에 프론트엔드를 공부를 하였지만 백엔드를 위주로 공부하고 있었지만 회사에서 Vue.js를 쓰는 프로젝트가 있어서 다시 Vue.js를 공부하고 있습니다.

# 인스턴스 라이프 사이클은 무엇일까?
인스턴스 라이프 사이클이란 뷰의 인스턴스가 생성되어 소멸되기까지 거치는 과정을 의미합니다. 인스턴스가 생성되고 나면 라이브러리 내부적으로 각각의 과정들이 진행됩니다.

# 라이프 사이클 다이어그램
![Diagram](https://joshua1988.github.io/vue-camp/assets/img/lifecycle.dcbe29f6.png)

## beforeCreate
인스턴스가 생성되고 나서 가장 처음으로 실행되는 라이프 사이클 단계입니다.  
data 속성과 methods 속성이 아직 인스턴스에 정의되어 있지 않고, 돔과 같은 화면 요소에도 접근할 수 없습니다.

## created
beforeCreate 라이프 사이클 단계 다음에 실행되는 단계입니다. data 속성과 methods 속성이 정의되었기 때문에 this.dat 또는 this.fetchData()와 같은 로직들을 이용하여 data 속성과 methods 속성에 정의된 값에 접근하여 로직을 실행할 수 있습니다. 아직 인스턴스가 화면 요소에 부착되기 전이기 때문에 template 속성에 정의된 돔 요소로 접근할 수 없습니다.

**data속성과 methods속성에 접근할 수 있는 가장 첫 라이프 사이클 단계이자 컴포넌트가 생성되고 나서 실행되는 단계이기 때문에 서버에 데이터를 요청하여 받아오는 로직을 수행하기 좋습니다.**

## beforeMount
created 단계 이후 template 속성에 지정한 마크업 속성을 render() 함수로 변환한 후 el 속성에 지정한 화면 요소(dom)에 인스턴스를 부착하기 전에 호출되는 단계입니다. render() 함수가 호출되기 직전의 로직을 추가하기 좋습니다.

## mounted
el 속성에서 지정한 화면요소에 인스턴스가 부착되고 나면 호출되는 단계로, template 속성에 정의한 화면 요소(dom)에 접근할 수 있어 화면 요소를 제어하는 로직을 수항해기 좋은 단계입니다.  
돔에 인스턴스가 부착되자마자 바로 호출되기 때문에 하위 컴포넌트나 외부 라이브러리에 의해 추가된 화면 요소들이 최종 HTML 코드로 변환되는 시점과 다를 수 있습니다.

## beforeUpdate
el 속성에서 지정한 화면 요소에 인스턴스가 부착되고 나면 인스턴스에 정의한 속성들이 화면에 치환됩니다. 치환된 값은 뷰의 반응성(Reactivity)을 제공하기 위해 ```$watch``` 속성으로 감시합니다.

beforeUpdate는 관찰하고 있는 데이터가 변경되면 가상 돔으로 화면을 다시 그리기 전에 호출되는 단계이며, 변경 예정인 새 데이터에 접근할 수 있어 변경 예정 데이터의 값과 관련된 로직을 미리 넣을 수 있습니다.  만약 여기에 값을 변경하는 로직을 넣더라도 화면이 다시 그려지지는 않습니다.

## updated
데이터가 변경되고 나서 가상 돔으로 다시 화면을 그리고 나면 실행되는 단계입니다.  
데이터 변경으로 인한 화면 요소 변경까지 완료된 시점이므로, 데이터 변경 후 화면 요소 제어와 관련된 로직을 추가하기 좋은 단계입니다. 이 단계에서 데이터 값을 변경하면 무한 루프에 빠질 수 있기 때문에 값을 변경하려면 computed, watch와 같은 속성을 사용해야 합니다.  
따라서 데이터 값을 갱신하는 로직은 가급적이면 beforeUpdate에 추가하고, udpated에서는 변경 데이터의 화면 요소(dom)와 관련된 로직을 추가하는 것이 좋습니다.

## beforeDestroy
뷰 인스턴스가 파괴되기 직전에 호출되는 단계입니다. 이 단계에서는 아직 인스턴스에 접근할 수 있습니다. 따라서 뷰 인스턴스의 데이터를 삭제하기 좋은 단계입니다.

## destroyed
뷰 인스턴스가 파괴되고 나서 호출되는 단계입니다. 뷰 인스턴스에 정의한 모든 속성이 제거되고 하위에 선언한 인스턴스들 또한 모두 파괴됩니다.

## 참고하거나 읽으면 좋은 문서
[Do it! Vue.js 입문](http://www.yes24.com/Product/Goods/58206961)를 읽고 기록하였습니다.
- [Cracking Vue.js 인스턴스 라이프 사이클](https://joshua1988.github.io/vue-camp/vue/life-cycle.html#%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8)
- [created](https://vuejs.org/v2/api/#created)
- [beforeMount](https://vuejs.org/v2/api/#beforeMount)
- [mounted](https://vuejs.org/v2/api/#mounted)
- [destroyed](https://vuejs.org/v2/api/#destroyed)