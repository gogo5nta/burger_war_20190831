## 1. python+ROS環境
### 1.1 python上のcws(カレントワークディレクトリ)
・vscode上でデバッグの場合、 cwsは  
```
~/catkin_ws/src/buger_war
```
　※vscodeで最初にフォルダを開く際、~/catkin_ws/src/buger_warを指定のと同じcwdに  
   
・一方、roslaunch(your_burger.launchや、setup.launch)で起動する場合、cwsは
```
~/.ros
```
　※python上で画像読込の場合、cwdからのpathを指定する事  
   
・VSCodeやroslaunch上など環境依存せず取得する場合、以下が参考  
  [[Python] スクリプト実行ディレクトリを絶対パスで取得](https://qiita.com/nagamee/items/b7d1b02074293fdfdfff)  
  例:pythonを実行したフォルダと同じフォルダにimg.jpgがある場合
```
img = cv2.imread(os.path.dirname(__file__) + "/img.jpg", 1)
```
 
##2. Python+OpenCV環境
### 2.1 画素の読み書き
[OpenCVによる画像処理〜画像のピクセルへのアクセスなど〜](https://sites.google.com/site/lifeslash7830/home/hua-xiang-chu-li/opencvniyoruhuaxiangchulihuaxiangnopikuseruhenoakusesunado)  
・冒頭  
```
import cv2
import numpy as np
```
・画像読込  
```
img = cv2.imread('img.jpg', 1)
```
・画像(img)の読込(60行(y)、40列(x)の画素)  
```
blue  = img.item(60,40,0)
green = img.item(60,40,1)
red   = img.item(60,40,2)
```
・画像(img)の書込(60行(y)、40列(x)の画素)  
```
img.itemset((60,40,0), blue )
img.itemset((60,40,1), green)
img.itemset((60,40,2), red  )
```
