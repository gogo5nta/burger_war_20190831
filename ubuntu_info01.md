# 1. Ubuntuメモ (便利な設定編)
## 1.1 日本語フォルダを英語にする
 ・[開発環境を構築する](http://zow.hatenablog.com/entry/20160320/1458473750)　 
 ```
//日本語⇒英語
$ LANG=C xdg-user-dirs-gtk-update

//英語⇒日本語
$ LANG=ja_JP.UTF-8 xdg-user-dirs-gtk-update 
 ```

## 1.2 Ubuntuにsshで設定
 ・[ubuntuにsshで接続する](https://qiita.com/a_k0406/items/660e686285ead5ac93f4)　 
  ubuntuの端末でssh接続を許可できるよう以下のコマンドを入力  
  ```
$ sudo apt-get install openssh-server
  ```
  UbuntuのIPアドレスを確認しておく  
  ```
$ ifconfig  
  ```
  Windows上では、teratermかminttyをインストール  
 ・[Windowsターミナルソフト + Rlogin が最高すぎる！！](https://qiita.com/murachi1208/items/d6e4ce7ba75f1625fe51) 
   画面分割や色分けができサーバーリストも保存可能。terminatorみたいな感覚で使用可能。
   ![02](https://user-images.githubusercontent.com/30023363/64484329-717e6600-d24b-11e9-8fed-db4b0e53f8fc.png)  
  Rloginの接続例  
  ![04](https://user-images.githubusercontent.com/30023363/64484399-5102db80-d24c-11e9-94b9-081e00bb592f.jpg)  
  
・[teraterm](https://forest.watch.impress.co.jp/library/software/utf8teraterm/)  
 　 ![03](https://user-images.githubusercontent.com/30023363/64484368-f0739e80-d24b-11e9-8b15-082607a7ff24.jpg)  
  teratermの設定例  
  ![01](https://user-images.githubusercontent.com/30023363/64484060-fcf5f800-d247-11e9-8365-79221b5fa549.png)  
  
## 1.3 固定IP(sshに便利)
 ・[Ubuntu16.04で固定IPアドレスを設定する方法(CUI編)](https://ygkb.jp/3672)　  
 ```
 //ip addressコマンドでNICの一覧を確認。
 //inet 192.168.1.203/24 brd 192.168.1.255 scope global eth0・・・
 $ ip address

 //簡単にならifconfigでもOK
 //
 $ ifconfig

// /etc/network/interfacesの書き換え
sudo nano /etc/network/interfaces
 ```
 「iface eth0 inet static」にして固定IP用の設定を追記  
 ```
 # The primary network interface
auto eth0
iface eth0 inet dhcp
 ```
 　↓  
 ```
 # The primary network interface
auto eth0
    iface eth0 inet static         # "static"が固定アドレスを表す
    address 192.168.1.101          # 振りたいIPアドレス
    netmask 255.255.255.0          # ネットワークマスク
    gateway 192.168.1.254          # デフォルトゲートウェイのIPアドレス
    dns-nameservers 192.168.1.254  # DNSサーバーのIPアドレス
 ```

## 1.4 ホストネームの変更
 ・[Ubuntu16.04でホスト名を変更する方法](https://ygkb.jp/3974)　 
  Ubuntu 16.04 Serverでホスト名を変更する時は  
  /etc/hostname  
  /etc/hosts  
  の２ファイルを修正し、再起動($ sudo reboot)

  /etc/hostnameの編集  ($ sudo nano /etc/hostname)  
  ```
ygkb
  ```
　↓  
 ```
jisaku
 ```  

  /etc/hostsの編集  ($ sudo nano /etc/hosts)  
 ```
127.0.0.1       localhost
127.0.1.1       ygkb
 ```
　↓  
 ```
127.0.0.1       localhost
127.0.1.1       jisaku 
 ```

## 1.5 VirualGL+TurboVNCでリモート上のOpenGLをローカルで表示
 ・[リモートでOpenGLを動かしてローカルにその画面を写す方法](http://geraniums.hatenablog.com/entry/2018/05/25/151153)  
 ・[リモートでCUDAとOpenGLが使えるようにする](https://qiita.com/exthnet/items/dcb0bd94f09a2b4c9835)  
 1. VirtualGLインストール  
 ```
 // リモート上でVirtualGLファイル入手
 wget https://sourceforge.net/projects/virtualgl/files/2.6.2/virtualgl_2.6.2_amd64.deb/download -O virtualgl_2.6.2_amd64.deb 

// リモートにVirtualGL のインストール
 sudo dpkg -i virtualgl_*_amd64.deb

// lightdm停止
 sudo systemctl stop lightdm

// リモートのVirtualGL の設定(1,y,y,yの順番でOK。詳細はhttps://virtualgl.org/vgldoc/2_2_1/#hd005001)
 sudo /opt/VirtualGL/bin/vglserver_config

// lightdm開始
sudo systemctl start lightdm
xauth merge /etc/opt/VirtualGL/vgl_xauth_key

 ```
 2. TurboVNCインストール  
 ```
 // リモート上でTurboVNCファイル入手
 wget https://sourceforge.net/projects/turbovnc/files/2.2.2/turbovnc_2.2.2_amd64.deb/download -O turbovnc_2.2.2_amd64.deb
 
 // リモートにTurboVNC のインストール
 sudo dpkg -i turbovnc*.deb 
 
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
  
3. ローカルからリモートへVNC接続  
https://ja.osdn.net/projects/sfnet_turbovnc/downloads/2.2.2/TurboVNC-2.2.2-x64.exe/からWindows用ファイルを入手。
　TurboVNCを起動した時:1であれば普通5901番ポートなので  
　VNCで<IPアドレス>:1に接続  
　真っ黒な画面が表示されれば成功  

4. gazeboの更新(ubuntu16.04 + ros kinetic)
kineticに入ってるgazeboはバグがあって、リモート接続で落ちる場合有。例えば7.11などにアップデート  
・[ROS環境で最新のGazeboを取得する](http://scnsh.hatenablog.com/entry/2018/03/04/183800)
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install gazebo7 -y
```

# 2. Ubuntuメモ (エラー編) 
## 2.1 apt-getで失敗
 以下のエラーが出た際の対処
 ・[dpkgのlock解除](http://inside-my-box.hatenablog.com/entry/2015/05/29/013008)　
```
E: /var/lib/apt/lists/lock が取得できませんでした - open (11: リソースが一時的に利用できません) 
```
　対処方法は、recovery modeで起動し、rootでログイン後、以下を実行  
 ```
 # リマウント
 mount -o remount,rw /
 # 修正
rm /var/lib/apt/lists/lock
rm /var/lib/dpkg/lock
apt-get update
apt-get install -f
 ```
   
 ## 2.2 CUIで文字化けの対策
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
