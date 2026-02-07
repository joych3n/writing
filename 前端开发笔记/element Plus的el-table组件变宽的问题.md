# element Plus的 el-table 组件不断变宽或抖动的问题

## 问题描述

在使用 Element Plus 的 `el-table` 时，发现页面加载或数据变化时表格会出现"不断变宽"或抖动的现象，经过排查，这是因为table同时设置了 `width: 100%`（或通过属性让表格占满父容器）并且设置了表格边框。

## 原因详解

如果你设置了 width: 100%，而父容器正好也是 100% 宽度，那么加上边框后，Table 的实际宽度就变成了 100% + 2px。这多出来的 2px 会撑大父容器，父容器变大后，Table 的 100% 又随之增长，形成一个正向反馈死循环，导致表格宽度不断“生长”。

## 解决方法

### 方案1：使用 border-box 盒模型（推荐）

让表格的宽度包含边框与内边距：

```css
.el-table,
.el-table * {
  box-sizing: border-box;
}
```

### 方案2：把边框设置在父容器上，表格不设置边框：

```html
<div style="border: 1px solid #ccc;">
  <el-table :data="tableData" style="width: 100%;">
    <!-- 表格列定义 -->
  </el-table>
</div>
```

**优先级方案**：

1. **首选** `box-sizing: border-box`（最根本的解决方式）
2. **次选** 将边框放在父容器上（适用于不想修改全局样式的情况）
