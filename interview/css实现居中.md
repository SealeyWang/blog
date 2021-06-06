# 如何垂直居中

## table 自带垂直居中

```html
<style>
  .parent {
    border: 1px solid red;
    height: 600px;
  }

  .child {
    border: 1px solid green;
  }
</style>
<table class="parent">
  <tr>
    <td class="child">child居中</td>
  </tr>
</table>
```

## 100% 高度的 after before 加上 inline block

```html
<style>
  .parent {
    border: 3px solid red;
    height: 600px;
    text-align: center;
  }

  .child {
    border: 3px solid black;
    display: inline-block;
    width: 300px;
    vertical-align: middle;
  }

  .parent:before {
    content: "";
    outline: 3px solid red;
    display: inline-block;
    height: 100%;
    vertical-align: middle;
  }
  .parent:after {
    content: "";
    outline: 3px solid red;
    display: inline-block;
    height: 100%;
    vertical-align: middle;
  }
</style>

<div class="parent">
  <div class="child">child居中</div>
</div>
```

## div 装成 table

```html
<style>
  div.table {
    display: table; /* 关键代码 */
    border: 1px solid red;
    height: 600px;
  }

  div.tr {
    display: table-row; /* 关键代码 */
    border: 1px solid green;
  }

  div.td {
    display: table-cell; /* 关键代码 */
    border: 1px solid blue;
    vertical-align: middle;
  }
  .child {
    border: 10px solid black;
  }
</style>

<div class="table">
  <div class="td">
    <div class="child">一串文字一串文字一串文字一串文字一串文字一</div>
  </div>
</div>
```

## margin-top -50%

```html
<style>
  .parent {
    height: 600px;
    border: 1px solid red;
    position: relative;
  }
  .child {
    border: 1px solid green;
    width: 300px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -150px;
    height: 100px; /* 高度100*/
    margin-top: -50px; /* -50%*/
  }
</style>

<div class="parent">
  <div class="child">
    一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
  </div>
</div>
```

## translate -50%

`transform:translate(-50% , -50%)`

```html
<style>
  .parent {
    height: 600px;
    border: 1px solid red;
    position: relative;
  }
  .child {
    border: 1px solid green;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>

<div class="parent">
  <div class="child">
    一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
  </div>
</div>
```

## absolute margin auto

```html
<style>
  .parent {
    height: 600px;
    border: 1px solid red;
    position: relative;
  }
  .child {
    border: 1px solid green;
    position: absolute;
    width: 300px;
    height: 200px;
    margin: auto;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }
</style>
<div class="parent">
  <div class="child">
    一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
  </div>
</div>
```

## flex

```html
<style>
  .parent {
    height: 600px;
    border: 3px solid red;

    display: flex;
    justify-content: center; /*主轴居中*/
    align-items: center; /*交叉轴居中*/
  }
  .child {
    border: 3px solid green;
    width: 300px;
  }
</style>
<div class="parent">
  <div class="child">
    一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
  </div>
</div>
```
