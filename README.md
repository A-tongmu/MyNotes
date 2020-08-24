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

## Git

### 版本回退

- 1 git reset
git reset中有三个命令（--hard、--soft与--mixed）；主要用于工作区、暂存区、本地仓库三个区域的文件提交撤回

```javascript
git log //查看commit的操作行为哈希ID
git reset --hard *** //会将工作区、暂存区、本地仓库的所有提交的文件全都撤销(操作需谨慎)
git reset --soft *** //用于将文件提交至本地仓库后撤回暂存区的操作
git reset --mixed *** //用于将本地仓库文件撤回至工作区

```
- 2 git revert
git revert 可以实现代码回退，版本向前的效果，回退代码建议使用该命令，可以保留commit记录
```javascript
git revert *** //反做某次commit
git revert ***...*** //撤销一串提交，不包括前但包括后，即( ]
git revert *** *** //撤销多个指定提交
```
* 出现Please enter the commit message for your changes. Lines starting'  输入 :wq 退出

- 3 git stash
git stash 暂存本地代码，开发中很好用
```javascript
git stash  //未add之前执行
git stash list //查看保存的记录列表
//***取出***//
git stash apply //默认使用第一个存储,即stash@{0}，不会删除存储记录
git stash pop //与 apply 差不多，只不过这个在取出之后会将此stash空间删除

```

## CSS3
- 1 radial-gradient
radial-gradient 实现电商卡券效果
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .cnt {
      width: 500px;
      margin: 100px auto;
      filter: drop-shadow(2px 2px 5px rgba(0, 0, 0, 0.15));
    }
    .top {
      height: 100px;
      background: radial-gradient(circle at bottom left, transparent 10px, pink 0)
        bottom left,
      radial-gradient(circle at bottom right, transparent 10px, pink 0) bottom
        right;
      background-size: 60% 100%;
      background-repeat: no-repeat;
      border-radius: 5px 5px 0 0;
    }
    b {
      position: absolute;
      left: 10px;
      width: 480px;
      border-top: 2px dotted #000;
    }
    .bottom {
      height: 100px;
      background: radial-gradient(circle at top left, transparent 10px, pink 0)
        top left,
      radial-gradient(circle at top right, transparent 10px, pink 0) top right;
      background-size: 60% 100%;
      background-repeat: no-repeat;
      border-radius: 0 0 5px 5px;
    }
  </style>
</head>
<body>
  <div class="cnt">
    <div class="top"></div>
    <b></b>
    <div class="bottom"></div>
  </div>
</body>
</html>
```
* 效果图：

### 待续...
