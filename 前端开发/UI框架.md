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

### 下拉选择

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

### 时间选择器

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

### 复选框

```scss
.el-checkbox__input.is-checked .el-checkbox__inner,.el-checkbox__input.is-indeterminate .el-checkbox__inner {
  background-color: $customThemeColor !important;
  border: 1px solid $customThemeColor !important;
}
.el-checkbox__inner:hover {
  border-color: $customThemeColor;
}
.el-checkbox__input.is-checked + .el-checkbox__label {
  color: $customThemeColor;
}
.el-checkbox__input.is-focus .el-checkbox__inner {
  border-color: $customThemeColor!important;
}
```

## 渲染

1. elementUI中 <el-radio-group> 动态展示 

   ```scss
   <el-radio-group
         v-if="question.questionTypeId == 'radio' ||
         question.questionTypeId == 'judge'"
         :value="getAnswer(item.correct, question.questionTypeId)"
         >
           <div
         class="selectDiv"
         v-for="(selection, index) in item.questionContent"
         :key="index"
         >
           <el-radio :key="selection.prefix" :label="selection.prefix">
             {{ selection.prefix }}
               <span class="itemCont">{{ selection.content }}</span>
         </el-radio>
         </div>
    </el-radio-group>
   --------------------CSS----------------------------------------
   
      .selectDiv {
        line-height: 50px;
   
        .itemCont {
          /deep/ p {
            display: inline-block;
          }
        }
   
      }
   
      /deep/ .el-radio-group {
        width: 100%;
        overflow: hidden;
        min-height: 200px;
   
        .el-radio {
          span {
            vertical-align: middle;
            white-space: normal;
          }
        }
          
        .el-radio__label {
          width: 100%;
          word-break: normal;
        }
   
      }
   ```

2. el-tree 在无checkbox的情况下，**默认选中第一行数据**

   ```js
   // 默认展开的节点数组 
   :default-expanded-keys="currentKey"
   
   // 监听数组 默认click
   watch: {
     
       'currentKey': function (newVal, oldVal) {
         if (newVal) {
           this.$nextTick(() => {
             document.querySelector('.el-tree-node__content').click()
           })
         }
       }
   
     },
   ```

3. 解决 多级连菜单 childList数组为空，但页面  ` el-cascader控件 最后一级出现空白 暂无数据`

   ```js
   // 解决方法： 将空的childList 数组 设置为undefined 
   this.treeData = this.optimizeTreeDate(res.data);
   
   optimizeTreeData(data){
     for(let i=0,i<data.length;i++){
       if(data[i].childList =length <1){
         data[i].childList = undefined ;
       }else{
         this.optimizeTreeData(data[i].childList)
       }
       return data
     }
   }
   ```

   