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

// 没有fixed的 hover样式
.el-table tbody tr:hover>td {
  background: $customThemeOpacityColor5 !important;
  color: #2B3245;
}
// 有fixed的 hover样式
.el-table__body .el-table__row.hover-row td{
  background-color: $customThemeOpacityColor5 !important;
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

### 分页

```scss
.el-pagination {
        .el-pagination__jump,
        .el-pagination__total {
          color: $font-color;
          font-size: $font-size;
          line-height: $el-pagination-li-height-length;
        }
        .btn-next,
        .btn-prev,
        .el-pager li {
          font-size: $font-size;
          line-height: $el-pagination-li-height-length;
          text-align: center;
          width: $el-pagination-li-width-length;
          height: $el-pagination-li-height-length;
          color: $font-minor-color;
          background-color: transparent;
          border: 1px solid $border-color-1;
        }
        &.is-background {
            .el-pager {
                li:not(.disabled) {
                    &.active{
                     background-color: $color-primary;
                     border: none;
                    }
                }
            }
        }
        .el-input.el-pagination__editor {
            margin: 0px 4px;
          .el-input__inner {
            height: $el-pagination-li-height-length;
            width: $el-pagination-input-width-length;
    
          }
        }
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
   
   optimizeTreeData(data) {
         for (let i = 0; i < data.length; i++) {
           if ((data[i].childList.length < 1)) {
             data[i].childList = undefined
           } else {
             this.optimizeTreeData(data[i].childList)
           }
         }
         return data
       },
   ```

## 验证规则

### 时间

```js
 checkStartTime(rule, value, callback) {
      if (value) {
        if (this.form.endTime && this.form.startFlag == 2) {
          let result = ((new Date(this.form.endTime.replace(/-/g, "\/"))) > (new Date(value.replace(/-/g, "\/"))))
          if (!result) {
            callback(new Error('开始时间请勿晚于结束时间'));
          } else {
            callback()
          }
        } else {
          callback()
        }
      }
    },
```

### 时间比大小

```js
// 通过 getTime( ) 都转为毫秒数比较 
function YMDHMS_daxiao(str1, str2) {
    str1 = new Date(str1).getTime();
    str2 = new Date(str2).getTime();
    // 小于0则str1时间小，大于0则str1时间大
    return str1 - str2
};

console.log(YMDHMS_daxiao('2021-9-1 8:00:00', '2021-12-3 15:00:00')); // -8060400000

```

### 密码/账户验证

```scss
// 用户名不能空、空格
export function checkAccount (rule, value, callback) {
  let reg = /\s+/
  if (!value || value.length === 0) {
    callback(new Error('账号不能为空'))
  } else if (reg.test(value)) {
    callback(new Error('账号不能有空格'))
  } else {
    callback()
  }
}

export function checkPassword (rule, value, callback) {
  let reg = /\s+/
  if (!value || value.length === 0) {
    callback(new Error('密码不能为空'))
  } else if (reg.test(value)) {
    callback(new Error('密码不能有空格'))
  } else {
    callback()
  }
}

export function checkPassword2 (rule, value, callback) {
  console.log(value, this)
  if (value !== this) {
    callback(new Error('两次输入密码不一致'))
  } else {
    callback()
  }
}

export function checkPhone (rule, value, callback) {
  // let reg = /^1(3[0-9]|5[0-35-9]|6[2567]|7[0-8]|8[0-9]|9[0-35-9])\d{8}$/
  let reg = /^1(3|4|5|6|7|8|9)\d{9}$/

  if (!reg.test(value)) {
    callback(new Error('手机号输入错误'))
  } else {
    callback()
  }
}

export function checkIpAddress (rule, value, callback) {
  console.log(value, this)
  var reg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/
  if (!reg.test(value)) {
    callback(new Error('IP地址不合法'))
  } else {
    callback()
  }
}

// 检查不能空、空格
export function checkNull (text) {
  let txt = text
  return function (rule, value, callback) {
    let reg = /\s+/
    if (!value || value.length === 0) {
      callback(new Error(txt + '不能为空'))
    } else if (reg.test(value)) {
      callback(new Error(txt + '不能有空格'))
    } else {
      callback()
    }
  }
}
```

## el-scrollbar

```js
<div style='height:768px'>
  <el-scrollbar style="height:100%">
    <ul>
    	<li style=“height:300px”></li>
			<li style=“height:300px”></li>
			<li style=“height:300px”></li>
	</ul>
</el-scrollbar>
</div>

			
```

注意：

1. 必须设置外层容器高度

2. 要设置el-scrollbar 的高度为 100% 将需要滚动的html代码包裹起来

3. 去掉水平滚动条使用 

   ```scss
   .el-scrollbar__wrap{
     overflow-x:hidden;
   }
   ```

   

## 注意点

1. 在element中 form表单 rules 验证规则中，一定要记得 callback , 不然 ` this.$refs.form.validate`不会证通过

2. el-form表单验证 规则未生效问题：

   出错点： 

   1. el-form  未绑定 双向绑定数据  `:model='form'` 
   2. el-form-item 的 prop 值和 输入框 的 v=model 不一致

   