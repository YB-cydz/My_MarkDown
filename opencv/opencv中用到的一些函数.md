## 堆叠

> 返回堆叠后的新图片
>
> 参数:缩放比例,要堆叠的一组图片(元组)

```python
def stackImages(scale, imgArray):

    row = len(imgArray)# 获取行数
    col = len(imgArray[0])# 获取列数

    x_offset_scale = int(imgArray[0][0].shape[1] * scale)  # 每张图片的宽度乘以缩放比例
    y_offset_scale = int(imgArray[0][0].shape[0] * scale)  # 每张图片的高度乘以缩放比例


    # 计算堆叠后的图片的高度和宽度
    width = col * x_offset_scale
    height = row * y_offset_scale

    # 创建一个空白图像用于堆叠
    stackedImage = np.zeros((height, width, 3), dtype=np.uint8)

    y_offset = 0
    for x in range(row):
        x_offset = 0
        for y in range(col):
            img = imgArray[x][y]
            if img is None:
                continue
            # 调整图片大小
            img = cv2.resize(img, (x_offset_scale, y_offset_scale))
            if len(img.shape) == 2:  # 如果是灰度图像
                img = cv2.cvtColor(img, cv2.COLOR_GRAY2BGR)

            # 将图片放入堆叠图像中
            stackedImage[y_offset:y_offset + y_offset_scale, x_offset:x_offset + x_offset_scale] = img
            x_offset += x_offset_scale
        y_offset += y_offset_scale

    return stackedImage
```

> 调用样例

``` python
imgStack = stackImages(0.8, ([img, black], [img, img]))
```

## 获取鼠标在图片中的坐标

```python
def mousePoints(event, x, y, flags, params):
    if event == cv2.EVENT_LBUTTONDOWN:  # 鼠标左键按下事件
        print(f'鼠标位置: ({x}, {y})')  # 输出鼠标位置
```

> 调用样例

```python
cv2.setMouseCallback('Image', mousePoints)  # 设置鼠标回调函数
```









