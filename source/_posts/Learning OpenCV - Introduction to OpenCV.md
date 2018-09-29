---
title: Learning OpenCV I
tags:
- OpenCV
categories:
- Computer Vision
- OpenCV
mathjax: true
date: 2018-09-27 21:46:22
---

進入書囉~

待補(也太多待補文了吧orz)

<!--more-->


# Introduction to OpenCV

## 讀入影片
既然會了讀入圖片那影片呢?

其實也很easy 只是路徑又設錯了... 原本設成這樣`videos/drop.avi`
```cpp
#include<opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main() {
	namedWindow("Ex3",WINDOW_AUTOSIZE);
	//VideoCapture cap;
	//cap.open("../videos/drop.avi");
	VideoCapture cap("../videos/drop.avi");
	if (!cap.isOpened()) {
		cout << -1 << endl;
	}
	Mat frame;
	while (1) {
		cap >> frame;
		if (frame.empty())break;
		imshow("Ex3", frame);
		waitKey(33);
	}
}
```

## 影片增加拉條trackbar
```cpp
#include<opencv2/opencv.hpp>
using namespace std;
using namespace cv;

int g_slider_position = 0;
int g_run = 1, g_dontset = 0; //start out in single step mode
VideoCapture g_cap;
void onTrackbarSlide(int pos,void *) {
	g_cap.set(CAP_PROP_POS_FRAMES, pos);
	if (!g_dontset) {// g_dontset初始0 g_run=1 他一開始會停在第一張圖
		g_run = 1;
	}
}
int main() {
	namedWindow("Example2_4",WINDOW_AUTOSIZE);
	g_cap.open("../videos/drop.avi");
	int frames = (int)g_cap.get(CAP_PROP_FRAME_COUNT);
	int tmpw = (int)g_cap.get(CAP_PROP_FRAME_WIDTH);
	int tmph = (int)g_cap.get(CAP_PROP_FRAME_HEIGHT);
	cout << "Video has " << frames << " frames of dimensions("
		<< tmpw << ", " << tmph << ")." << endl;

	createTrackbar("Position", "Example2_4", &g_slider_position, frames,
		onTrackbarSlide);

	Mat frame;
	while (1) {
		if (g_run != 0) {
			g_cap >> frame;if (frame.empty())break;
			// CAP_PROP_POS_FRAMES 就是找pos
			int cur_pos = (int)g_cap.get(CAP_PROP_POS_FRAMES);
			g_dontset = 1;
			setTrackbarPos("Position", "Example2_4", cur_pos);
			imshow("Example2_4", frame);
			g_run--;
			// 當g_run=1的時候 他會變成0 然後在run mode變成-1之前 就會無限0 讓圖片定格
			// g_run=-1的時候 會一直是保持<0 所以圖繪在run mode
		}
		char c = (char)waitKey(10);
		if (c == 's'){ // single step
			g_run = 1; cout << "Single step, run = " << g_run << endl;
		}
		if (c == 'r'){ // run mode
		
			g_run = -1; cout << "Run mode, run = " << g_run << endl;
		}
		if (c == 27)
			break;
	}
}
```

## Split&Merge通道+混合


### Split&Merge
基本上split就是把三通道分成單通道 merge相反


先來說一下OpenCV使用 BGR色彩空間
channel.at(0) 就是取藍色B 1綠色G 2紅色R
at函數是引用 所以修改的時候兩者會一起變動

坑點:
**addWeighted**不只要求**同SIZE**還要求**同channel數**

### Code
```cpp
#include<opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main() {
	Mat img,logo,img_bluechannel;
	vector<Mat>channel;
	img = imread("../images/girl.jpg");
	logo = imread("../images/logo.jpg",0);//0 = gray_scale
	if (img.empty()) {
		return -1;
	}
	if (logo.empty()) {
		return -1;
	}
	namedWindow("Origin", WINDOW_AUTOSIZE);
	namedWindow("logo", WINDOW_AUTOSIZE);
	namedWindow("New", WINDOW_AUTOSIZE);
	// 因為大小不一 所以(大概)比例縮放
	resize(img, img, Size(480, 810), 0, 0);
	resize(logo, logo, Size(100, 100), 0, 0);
	split(img, channel);
	img_bluechannel = channel.at(2);//Red
	imshow("logo", logo);
	imshow("Origin", img);
	//Rect(x,y,w,h) 這裡的ROI的4個corner(0,0)(logo.cols,0)(logo.cols,logo.rows)(0,logo.rows)
	//addWeighted(input1,alpha,input2,beta,gamma,output) 公式alpha*input1+beta*input2+gamma
	addWeighted(img_bluechannel(Rect(0, 0, logo.cols, logo.rows)), 0.5, logo, 0.9, 0, img_bluechannel(Rect(0, 0, logo.cols, logo.rows)));
	merge(channel, img);
	imshow("New", img);
	imwrite("../images/blend.jpg", img);
	waitKey(0);
}

```

![](https://i.imgur.com/LvRu5vm.jpg)
![](https://i.imgur.com/WNI3hhX.jpg)
![](https://i.imgur.com/MiefjN9.jpg)

# Image Analysis


## DFT



## Canny Edge Detector
```
void cv::Canny	(	InputArray 	image,
OutputArray 	edges,
double 	threshold1,
double 	threshold2,
int 	apertureSize = 3,
bool 	L2gradient = false 
)	
```
The smallest value between threshold1 and threshold2 is used for edge linking
The largest value is used to find initial segments of strong edges.
###步驟
1. Gaussian filter 降噪
Gaussian kernel 的大小會影響到降噪效果
Size越大對於noise敏感度越低 誤差也會變高
2. 找梯度
梯度的方向會rounded到4個方向(0,45,90,135)
3. NMS(Non-maximum suppression)
用處在於找"最粗"的edge 把除了最大值以外都set到0 意思就是sharp最明顯變化的地方
4. Double Thresholding
剩下的點就很接近Real Edge
但有些點因為noise和color variation 還會留下
low,high兩種值 大於high的就是**strong**edge 剩下的 大於low的就是**weak**edge 比low還小的就byebye
5. Edge tracking
接下來 **strong**不用說一定保留 但是**weak**可能就要取捨  如果它周圍有**strong**就留 否則就不留

### 缺點
1. 高斯降噪會順便把edge也處理掉 這樣weak edge可能就不小心再後面處理的時候砍掉 或者 出現獨立的edge 總之就是不夠好
2. 梯度幅度計算上 再\\(2\times 2\\)window處理 容易找出false edge或lose true edge
3. Threshold的決定上困難
4. 正確率低

```cpp
#include<opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main() {
	Mat img_rgb, img_gry, img_cny;
	img_rgb = imread("../images/girl.jpg");
	cvtColor(img_rgb, img_gry, COLOR_BGR2GRAY);
	namedWindow("Gray",WINDOW_AUTOSIZE);
	namedWindow("Canny", WINDOW_AUTOSIZE);
	
	imshow("Gray", img_gry);
	pyrDown(img_gry, img_gry);
	pyrDown(img_gry, img_gry);
	Canny(img_gry, img_cny, 10, 100, 3,true);
	imshow("Canny", img_cny);
	waitKey(0);

}
```