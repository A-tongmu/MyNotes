# 学习笔记

## JavaScript

### 地狱回调处理

- 1 回调参数互相依赖

```javascript
const asyncQuery = (query, params) => {
  return new Promise((resolve, reject) => {
    query(params)
      .then((res) => {
        resolve(res)
      })
      .catch((error) => {
        reject(error)
      })
  })
}
asyncQuery(QueryTestService, {})
  .then((res) => {
    let userSeq = res.UserSeq
    console.log(userSeq)
    return asyncQuery(QueryOthersService, { UserSeq: userSeq })
  })
  .then((res) => {
    console.log(res)
  })
```

- 2 回调参数不依赖

```javascript
let a = new Promise((resolve, reject) => {
  QueryTestService({}).then((res) => {
    resolve(res)
  })
})
let b = new Promise((resolve, reject) => {
  QueryOthersService({}).then((res) => {
    resolve(res)
  })
})
Promise.all([a, b]).then((result) => {
  console.log(result)
})
```

- 3 async + await 处理异步请求

```javascript
getData = async () => {
  try {
    const data = await http(xxx)
    this.setState({
      data,
    })
  } catch (error) {
    this.setState({
      error,
    })
  }
}
```

### PDF.js 的使用

- 1 下载地址
  [PDF.js(有 ES5 版本)](http://mozilla.github.io/pdf.js/getting_started/#download)
- 2 基本使用
  -- 在框架中使用
  -- 把 build、web 放在 public 文件下
  -- 删除.pdf、.map 文件，删掉 viewer.js 中 defaultUrl 中的引用，减少文件大小
  -- 核心代码

```javascript
// encodeURIComponent 应对可能的汉字传参
// 结尾加上.pdf
window.open(
  '/pdf/web/viewer.html?file=' + encodeURIComponent(`https://***?id=123&.pdf`)
)
```

### 待续...
