# 追加情報 [gogo5nta 参考情報](https://github.com/gogo5nta/burger_war/blob/master/info.md)  
 ・ROS関係の情報等を追加記載  
　 一括でROSをインストールしたい場合は、https://github.com/gogo5nta/burger_war/tree/master/other  
　 にある「install_ros_kinetic.sh」、「add_robocon2019.sh」をローカルにコピーし、以下を実行
```
// ROS(kinetic)を一括インストール
$ chdmod 777 install_ros_kinetic.sh
$ ./install_ros_kinetic.sh

// Robocon2019に必要な物を一括インストール
$ chdmod 777 add_robocon2019.sh
$ ./add_robocon2019.sh
```
・お勧めalias：~/.bashrcファイルをエディタ等で開き、設定を追加　　
  参考：[ROSのインストールとセットアップ](http://bril-tech.blogspot.com/2016/10/ros2-ros.html)  
  ```
# ROS全般コマンド  
alias cw='cd ~/catkin_ws'
alias cs='cd ~/catkin_ws/src'
alias cm='cd ~/catkin_ws && catkin_make'
# robocon2019用コマンド  
alias bw='cd ~/catkin_ws/src/burger_war'
alias sim='cd ~/catkin_ws/src/burger_war && bash scripts/sim_with_judge.sh'
alias start='cd ~/catkin_ws/src/burger_war && bash scripts/start.sh'
# robot位置がリセット。↑のstartで再スタート  
alias reset='rosservice call /gazebo/reset_simulation "{}"'  
  ```
・vscode上でROSをデバッグ(python)  
  このページを参考に。[ROSでデバッグ (Visual studio code & ROS & python)](https://github.com/gogo5nta/burger_war/blob/master/debug.md)
  
# burger_war
ロボットで戦車対戦をするゲームです。
大砲で撃つ代わりに、カメラでターゲットのARマーカーを読み取ります。<BR>

## 目次
- インストール
- 審判サーバー
- ファイル構成
- その他
- 動作環境

## インストール
burger_warには**実機**と**シミュレータ**があります。

### 1. ros (kinetic) のインストール
rosのインストールが終わっている人は`2.このリポジトリをクローン` まで飛ばしてください。

参考  ROS公式サイト<http://wiki.ros.org/ja/kinetic/Installation/Ubuntu>
上記サイトと同じ手順です。
ros インストール
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://ha.pool.sks-keyservers.net:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt-get update
sudo apt-get install ros-kinetic-desktop-full
```
環境設定
```
sudo rosdep init
rosdep update
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
ワークスペース作成

参考<http://wiki.ros.org/ja/catkin/Tutorials/create_a_workspace>
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
cd ~/catkin_ws/
catkin_make
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 2. このリポジトリをクローン
gitをインストールします。
```
sudo apt-get install git
```

turtlr_war リポジトリをクローンします。
先程作ったワークスペースの`src/`の下においてください。
```
cd ~/catkin_ws/src
git clone https://github.com/OneNightROBOCON/burger_war
```

このリポジトリのフィールド用のGAZEBOモデルにPATHを通す
```
export GAZEBO_MODEL_PATH=$HOME/catkin_ws/src/burger_war/burger_war/models/
```
シェルごとに毎回実行するのは面倒なので上記は`~/.bashrc`に書いておくと便利です｡

### 3. 依存ライブラリのインストール
- pip : pythonのパッケージ管理ツール
- requests : HTTP lib
- flask : HTTP server 審判サーバーで使用
- turtlebot3
- aruco

```
# pip のインストール 
sudo apt-get install python-pip
#　requests flask のインストール
sudo pip install requests flask
# turtlebot3 ロボットモデルのインストール
sudo apt-get install ros-kinetic-turtlebot3 ros-kinetic-turtlebot3-msgs ros-kinetic-turtlebot3-simulations
# aruco (ARマーカー読み取りライブラリ）
sudo apt-get install ros-kinetic-aruco-ros
```


### 5. make
```
cd ~/catkin_ws
catkin_make
```

インストールは以上です。

## サンプルの実行
### シミュレータ
シミュレータ､ロボット(turtle_bot),審判サーバー､観戦画面のすべてを一発で起動する。大会で使用するスクリプト。<BR>
最初にburger_warのフォルダまで移動します。
```
cd ~/catkin_ws/src/burger_war
```
初回のみ、以下のコマンドでGazeboを起動し、モデルデータ等を読み込んでおくとよいです。<BR>
(初回はGazeboの起動がおそいので)
```
gazebo
```
次にシミュレーションを起動
```
bash scripts/sim_with_judge.sh
```
フィールドとロボットが立ち上がったら
別のターミナルで下記ロボット動作スクリプトを実行
```
bash scripts/start.sh
```

![screenshot](https://user-images.githubusercontent.com/17049327/61606479-7ed49680-ac85-11e9-8c77-5cad3a5db4ed.png)

↑このようなフィールドが現れロボットが2台出現します。
審判画面も表示されます。


審判サーバーを立ち上げずにシミュレータとロボットのみ立ち上げる場合
```
roslaunch burger_war　setup_sim.launch
```
フィールドとロボットが立ち上がったら
別のターミナルで下記ロボット動作スクリプトを実行
```
bash scripts/start.sh
```
審判サーバー以外は上記と同じです。


### 実機
センサなどが立ち上がりロボットを動かす準備
```
roslaunch burger_war setup.launch
```
別のターミナルで
```
roslaunch burger_war action.launch
```


## 審判サーバー
審判サーバーは`burger_war_judge/`以下にあります
そちらのREADMEを参照ください

## ファイル構成
各ディレクトリの役割と、特に参加者に重要なファイルについての説明

下記のようなディレクトリ構成になっています。  

```
burger_war
├── burger_war
│   ├── CMakeLists.txt
│   ├── launch  launchファイルの置き場
│   │   ├── sim_robot_run.launch  シミュレータ上で２台のロボットを動かすlaunchファイル
│   │   └─ setup_sim.launch  Gazeboシミュレータ上でフィールドの生成ロボットを起動、初期化するlaunchファイル
│   │
│   ├── models   GAZEBOシミュレーター用のモデルファイル
│   ├── package.xml
│   ├── scripts    pythonで書かれたROSノード
│   └── world     GAZEBO用の環境ファイル
│       ├── gen.sh          burger_field.world.emから burger_field.worldを作成するスクリプト
│       ├── burger_field.world  最新のworldファイル
│       └── burger_field.world.em  worldファイルのマクロ表記版､こっちを編集する
|
├── judge   審判サーバー
│   ├── judgeServer.py  審判サーバー本体
│   ├── log   ログがここにたまる
│   ├── marker_set  マーカーの配置設定ファイル置き場
│   ├── picture  観戦画面用画像素材
│   ├── README.md  
│   ├── test_scripts   初期化などのスクリプト
│   └── visualizeWindow.py  観戦画面表示プログラム
|
├── README.md   これ
├── rulebook.md  ルールブック
└── scripts      一発起動スクリプト
    ├─── sim_with_judge.sh   シミュレーターとロボットと審判サーバーの立ち上げ初期化をすべて行う
    └──  start.sh             赤サイド、青サイドのロボットを動作させるノードを立ち上げるスクリプト
```
↑ディレクトリと特に重要なファイルのみ説明しています。

## 推奨動作環境
- Ubuntu 16.04 
- Ros kinetic
2018年からkineticで開発しています｡

## Turtlebot3のスペック
- http://emanual.robotis.com/docs/en/platform/turtlebot3/specifications/

## その他
https://github.com/gogo5nta さんに一括でインストールするスクリプトを作成いただいたので本リポジトリにも置いています。
ご活用ください。
```
// ROS(kinetic)を一括インストール
$ chdmod 777 ./scripts/install_ros_kinetic.sh
$ ./scripts/install_ros_kinetic.sh

// Robocon2019に必要な物を一括インストール
$ chdmod 777 ./scripts/add_robocon2019.sh
$ ./scripts/add_robocon2019.sh
```
