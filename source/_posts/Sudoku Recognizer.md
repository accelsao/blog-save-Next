---
title: Sudoku Recognizer
tags:
  - OpenCV
categories:
  - Image Processing
mathjax: true
date: 2018-09-24 22:32:42
---

一系列紀錄

我要從圖片中辨識數獨
用opencv-python來完成

待補

<!--more-->

# Setup
首先因為我要用python(Jupyter)做 所以要安裝環境
我用Anaconda 所以pip安裝即可
```
pip install numpy Matplotlib
pip install opencv-python
```

然後 第一件事如同程式的helloworld一樣
我們要輸出圖片
```py
import cv2
img = cv2.imread('images/girl.jpg')
cv2.imshow('image',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
注意path `images/girl.jpg` 我是在同樣的ipynb建立images資料夾 先前用`../images/girl.jpg` 一直出錯

或者matplot在notebook裡
```py
import cv2
from matplotlib import pyplot as plt
img = cv2.imread('images/sudoku.jpg')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB) 
plt.axis('off')
plt.imshow(img)
plt.show()
```

因為他cv2.imread似乎是BGR

這樣就輸出圖片囉~
![](https://i.imgur.com/mWtbpzn.jpg)

# Introduction
辨識數獨 如下圖
我們有幾個難點
1. line不完美
2. 黑色格子線顏色和周圍有的沒的東西類似 也和數字衝突
3. 格子太小難提取


# Segmenting the Sudoku
分割圖片
我們要找出Line還有整張puzzle 來簡化當前的圖片
先用[adaptativeThreshold](https://docs.opencv.org/2.4/modules/imgproc/doc/miscellaneous_transformations.html#adaptivethreshold)來extract edge

## adaptativeThreshold
這個會讓grayscale image 轉成 binary image 
所以我們先轉換吧
Change to Grayscale image
```py
img_gray=cv2.cvtColor(img,cv2.COLOR_RGB2GRAY)
cv2.imwrite("images/sudoku_gray.jpg",img_gray)
plt.axis('off')
plt.imshow(img_gray)
plt.show()
```


![](https://i.imgur.com/G3OU9uH.jpg)


* Threshold_Binary
$$
dst(x,y)=
\begin{cases}
    maxValue& \text{if } src(x,y)>T(x,y\\
    0       & \text{otherwise}
\end{cases}
$$
* THRESH_BINARY_INV
$$
dst(x,y)=
\begin{cases}
    0& \text{if } src(x,y)>T(x,y\\
    maxValue       & \text{otherwise}
\end{cases}
$$

where \\(T(x,y)\\) is a threshold calculated individually for each pixel
* ADAPTIVE_THRESH_MEAN_C 
mean of the \\(blocksize\times blocksize\\) neighborhood of \\((x,y)\\) minus C
* ADAPTIVE_THRESH_GAUSSIAN_C
weighted sum of  \\(blocksize\times blocksize\\) neighborhood of \\((x,y)\\)


這裡我們採用ADAPTIVE_THRESH_MEAN_C 參數都只是大概set看有沒有清楚而已

```py
thresh = cv2.adaptiveThreshold(img_gray,255,0,1,11,15)
plt.imshow(thresh, cmap=plt.gray())
plt.axis("off")
```

![](https://i.imgur.com/6rNeT9L.jpg)

[Enumerations](https://docs.opencv.org/3.4.0/d7/d1b/group__imgproc__misc.html)
```
cv::AdaptiveThresholdTypes { 
  cv::ADAPTIVE_THRESH_MEAN_C = 0, 
  cv::ADAPTIVE_THRESH_GAUSSIAN_C = 1 
}
cv::ThresholdTypes { 
  cv::THRESH_BINARY = 0, 
  cv::THRESH_BINARY_INV = 1, 
  cv::THRESH_TRUNC = 2, 
  cv::THRESH_TOZERO = 3, 
  cv::THRESH_TOZERO_INV = 4, 
  cv::THRESH_MASK = 7, 
  cv::THRESH_OTSU = 8, 
  cv::THRESH_TRIANGLE = 16 
}
```

## Find the contour
接下來我們要找出contour
簡略一下[Contours  vs Edges](https://stackoverflow.com/questions/17103735/difference-between-edge-detection-and-image-contours)
contour boundary
edge image gradient的極值

contour 處理的是binary image

```py
_,contour,_ = cv2.findContours( thresh,
                                        cv2.RETR_LIST,
                                        cv2.CHAIN_APPROX_SIMPLE)
```
提一下 [cv2.findContours](https://docs.opencv.org/3.0-beta/modules/imgproc/doc/structural_analysis_and_shape_descriptors.html#findcontours) 3.0.0版本改成3output 
但我只要contour所以其他用 `_` 取代
cv2.RETR_LIST, 取得所有輪廓，不建立階層(hierarchy)
cv2.CHAIN_APPROX_SIMPLE 挑點剩下end points 簡化

[cv2.approxPolyDP](https://docs.opencv.org/2.4/modules/imgproc/doc/structural_analysis_and_shape_descriptors.html#approxpolydp)
取approximate點
cv2.approxPolyDP(curve, epsilon, closed[, approxCurve]) → approxCurve

```py
# 先複製RGB的 等等要畫線
img_candidate=img.copy()

# 最大的矩形 也就會是Sudoku本體
max_size=0

for i in range(len(contour)):
	# 找逼近點
    approx=cv2.approxPolyDP(contour[i], 4, True)
    if(not (len (approx)==4)):
        continue;
	# 判斷convex 
    if(not cv2.isContourConvex(approx) ):
        continue;
	# 計算面積
    rect_size=cv2.contourArea(approx)
    if rect_size>max_size:
        max_size=rect_size
        biggest_rect=approx

approx=biggest_rect

# 畫線
for i in range(len(approx)):
        cv2.line(img_candidate,(approx[i][0][0], approx[i][0][1]),
             (approx[(i+1)%4][0][0], approx[(i+1)%4][0][1]),
             (255, 0, 0), 2)
        print((approx[i%4][0][0], approx[i%4][0][1]))

plt.imshow(img_candidate)
plt.axis('off')
plt.show()
```

![](https://i.imgur.com/xr3fLQ0.jpg)

現在我們有了被框起來的數獨
現在我們把畫面變換
簡單說就是把框框拉滿 這樣圖片就可以分成\\(9\times 9\\)了
方法呢?

## Perspective transformation
我們使用以下變換
$$
\begin{align}
X &= \frac{ax + by + c}{gx + hy +1}\\
Y &= \frac{dx + ey + f}{gx + hy +1}
\end{align}
$$
map\\((x,y)\\)to\\((X,Y)\\)

這裡有8個未知數!!!
有解嗎? 有解需要8個已知數
我們Sudoku 4個corner(x,y) 4*2=8  -> 可解

那我們就套函數吧
[getPerspectiveTransform](https://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#getperspectivetransform)
把4個點input 4個output丟進去可以得到warpPerspective的M矩陣

[warpPerspective](https://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#warpperspective)
得到M矩陣後透過以下公式 就可以得到轉換後的點
![](https://docs.opencv.org/2.4/_images/math/fa7e99ae84a2294284e680e2d4c4011121be6492.png)


圖片SIZE(16,16) 因為要轉成\\(9\times 9\\) 所以SIZE要\\(144\times 144\\)
再來 我們需要先拿到corner point
```py
import numpy as np
import math
IMAGE_WIDHT = 16
IMAGE_HEIGHT = 16
SUDOKU_SIZE= 9

def getCorner(rcCorners):
    f=[]
    for i in range(4):
        f.append(rcCorners[i,0,:])
	#求得4個點中心 再根據角度大小 可以求出順/逆時針 點的順序
    x_avr=sum(rcCorners[i,0,0] for i in range(len(rcCorners)) ) / len(rcCorners)
    y_avr=sum(rcCorners[i,0,1] for i in range(len(rcCorners)) ) / len(rcCorners)
    def algo(v):
        return (math.atan2(v[0] - x_avr, v[1] - y_avr)
                + 2 * math.pi) % 2*math.pi
        f.sort(key=algo)
    # 這裡的順序我目前只能透過輸出來決定
    return (f[3],f[0],f[1],f[2])
```

做Perspective transformation
```py
points1 = np.array([
                    np.array([0.0,0.0] ,np.float32) + np.array([144,0], np.float32),
                    np.array([0.0,0.0] ,np.float32),
                    np.array([0.0,0.0] ,np.float32) + np.array([0.0,144], np.float32),
                    np.array([0.0,0.0] ,np.float32) + np.array([144,144], np.float32),
                    ],np.float32)
# Corner Point 轉成np.array
outerPoints = getCorner(approx)
points2 = np.array(outerPoints,np.float32

# pers就是M矩陣
pers = cv2.getPerspectiveTransform(points2,  points1 )
warp = cv2.warpPerspective(img, pers, (SUDOKU_SIZE*IMAGE_HEIGHT, SUDOKU_SIZE*IMAGE_WIDHT));
warp_gray = cv2.cvtColor(warp, cv2.COLOR_BGR2GRAY)

#show image
_=plt.imshow(warp_gray, cmap=plt.gray())
_=plt.axis("off")
cv2.imwrite("images/sudoku_pers.jpg",warp_gray)

```

![](https://i.imgur.com/ZwXUTBF.jpg)

# Segmenting the numbers (待續)

## Extract數字
首先 我們要提取小框框裡面的數字
```py
def extract_number(x,y):
    # get number in small sqare
    im_number = warp_gray[x*IMAGE_HEIGHT:(x+1)*IMAGE_HEIGHT, y*IMAGE_WIDHT:(y+1)*IMAGE_WIDHT]
    # threshold
    im_number_thresh=cv2.adaptiveThreshold(im_number,255,1,1,15,9)
    w,h=im_number.shape
	# delete 外圍的部分 
    for i in range(w):
        for j in range(h):
            dist = math.sqrt( (IMAGE_WIDHT/2 - i)**2  + (IMAGE_HEIGHT/2 - j)**2);
            if dist > 6:
                im_number_thresh[i,j]=0
    active_pixel=cv2.countNonZero(im_number_thresh)
    return [im_number,im_number_thresh,active_pixel]
```

```py
# 遇到奇怪的index用法 自己來測試一下
warp_gray.shape=144,144

print(warp_gray[2:5]) # 3X144 取第一維2,3,4
print(warp_gray[2:5][:]) # 同上
print(warp_gray[2:5][1:2]) # 取[2:5]中的[1:2] 也就是2,3,4的第二個 -> 3 1XALL
print(warp_gray[2:5][:,1:2]) #  輸出[2,3,4]裡面的 [1:2]  3X1
print(warp_gray[:,2:5]) 144X3
print(warp_gray[:,2:5][1:2]) 1X3 
print(warp_gray[1,2:5]) 同上

print(warp_gray[x*IMAGE_HEIGHT:(x+1)*IMAGE_HEIGHT,y*IMAGE_WIDHT:(y+1)*IMAGE_WIDHT]) 等價
print(warp_gray[x*IMAGE_HEIGHT:(x+1)*IMAGE_HEIGHT][:,y*IMAGE_WIDHT:(y+1)*IMAGE_WIDHT])
```
## 找外框 
找出每個數字外圍的框框
```py
def find_biggest_bounding_box(im_number_thresh):
    _,contour,_ = cv2.findContours(im_number_thresh.copy(),
                                         cv2.RETR_CCOMP,
                                         cv2.CHAIN_APPROX_SIMPLE)
    biggest_bound_rect = []
    bound_rect_max_size = 0;
    for i in range(len(contour)):
        bound_rect= cv2.boundingRect(contour[i])
        size_bound_rect = bound_rect[2]*bound_rect[3]
        if  size_bound_rect  > bound_rect_max_size:
            bound_rect_max_size=size_bound_rect
            biggest_bound_rect = bound_rect
    x_b, y_b, w, h = biggest_bound_rect
    # 增加邊界 不然有的會超出邊界
    x_b= x_b-1
    y_b= y_b-1
    w = w+2
    h = h+2
    
    return [x_b, y_b, w, h]
```
## Main
```py
import math
import numpy as np

N_MIN_ACTVE_PIXELS=10
sudoku = np.zeros(shape=(9*9,IMAGE_WIDHT*IMAGE_HEIGHT))

def Recognize_number( x, y):
    
    # Recognize the number in the rectangle
      
    # extract the number (small squares)
    [im_number, im_number_thresh, n_active_pixels] = extract_number(x, y)

    if n_active_pixels> N_MIN_ACTVE_PIXELS:
        [x_b, y_b, w, h] = find_biggest_bounding_box(im_number_thresh)

        im_t = cv2.adaptiveThreshold(im_number,255,1,1,15,9);
        number = im_t[y_b:y_b+h, x_b:x_b+w]

        if number.shape[0]*number.shape[1]>0:
            number = cv2.resize(number, (IMAGE_WIDHT, IMAGE_HEIGHT), interpolation=cv2.INTER_LINEAR)
			# Threshold 調整圖片threshold
            _,number2 = cv2.threshold(number, 127, 255, 0)
            number = number2.reshape(1, IMAGE_WIDHT*IMAGE_HEIGHT)
            sudoku[x*9+y, :] = number;
            return 1

        else:
            sudoku[x*9+y, :] = np.zeros(shape=(1, IMAGE_WIDHT*IMAGE_HEIGHT));
            return 0
```

[cv.threshold](https://docs.opencv.org/3.4/d7/d1b/group__imgproc__misc.html#gae8a4a146d1ca78c626a53577199e9c57)

## 輸出圖片部分
具體可根據要輸出的圖片數量調整 這裡有25張圖片 所以用\\(5\times 5\\)格式
```py
index_subplot=0
n_numbers=0
indexes_numbers = []
for i in range(SUDOKU_SIZE):
    for j in range(SUDOKU_SIZE):
        if Recognize_number(i, j)==1:
            if (n_numbers%5)==0:
                index_subplot=index_subplot+1
            indexes_numbers.insert(n_numbers, i*9+j)
            n_numbers=n_numbers+1

#create subfigures
f,axarr= plt.subplots(index_subplot,5)

width = 0;
for i in range(len(indexes_numbers)):
    ind = indexes_numbers[i]
    if (i%5)==0 and i!=0:
        width=width+1
    axarr[i%5, width].imshow(cv2.resize(sudoku[ind, :].reshape(IMAGE_WIDHT,IMAGE_HEIGHT), (IMAGE_WIDHT*5,IMAGE_HEIGHT*5)), cmap=plt.gray())
    axarr[i%5, width].axis("off")
```

# Recognizing digits
到了最後也得到數字了 接下來就要來針對"數字"來轉成matrix的0~9 畢竟雖然是印刷體還是無法直接計算

到這邊為止...
我難產了 暫時需要補點知識了 Q_Q

# Reference 

[Sudoku recognizer](http://www.shogun-toolbox.org/static/notebook/current/Sudoku_recognizer.html#Sudoku-recognizer)