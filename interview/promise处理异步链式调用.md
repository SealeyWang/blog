# promise 处理异步链式调用

通过在 then 里 return 另一个异步回调

```js
Login()
  .then((token) => {
    return getUserInfo(token);
  })
  .then((res) => {
    console.log("get user info success");
    console.log(res);
  });
```

[demo with codesandbox](https://codesandbox.io/s/promise-yi-bu-lian-shi-diao-yong-ehigz?file=/src/App.js)
