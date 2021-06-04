# active class

```jsx
<NavLink to="/tags" activeClassName="selected">
  <Icon name="tag" />
  标签
</NavLink>
```

React onChange 和 HTML onChange 是不一样的

React onChange 会在你输入一个字的时候就触发

HTML onChange 在鼠标移走的时候触发 早于 onBlur
