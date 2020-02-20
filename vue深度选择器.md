# vue项目深度选择器.md

### 深度选择器解决vue项目里样式规则无法作用于UI组件的问题（为什么我的样式没有生效？）

#### 1. css >>> 用法
```
  <style scoped>
      .table-wrapper {
        overflow: visible;
        ::v-deep .el-table__body-wrapper {
          overflow: visible;
        }
  </style>
```
  
####  2. /deep/ 用法
```
<style scoped>
  .table-wrapper {
    overflow: visible;
    /deep/ .el-table__body-wrapper {
      overflow: visible;
    }
</style>
// 此法可能因为版本关系会报错，那么可以尝试第3种
    
```

#### 3. ::v-deep 用法
```
<style scoped>
  .table-wrapper {
    overflow: visible;
    ::v-deep .el-table__body-wrapper {
      overflow: visible;
    }
<style>
```
