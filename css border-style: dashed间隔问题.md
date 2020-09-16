
# css border-style: dashed间隔问题

#### 不如意的虚线/线条效果

```
border: 1px dashed #aaa;
```

#### 解决方法


```
  /* 通过重复渐变背景来模拟边框 */

    background-image: linear-gradient(to right, #ccc 0%, #ccc 50%, transparent 50%);
    /* 背景尺寸 */
    background-size: 10px 1px;
    /* 背景定位 */
    background-position-y: 100%;
    background-repeat: repeat-x;
```

#### 扩展

##### border-style: 可选值
```
border-style: dotted solid double dashed
```
