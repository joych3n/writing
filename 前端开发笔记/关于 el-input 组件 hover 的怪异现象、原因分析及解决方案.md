# 关于 el-input 组件 hover 的怪异现象、原因分析及解决方案

## 问题描述

在使用 `el-input` 组件时，我发现该组件 `hover` 的一个怪异现象，该组件渲染后有一个`.el-input__wrapper` 元素，这个元素它会使用 `box-shadow` 属性实现边框效果，我给这个元素写了`box-shadow: none`的样式，在重点关注 `box-shadow` 属性时，这个怪异现象表现如下（用注册页和登录页来举例）：

- 当我在本地调试时，`[data-v-c5d9c666] .el-input__wrapper`比`.el-input__wrapper:hover` 的优先级高，无论是注册页还是登录页都是这样，所以在 `hover` 时边框不显示。
- 当发布到线上环境后，如果直接打开登录页或者刷新登录页，会惊讶的发现，此时登录页的`[data-v-c5d9c666] .el-input__wrapper`比`.el-input__wrapper:hover` 的优先级低，`hover` 时会有边框效果。
- 如果点击“立即注册”前往注册页，这时又会变成`[data-v-c5d9c666] .el-input__wrapper`比`.el-input__wrapper:hover` 的优先级高，`hover` 时没有边框效果。
- 总结一下怪异现象的规律：在本地调试时，无论先打开登录页还是注册页，这两个页面 `hover` 始终都没有边框效果，但是在线上环境时，无论是登录页还是注册页，直接打开或刷新的页面 `hover` 时有边框效果，后续跳转过去的页面 `hover` 时没有边框效果。

## 原因分析

- 我的代码 `:deep(.el-input__wrapper)` 编译后是 `[data-v-xxx] .el-input__wrapper`（属性选择器+类选择器）。
- Element Plus 的默认 `hover` 样式是 `.el-input__wrapper:hover`（类选择器+伪类选择器）。
- 这两种选择器的特指度（权重优先级）是相同的。当权重相同时，后加载/后定义的样式会生效。
- 本地调试：Vue 组件的样式通常会在运行时动态注入到 `<head>` 的末尾，我的样式是最后插入的。
- 线上刷新：由于加载时序问题和 CSS 被提取合并，Element Plus 的样式排在我的代码之后。

## 解决方案

显式地定义 hover 状态的样式，确保它的优先级足够高。 例如：

```css
:deep(.el-input__wrapper) {
  box-shadow: none;
  &:hover {
    box-shadow: 0 0 0 1px #c0c4cc inset;
  }
}
```
