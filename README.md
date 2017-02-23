# vue-todos

> A Vue.js project

## 버그인지 안된건지 모르겠지만 기록...

### transition-group 사용해서 v-for에 효과를 줄 때

아래와 같이 v-for의 index를 key로 지정하면 아래쪽만 transition되는 버그가 있는 듯 하다 (index값은 정상적으로 나온다!)
``` javascript
<li 
  v-for="(todo, index) in todos"
  :key="index"></li>
```
~~해결법) v-for를 돌릴 data 배열에 key 컬럼을 지정해주면 된다.~~<br>
해결법) key에 리스트의 item을 넣어준다 (공식 문서에 친절하게 나와있드라 ㅠㅠ..)


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
