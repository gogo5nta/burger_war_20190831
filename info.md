# 参考になるURL
## 1.ROS関係
### 1.1 ためになるサイト
・[demura.net: ROS全般で情報になるサイト](https://demura.net/ "demura.net")　　

### 1.2 ROSで便利なalias設定  
・[ROSのインストールとセットアップ](http://bril-tech.blogspot.com/2016/10/ros2-ros.html) 　
  　- ~/.bashrcファイルをエディタ等で開き、設定を追加　　
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
# burger_navigation使うときに必要
export TURTLEBOT3_MODEL=burger
# blue_botやred_botのteleop
alias teleop_blue_bot='cd ~/catkin_ws/src/burger_war/burger_war/launch && roslaunch teleop_blue_bot.launch'
alias teleop_red_bot='cd ~/catkin_ws/src/burger_war/burger_war/launch && roslaunch teleop_red_bot.launch'
```
  
buger_war/buger_war/launch/teleop_blue_bot.launch  
```
<launch>
  <!-- turtlebot3_teleop_key already has its own built in velocity smoother -->
  <group ns="blue_bot">
    <node pkg="turtlebot3_teleop" type="turtlebot3_teleop_key" name="turtlebot3_teleop_keyboard"  output="screen">
    </node>
  </group>
</launch>
```
  
buger_war/buger_war/launch/teleop_red_bot.launch  
```
<launch>
  <!-- turtlebot3_teleop_key already has its own built in velocity smoother -->
  <group ns="red_bot">
    <node pkg="turtlebot3_teleop" type="turtlebot3_teleop_key" name="turtlebot3_teleop_keyboard"  output="screen">
    </node>
  </group>
</launch>
```
  
### 1.3 burger_navigationの動作テスト
・パッケージをインストール(Ref: http://emanual.robotis.com/docs/en/platform/turtlebot3/pc_setup/#install-dependent-ros-packages)  
```
sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan  ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation ros-kinetic-interactive-markers
```
・Turtlebot3のモデル名の指定が必要  
```
echo "export TURTLEBOT3_MODEL=burger" >> ~/.bashrc
```
・Gazeboを起動  
```
cd ~/catkin_ws/src/burger_war
bash scripts/start.sh
```
・Navigationを起動  
```
roslaunch burger_navigation multi_robot_navigation_run.launch
```
・ゴール地点をコマンドラインから決める  
```
rostopic
 pub /red_bot/move_base_simple/goal geometry_msgs/PoseStamped '{header: 
{stamp: now, frame_id: "red_bot/map"}, pose: {position: {x: 0.0, y: 1.0,
 z: 0.0}, orientation: {w: 1.0}}}'
``` 

### 1.4 topic一覧の確認
・rostopic listでトピック一覧を取得（まずはこれから！)
```
$ rostopic list
```
### 1.5 cmd_velをpublish
・red_botを直進  
```
rostopic pub -1 /red_bod/cmd_vel geometry_msgs/Twist -- '[1.0, 0.0, 0.0]' '[0.0, 0.0, 0.0]'
```
・red_botを停止  
```
rostopic pub -1 /red_bod/cmd_vel geometry_msgs/Twist -- '[0.0, 0.0, 0.0]' '[0.0, 0.0, 0.0]'
```

### 1.6 topic一覧の取得、publishの参考になる資料
・[ROSトピックの理解](http://wiki.ros.org/ja/ROS/Tutorials/UnderstandingTopics) 

### 1.7 gazeboの更新(ros kinetic)
kineticに入ってるgazeboはバグがあって、リモート接続で落ちる場合有。例えば7.11などにアップデート
・[ROS環境で最新のGazeboを取得する](http://scnsh.hatenablog.com/entry/2018/03/04/183800)
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install gazebo7 -y
```

### 1.9 その他 ROS初心者用コマンド
・[ROSのTips（初心者向け）](https://qiita.com/yukkysaito/items/e2f714c254bf7799677e)  
・[安曇野の森から-ROSコマンド一覧（グループ別）](http://forestofazumino.web.fc2.com/ros/ros_command_list.html)  
・[(forkした非公式?)ROSBOOK_jp v1.0](https://github.com/bmagyar/rosbook_jp/tree/master/release)  

## 2.Ubuntu関係
### 2.1 Ubuntuのおすすめソフト
・[Ubuntu初心者がROSを使うときにおすすめのターミナル「terminator」](http://cryborg.hatenablog.com/entry/2016/09/03/164940)
  ![002](https://user-images.githubusercontent.com/30023363/61993971-47b01c00-b0af-11e9-9d9a-8f009a7b6060.jpg)  
```
  sudo apt-get install terminator
```
  
・[Ubuntuのベースバージョンを変えずにLinuxカーネルをアップグレードする方法](https://iberianpig.github.io/posts/2017-02-06_how_to_upgrade_kernel/)
  ![003](https://user-images.githubusercontent.com/30023363/61993980-8219b900-b0af-11e9-90fc-a0d49f8b412f.jpg)  
```
  //リポジトリを追加する
  sudo apt-add-repository -y ppa:teejee2008/ppa

  //ukuuのインストール
  sudo apt update && sudo apt install ukuu
```
### 2.2 CUIで文字化けの対策
・[コンソールでの文字化け対策](https://qiita.com/ihyomo/items/a66660f11cfa9f9668b4)  
```
$ nano .bashrc
```
 お好みのエディターでOK。nanoの場合、ファイルの保存は、Ctrl+O   
```
case $TERM in
    linux) LANG=C ;;
    *) LANG=ja_JP.UTF-8 ;;
esac
```

### 2.3 NVIDIAのdriverが原因のUbuntuログイン無限ループ
・[NVIDIAのdriverが原因のUbuntuログイン無限ループを避ける）](http://yusuke-ujitoko.hatenablog.com/entry/2019/03/16/174748)  
・[Ubuntu 16.04にNvidiaドライバをインストールしたらログインループになったときのメモ](https://qiita.com/konzo_/items/ca9303d5d515dbf5b66e)   
・[(↑二つがNGなら)Ubuntu 16.04 LTSにnVIDIAドライバーを入れなおす](https://rc30-popo.hatenablog.com/entry/2019/03/27/233257)  
・[blacklist nouveau関連](https://qiita.com/sasayabaku/items/2323a2c501e58c0621b6)  
  Ctrl+Alt+F2でコンソールに入り下記作業を行う  
```
# ドライバアンインストール
sudo apt-get --purge remove nvidia-*
sudo apt-get --purge remove cuda-*

# レポジトリ登録
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update

# ドライバ調査
sudo add-apt-repository ppa:xorg-edgers/ppa
sudo apt-get update
sudo apt-cache search 'nvidia-[0-9]+$'

# ドライバインストール
sudo apt-get install nvidia-XXX

# グラフィックドライバー等の状態を更新？
sudo update-initramfs -u
```

## 3. 仮想環境でROSを動かす
### 3.1 仮想環境(VirtualBox)でROS
・[ロボットプログラミングⅡ-2018：VirtualBoxでの演習まとめ - demura](https://demura.net/lecture/15435.html) <P>
  ![001](https://user-images.githubusercontent.com/30023363/61966794-89d55100-b00e-11e9-990e-cb122939d848.jpg)<P>
・[[備忘録] VirtualboxでGazebo使ったら落ちる時にチェックすること](http://zumashi.blogspot.com/2019/01/virtualboxgazebo.html)<BR>
  　- 3DアクセラレーションをOnにするとGazeboが途中で落ちる!<P>
・[virtualbox上のGazeboがセグメンテーション違反を引き起こす](https://codeday.me/jp/qa/20190621/1068199.html)<BR>
  　- .bashrcに下を記載し、ソフトウェアアクセラレーションを使用
  ```
  LIBGL_ALWAYS_SOFTWARE = 1
  ```

### 3.2仮想環境(VMWare)でROS ※OpenGL系が動く
・[ROS講座54 VMWare上でROSを使う - Qiita](https://qiita.com/srs/items/25efd45641c274bb8415) <P>
・[VMwareのROS環境構築](http://peach-pie.blog.jp/archives/12581678.html) <P>

## 5.Git
・[Windows: Git GUIを使う準備](http://sutara79.hatenablog.com/entry/2015/07/06/113431) <P>
・[Ubuntuにgit-guiをインストールする](https://pcl.solima.net/archives/743) <BR>
  ```
  $ sudo apt-get -y install git-gui
  ```
  git-guiの起動
  ```
  $ git gui
  ```
・[【GitHub】README.md に画像を表示させる簡単な方法](https://qiita.com/ROY_M/items/2c4feb5de05535441bc8) <BR>
  　- issue画像をアプロードし、生成されたリンクを貼り付けるだけ。<BR>
  　- issueはSettingsからOnにする事<P><HR>
・[1. 初期設定] ubuntu上のコマンドから最初に行うこと
  ```
  git config --global user.email "you@example.com" 
  git config --global user.name "Your name"
  ```
・[2. フォルダ、ファイルの追加(フォルダ内すべて)]
  ```
  git add .
  ```
・[3. ローカルリポジトリにコミット]
  ``` 
  git commit -m “first commit” 
  ```
・[4. push (ローカルのファイルを、リモートリポジトリにアップロード)]
  ```
  git push -u origin master 
  ```
・[5. fetch (origin から master ブランチに関する履歴と参照を取得。pullする前に実施するのを推奨（なくてもOK）)]
  ```
  git fetch
  ```    
・[6. pull (ローカルのファイルを、リモートリポジトリの状態に更新)]
  ```
  git pull 
  ```  

## 6.markdown関係
・[Markdown記法一覧 - Qiita](https://qiita.com/oreo/items/82183bfbaac69971917f) <P>
・[Markdown文法まとめ - Qiita](https://qiita.com/higuma/items/3344387e0f2cce7f2cfe) <P>
・[Markdown記法 チートシート - Qiita](https://qiita.com/Qiita/items/c686397e4a0f4f11683d#2-9) <P>
・[Markdown記法 サンプル集 - Qiita](https://qiita.com/tbpgr/items/989c6badefff69377da7) <P>
