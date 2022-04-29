# ElementUI框架

## 暴改样式

### 表格

```scss
.el-table {
  color: #5F677B;
}

// 表格行高 44px
.el-table th {
  font-weight: 400;
  color: #2B3245;
  font-family: '微软雅黑';
  font-weight: 400;
}


.el-table th,
.el-table td {
  padding: 0;
  padding-left: 24px;
  height: 44px;
  font-size: 14px;
}

.el-table .el-table__cell {
  padding: 0;
  padding-left: 14px;
  height: 44px;
  font-size: 14px;
  text-align: left;
}

.el-table .el-button--small {
  font-size: 14px;
}

// .el-table .el-table--enable-row-hover .el-table__body tr:hover>td {
//   background-color: black !important;
// }
.el-table__body tr:hover>td,
.el-table__body tr:hover>.el-table__row {
  background-color: $customThemeOpacityColor5 !important;
  color: #2B3245;
}

.el-table--enable-row-hover .el-table__body tr:hover>td {
  background-color: $customThemeOpacityColor5 !important;
  color: #2B3245;
}
```

## 下拉选择

```scss
// 单选选中
.el-radio-button__inner:hover,
.el-radio__input.is-checked+.el-radio__label,
// select下拉选中
.el-select-dropdown__item.selected,
// cascader框
.el-cascader-node.in-active-path,
.el-cascader-node.is-active,
.el-cascader-node.is-selectable.in-checked-path {
  color: $customThemeColor !important;
}
.el-cascader-node.is-selectable:hover{
  background-color: $customThemeOpacityColor5 !important;
}
.el-select-dropdown .el-select-dropdown__item:hover{
  background-color: $customThemeOpacityColor5 !important;
}
```

## 时间选择器

```scss
.el-date-table{
   td.available:hover{
    color:$customThemeColor;
  }
   td.today span{
    color: $customThemeColor;
  }
   td.current:not(.disabled) span{
    background-color: $customThemeColor;
  }
}
```

