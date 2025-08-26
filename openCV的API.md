> 从文件中读取图片

```python
img = cv2.imread('Resource\img2.jpg')#从文件中读取图片
```

> 展示图片

```python
cv2.imshow('output',img)            #展示图片,参数:窗口名,img
```

> 从文件中读取视频

```python
cap = cv2.VideoCapture('Resource/fix_panner.py - Visual Studio Code 2025-08-01 18-21-18.mp4')#从文件中获取视频
```

> 读取视频中的帧

``` python
success, img = cap.read()#从cap中读取帧,success:bool 表示是否读取到帧,img为读取到的帧
```

> 退出窗口，参数:等待时间

```python
cv2.waitKey(0)                      #等待按下任意键后退出
#or
if cv2.waitKey(1) & 0xFF == ord('q'):#按q退出窗口
        break
```
> 从摄像头读取视频

``` python
cap = cv2.VideoCapture(0)#从摄像头获取视频,0表示默认摄像头,参数为摄像头编号
```
> 修改摄像头参数

``` python
cap.set(cv2.CAP_PROP_FPS, 60)#修改参数,参数列表:要修改的参数,值
```
> 转换颜色空间

``` python
#灰度,参数:图片,颜色空间转换方式
imgGray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY) 
```
> 高斯模糊

``` python
#模糊,参数:图片,模糊核大小(宽,高)(奇数),标准差
imgBlur = cv2.GaussianBlur(imgGray, (7,7), 0)
```
> 获取边缘

``` python
#边缘,参数:图片,低阈值,高阈值,低于低阈值的边缘被抑制,高于高阈值的边缘被保留,介于两者之间的边缘被保留或抑制取决于其连接性
imgCanny = cv2.Canny(imgBlur, 50, 150)
```
> 膨胀与腐蚀
>
>  **典型用途**
>
> 1. **降噪**：先腐蚀再膨胀（开运算）去除噪点。
> 2. **填充**：先膨胀再腐蚀（闭运算）填补小洞。
> 3. **边缘检测后处理**：
>    - 膨胀（`dilate`）：让边缘更粗，更容易观察。
>    - 腐蚀（`erode`）：让边缘更细。
> 4. **物体分离/粘连分割**：利用腐蚀分开相连的物体。

``` python
kernel = np.ones((5,5), np.uint8)#卷积核
#膨胀和腐蚀,用于去除噪声,参数:图片,卷积核,迭代次数,迭代次数越多,越明显
#膨胀会使边缘变粗,腐蚀会使边缘变细
imgDialation = cv2.dilate(imgCanny, kernel=kernel, iterations=1)#膨胀
imgEroded = cv2.erode(imgDialation, kernel=kernel, iterations=1)#腐蚀
```
> 修改图片大小

``` python
imgResized = cv2.resize(img, (480, 640))  # 修改图片大小,参数:图片,新大小(宽,高)(x,y)
```
> 裁剪图片

``` python
imgCropped = img[0:200, 200:]  # 裁剪图片(y1:y2, x1:x2)
```
> 修改图片颜色

``` python
img[200:300, 100:300] = 0,255,0 #图片的一部分变成绿色,参数:位置(y1:y2, x1:x2),颜色(B,G,R)
```
> 画线

``` python
#画线,参数:图片,起点,终点(x,y),颜色(B,G,R),线宽
cv2.line(img, (0,0), (img.shape[1], img.shape[0]), (0,0,255), 3)
```
> 画矩形

``` python
#画矩形,参数:图片,左上角,右下角(x,y),颜色(B,G,R),线宽(负数表示填充)
cv2.rectangle(img, (0,0), (250,350), (0,255,0), 2)
```
> 画圆

``` python
#画圆,参数:图片,圆心(x,y),半径,颜色(B,G,R),线宽(负数表示填充)
cv2.circle(img, (400,50), 30, (0,0,255), 3) 
```
> 添加文本

``` python
#写字,参数:图片,文字内容,起始位置(x,y),字体,字体大小,颜色(B,G,R),线宽
cv2.putText(img, 'Hello', (300,200), cv2.FONT_HERSHEY_SIMPLEX, 1, (255,255,255), 2) 
```
> 计算透视变换矩阵

``` python
# 获取透视变换矩阵，参数:原始点,目标点
transform_matrix = cv2.getPerspectiveTransform(points1, points2)
```
> 应用透视变换

``` python
# 应用透视变换，参数:图片,变换矩阵,输出大小(宽,高)(x,y)
img_output = cv2.warpPerspective(img, transform_matrix, (w, h)) 
```
> 堆叠

``` python
#横向堆叠
imgHor = np.hstack((img, img))  # 水平堆叠两张图片
#纵向堆叠
imgVer = np.vstack((img, img))  # 垂直堆叠两张图片
```
> 创建窗口

``` python
cv2.namedWindow('Trackbars')  # 创建一个窗口用于滑块
```
> 修改窗口大小

``` python
cv2.resizeWindow('Trackbars', 640, 240)  # 调整窗口
```
> 创建滑块

``` python
cv2.createTrackbar('Hue Min', 'Trackbars', 0, 179, empty)  # 创建滑块:色调最小值,参数:滑块名,窗口名,最小值,最大值,回调函数
```
> 获取滑块值

``` python
h_min = cv2.getTrackbarPos('Hue Min', 'Trackbars')  # 获取滑块:色调最小值,参数:滑块名,窗口名
```
> 创建色块蒙版

``` python
# 创建掩码,参数:HSV空间图片,下限,上限
# inRange函数会将图片中在下限和上限之间的像素设置为255,其他像素设置为0
# 这样可以提取出指定颜色范围内的像素
mask = cv2.inRange(imgHSV, lower, upper)
```
> 应用蒙版提取色块
>
> 当 `mask` 存在时，只有蒙版中白色的区域保留下来。(src1一般与src2相同)
>
> 当 `mask` 不存在时，就是 `src1 & src2` 的逐像素按位与

``` python
# 应用掩码,参数:src1,src2,mask(可选)
# bitwise_and函数会将图片中掩码为255的像素与图片进行按位与操作
# 这样可以提取出指定颜色范围内的像素
imgResult = cv2.bitwise_and(img, img, mask=mask)
```
> 获取轮廓，返回值：contours：一个 Python 列表，每个元素是一个轮廓（即点集）；hierarchy：轮廓的层级信息（不用但是得有）

``` python
contours, hierarchy = cv2.findContours(img, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)  # 查找轮廓,参数:图片,轮廓检索模式,轮廓近似方法
```
> 计算轮廓面积

``` python
area = cv2.contourArea(cnt)  # 计算轮廓面积
```
> 勾勒轮廓

``` python
cv2.drawContours(imgContour, cnt, -1, (255, 0, 0), 3)  # 绘制轮廓,参数:图片,轮廓,-1表示绘制所有轮廓,(B,G,R),厚度
```
> 计算轮廓周长

``` python
peri = cv2.arcLength(cnt, True)  # 计算轮廓周长,参数:轮廓,是否闭合
```
> 多边形逼近，获取轮廓上的点

``` python
approx = cv2.approxPolyDP(cnt, 0.02 * peri, True)#多边形逼近,参数:轮廓,精度(越小越精确),是否闭合
```
> 获取轮廓边界，返回值：边界左上角的点，宽度和高度

``` python
x, y, w, h = cv2.boundingRect(approx)  # 获取边界框,参数:多边形
```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```
> 

``` python

```







