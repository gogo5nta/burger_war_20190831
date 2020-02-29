# 1. Jetson nanoのセットアップ(HW)
## 1.1 ケース、ファン、SD Cardの購入
・[Jetson Nano＋パーツの購入](https://www.nakasha.co.jp/future/ai/vol2_yolov3nvidia_jetson_nano.html)  
 (1）[Geekworm NVIDIA Jetson Nano用金属ケース、電源およびリセットコントロールスイッチ付きのメタルシェル、Jetson Nano Boardに適用](https://www.amazon.co.jp/gp/product/B07RZL6TTM/ref=ppx_yo_dt_b_asin_title_o06_s00?ie=UTF8&psc=1)  
 (2) [SUCCUL ACアダプター 5V 4A 大手メーカーOEM社製品 センタープラス スイッチング式 最大出力20W 出力プラグ外径5.5mm(内径2.1mm)PSE取得品](https://www.amazon.co.jp/gp/product/B015RKFAA2/ref=ppx_yo_dt_b_asin_title_o05_s00?ie=UTF8&psc=1)  
 (3) [microSDXC 128GB SanDisk サンディスク Extreme UHS-1 U3 V30 4K Ultra HD A2対応 SDアダプター付 [並行輸入品]](https://www.amazon.co.jp/gp/product/B07H89SQTH/ref=ppx_yo_dt_b_asin_title_o04_s00?ie=UTF8&psc=1)  
 (4) [Noctua NF-A4x20 5V PWM 40mm 静音 ファン 5000rpm](https://www.amazon.co.jp/gp/product/B071FNHVXN/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1)  

## 1.2 ケース、ファンの取り付け
 ・[Jetson Nano＋パーツの組み立て](https://www.nakasha.co.jp/future/ai/vol2_yolov3nvidia_jetson_nano.html)  
 ・[NVIDIA Jetson Nano Metal case](http://raspberrypiwiki.com/index.php/N100)

 ![j40](https://user-images.githubusercontent.com/30023363/64614378-5a8a7000-d413-11e9-9844-1dd67de8ac1b.jpg)  
 40pinをジャンパーピン(↑写真の一番左)  
 ![j48](https://user-images.githubusercontent.com/30023363/64614465-8a397800-d413-11e9-856c-3e84c960aaa1.jpg)  
 48pinをジャンパーピン  
 ![fan](https://user-images.githubusercontent.com/30023363/64614525-a50bec80-d413-11e9-9b19-094fe7cd8529.jpg)  
 ファンを4Pinコネクタで接続  
 ![gpio](https://user-images.githubusercontent.com/30023363/64614564-b5bc6280-d413-11e9-94b6-ec2ccba08f07.jpg)  
 GPIOの1Pin(3.3V)に青、9Pin(GND)に白のファンケーブルと接続。ただし初期では4Pinファンは起動せず。  
 [このページを参考](http://www.neko.ne.jp/~freewing/raspberry_pi/nvidia_jetson_nano_tips/)に起動後FANの動作確認を実施する事(最初にハマります)
 ![build](https://user-images.githubusercontent.com/30023363/64614660-eb614b80-d413-11e9-8e45-091a4f097c61.jpg)  
 ケースにリセットスイッチ(上)と電源スイッチ(下)を取り付けた例
 ![600px-IMG-6050-1](https://user-images.githubusercontent.com/30023363/64614827-4dba4c00-d414-11e9-82fa-7a5855c163aa.jpg)  
 リセットスイッチと電源スイッチの取り付け例
  
# 2. Jetson nanoのセットアップ(SW)
・[(全般的に参考になる)Jetson Nano関係のTIPSまとめ](https://qiita.com/karaage0703/items/b14c249aa33112669ee4)  
  
## 2.1 SDカードにOSをインストール
 ・[小型 AI コンピュータ NVIDIA Jetson Nano 開発者キットのセットアップ](https://qiita.com/tsutof/items/66e68c75b462c4faf1cb)  
 ・[第1回ゼロから始めるJetson : nanoセットアップ](https://digital-light.jp/2019/04/28/how-to-set-up-nvidia-jetson-nano/)  
 まず、[Jetson DOwnload Center](https://developer.nvidia.com/embedded/downloads)からJetson Nano Developer Kit SD Card Image  
 をダウンロード  
 ![001](https://user-images.githubusercontent.com/30023363/64615269-2fa11b80-d415-11e9-9d65-3cd35948a097.jpg)  
   
 次に、必要なら[SD Memory Card Formatter](https://www.sdcard.org/downloads/formatter/eula_windows/)でSDカードをフォーマット  
 ![006](https://user-images.githubusercontent.com/30023363/64616896-62004800-d418-11e9-8f06-7426e80589e2.png)  
   
 次に、[Etcher](https://www.balena.io/etcher/)というソフトを入手し、ダウンロードしたImageをSDカードに書き込み　　
 ![002](https://user-images.githubusercontent.com/30023363/64615408-78f16b00-d415-11e9-98fd-db0b456f5d5d.png)　　
 　　
 最後に、[公式ページ](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#setup)で説明のとおり  
 Jetson Nano 開発者キットにマイクロ SD カードを挿入し、ディスプレイ、キーボード、マウスを接続。最初は有線LANで接続の方が安定。  
 参考までに、Jetson NanoはWiFiやBluetoothを搭載していないため、USBタイプのWiFiドングルを購入する事をお勧め。  
 ![003](https://user-images.githubusercontent.com/30023363/64615600-d980a800-d415-11e9-918a-e4ebecf23fb4.jpg)  
   
## 2.2 初期セットアップ
 SDカードをセットし、電源をOnにすると起動画面が表示  
 ![004](https://user-images.githubusercontent.com/30023363/64616061-b5719680-d416-11e9-9e1c-aa2c14c9d235.jpg)
  
 セットアップ中、言語を選択するが、最初は必ずEnglishを選択(日本語選択ではインストールできないソフトがあるらしい)  
 ![007](https://user-images.githubusercontent.com/30023363/64617036-a7247a00-d418-11e9-9dc4-5958f9682a99.jpg)  
   
 自分が使っているキーボードを選択します。Japaneseのキーボードが一般的。  
 ![008](https://user-images.githubusercontent.com/30023363/64617152-ec48ac00-d418-11e9-9698-0687bbe61f39.jpg)  
   
 セットアップが完了すると、以下のデスクトップが起動  
 ![009](https://user-images.githubusercontent.com/30023363/64617211-04b8c680-d419-11e9-83df-faf97812e821.png)  

## 2.3 SWAPファイルの設定
[installSwapfile](https://github.com/JetsonHacksNano/installSwapfile)ここをダウンロードし実行すると6GBのSWAP領域を自動作成。  
Swapファイルのサイズを変更したい場合は、installSwapFile.shのSWAPSIZE=6を好きな数字に変更  
```
$ git clone https://github.com/JetsonHacksNano/installSwapfile
$ cd installSwapfile
$ ./installSwapfile.sh
```

## 2.4 パフォーマンスの最大化
・[Jetson Nano関係のTIPSまとめ](https://qiita.com/karaage0703/items/b14c249aa33112669ee4)  
・[NVIDIA Jetson Nano高速化](https://www.usagi1975.com/201906272044/)  
以下コマンドでパフォーマンスモードを変更できます。-mの後の数字が0のときが最大(Maxモード）で1が最小（5Wモード）  
ただし、Maxモード(20Wモード)ではJ48にジャンパーピンを行い、ACアダプタ給電が必要。
```
$ sudo nvpmodel -m 0
```
以下コマンドを実行すると、クロック周波数などのパフォーマンスを最大化できます。FANがついていれば、FANも最大速度で動作  
```
$ sudo jetson_clocks
```
設定確認は以下を実行  
```
$ sudo nvpmodel -q
$ sudo jetson_clocks --show
```
  
起動時からフルパワーにしたい場合、[このページ](http://www.neko.ne.jp/~freewing/raspberry_pi/nvidia_jetson_nano_cron_crontab_autoexec_script/)を参考  
### 2.4.1 スクリプトファイル作成
/home/[user]/nano_start.shを作成  
```
#!/bin/sh

sleep 5
sudo nvpmodel -m 0
sleep 5
sudo jetson_clocks
```
### 2.4.2 chmodでroot権限を付加  
```
$ chmod 755 nano_start.sh
```
### 2.4.3 sudoを付けた crontabで cronファイルを編集
```
$ sudo crontab -e
```
以下の行を追加。基本のエディターはviなので、viの使い方を覚えておく。  
[viの使い方](http://www.gi.ce.t.kyoto-u.ac.jp/user/susaki/command/vi.html)  
[crontab ファイルの作成と編集](https://docs.oracle.com/cd/E19253-01/819-0379/sysrescron-24589/index.html)  
i   コマンドモード→入力モード  
Esc 入力モード→コマンドモード
:wq (コマンドモード)保存して終了  
```
@reboot    /home/[user]/nano_start.sh
```

## 2.5 日本語の設定
 ・[日本語環境の構築](https://gihyo.jp/admin/serial/01/ubuntu-recipe/0563?page=3)  
 ・[Jetson Nano:日本語入力できるようにする](https://toyo-interest.com/jetson-nano/jetson-nano%E6%97%A5%E6%9C%AC%E8%AA%9E%E5%85%A5%E5%8A%9B%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/)  
   日本語入力をとりあえずできるようにする。  
```
# 日本語フォントをインストール
$ sudo apt-get install fonts-ipafont fonts-ipaexfont

# アップデート
$ sudo apt-get update

# Google日本語入力のMozcをインストール
$ sudo apt-get install fcitx-mozc

# インストール後はnanoを再起動
$ sudo reboot now
```
再起動後GUI右上のJPマークが「キーボードマークとjp」に  
ブラウザを立ち上げて、キーボードの半角全角ボタンを押すと、日本語入力に切替　　

## 2.6 VirualGL+TurboVNCでリモート上のOpenGLをローカルで表示
 ・[[HowTo]Install VirtualGL and TurboVNC to Jetson TK1](https://devtalk.nvidia.com/default/topic/828974/jetson-tk1/-howto-install-virtualgl-and-turbovnc-to-jetson-tk1/2)  
 ・[リモートでOpenGLを動かしてローカルにその画面を写す方法](http://geraniums.hatenablog.com/entry/2018/05/25/151153)  
 ・[リモートでCUDAとOpenGLが使えるようにする](https://qiita.com/exthnet/items/dcb0bd94f09a2b4c9835)  
 ・[リモート操作](https://qiita.com/koppe/items/64d155bc7814441920f9)  
 

 1. ファイルの入手  
 ```
//wget http://demotomohiro.github.io/hardware/jetson_tk1/pkg/libjpeg-turbo_1.5.1_arm64.deb
wget http://demotomohiro.github.io/hardware/jetson_tk1/pkg/virtualgl_2.5.1_arm64.deb
wget http://demotomohiro.github.io/hardware/jetson_tk1/pkg/turbovnc_2.0.91_arm64.deb
```
 2. インストール  
 ```
//sudo dpkg -i libjpeg-turbo_1.5.1_arm64.deb
//sudo apt-get install libtubojpeg*
sudo dpkg -i virtualgl_2.5.1_arm64.deb
sudo dpkg -i turbovnc_2.0.91_arm64.deb
sudo apt-get install xfce4
echo xfce4-session >~/.xsession
 ```
 3. 設定  
``` 
// lightdm停止
 sudo systemctl stop lightdm

// リモートのVirtualGL の設定(1,y,y,yの順番でOK。詳細はhttps://virtualgl.org/vgldoc/2_2_1/#hd005001)
 sudo /opt/VirtualGL/bin/vglserver_config

// lightdm開始
sudo systemctl start lightdm
xauth merge /etc/opt/VirtualGL/vgl_xauth_key

// リモートでTurboVNCの起動　(パスワードの初期設定は8文字にしないと短いといわれる。再起動の度に以下を実行後、リモートから接続)
/opt/TurboVNC/bin/vncserver -depth 24
```
 リモート先を起動する毎に、TurboVNCを起動する必要があるため、Aliasで設定すると良い  
 $ nano .bashrc
 ```
 # TurboVNCの起動
 alias vnc='/opt/TurboVNC/bin/vncserver -depth 24'
 ```
 
 WindowsのTurboVNCの接続画面  
 ![07](https://user-images.githubusercontent.com/30023363/64484882-78a97200-d253-11e9-9071-18a2c12a4886.jpg)  
 接続し、Gazeboを起動してOpenGLのリモート表示を確認  
 ![06](https://user-images.githubusercontent.com/30023363/64484897-9e367b80-d253-11e9-9f1d-fc107b51b107.jpg)

#2.7 テキストエディタnanoをインストール
```
$ sudo apt -y install nano
```
  
3. ローカルからリモートへVNC接続  
https://ja.osdn.net/projects/sfnet_turbovnc/downloads/2.2.2/TurboVNC-2.2.2-x64.exe/からWindows用ファイルを入手。
　TurboVNCを起動した時:1であれば普通5901番ポートなので  
　VNCで<IPアドレス>:1に接続  
　真っ黒な画面が表示されれば成功  

# 3. ROSをインストール
## 3.1 ROS(ubuntu 18.04＋melodic)のインストール
・[Jetson NanoにROSをインストールする方法](https://qiita.com/karaage0703/items/aa54e086f0a2f165d3e9)  

# 4. Dockerを起動
## 4.1 Dockerの確認
・[Jetson NanoでDockerを動かす(前編)](https://qiita.com/yamamo-to/items/33f57f9a059cc95b9c08)  
