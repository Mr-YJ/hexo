---
title: async/await
date: 2019-04-22 19:32:00
tags:
---

## async
async fun()  
函数前的async使得这个函数返回一个promise,即便这个函数中没有返回一个promise,类似于将return变成resolve
  
## await
await等待一个promise执行完才会继续执行后面的操作,但是要注意的是await必须出现在async里面，也就是不能在非async的函数里面使用await   

在循环语句中使用await时，forEach是不能生效的,可以使用其他循环语句
``` bash
await (async () => {
  for (let i = 0; i < 10; i++) {
      await funcA();
    }
  }
})();
```

