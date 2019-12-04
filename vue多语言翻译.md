# vue多语言翻译

## 起步

1. 引入vue-i18

```
npm install vue-i18n
```

## 文件结构

lang

- index.js
- en.js
- zh.js
- zh文件夹
- - 语言翻译的js文件
- en文件夹
- - 语言翻译的js文件

page

- index页面
- - index.js
- - ……

## 核心index.js

1. 引入vue和vue-i18，设定vue使用vue-i18

```
import Vue from 'vue'
import VueI18n from 'vue-i18n'
Vue.use(VueI18n)
```

1. 设置对象并输出

```
const i18n = new VueI18n({
  locale: 'zh',
  messages: {
      zh: {},
      en: {}
  }
})
export default i18n
```

1. 引入对应语言json

```
import enLocale from './en'
import zhLocale from './zh'
const i18n = new VueI18n({
  locale: 'zh',
  messages: {
      zh: {...zhLocale},
      en: {...enLocale}
  }
})
```

1. 优化初始语言

```
import Cookies from 'js-cookie'
// 获得浏览器语言
const navigatorLanguage = (navigator.language || navigator.userLanguage).substr(0, 2)
// 获得保存在cookie中的语言设置
const cookieLanguage = Cookies.get('lang')
// 定义默认语言为中文
const lang = cookieLanguage || navigatorLanguage || 'zh'
const i18n = new VueI18n({
  locale: lang,
  messages: {
      zh: {...zhLocale},
      en: {...enLocale}
  }
})
```

1. 优化网页的title

```
// 根据语言改变窗口的title
if (lang === 'en') {
  document.title = 'Now is English'
} else {
  document.title = '现在是中文'
}
```

## en.js和zh.js

1. 语言模块自动注册：自动获取en或zh下的js文件

```
const requireModule = require.context('./en', true, /\.js$/)
```

1. 对获取的文件名进行处理，以文件名作为key，最后输出

```
const modules = {}

requireModule.keys().forEach(fileName => {
  const moduleName = fileName.replace(/(\.\/|\.js)/g, '')
  modules[moduleName] = {
    ...requireModule(fileName).default
  }
})

let en = {...modules}
export default en
```

## 使用vue-i18

1. 在页面的index.js中引入

```
import i18n from '@/lang'
new Vue({
  el: '#app',
  i18n,
  components: { App },
  template: '<App/>'
})
```

1. 在页面中使用

```
$t('filename.key') // 变量
```