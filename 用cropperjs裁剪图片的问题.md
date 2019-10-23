### 用cropperjs裁剪图片的问题

报错： First argument to DataView constructor must be an ArrayBuffer

或者

First argument to DataView constructor must be an element <img> or <canvas>

原因：mock.js导致图片转换受到影响，需关闭mock(坑了我好久)
