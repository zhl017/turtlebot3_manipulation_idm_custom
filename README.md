# TurtleBot3 Manipulation
<img src="https://github.com/ROBOTIS-GIT/emanual/blob/master/assets/images/platform/turtlebot3/manipulation/tb3_with_opm_logo.png">
<img src="https://github.com/ROBOTIS-GIT/emanual/blob/master/assets/images/platform/turtlebot3/manipulation/hardware_setup.png">
# TurtleBot3 Custom Project : TurtleBot3 Mecanum With Open Manipulator
<img src="https://github.com/ROBOTIS-GIT/emanual/blob/master/assets/images/platform/turtlebot3/logo_turtlebot3.png" width="300">

## 規格
- W210

  - 使用馬達：XM430-W210-T
  - 最大線性速度：0.40 m/s
  - 最大角速度：1.76 rad/s

- W350

  - 使用馬達：XM430-W350-T
  - 最大線性速度：0.24 m/s
  - 最大角速度：1.05 rad/s

## 快速安裝手冊

### 1. 環境設定
要使用客製化系列的模型與程式，需要下列一些配置。

#### 1.1. PC安裝

##### 1.1.1. Ubuntu安裝

可選擇直接安裝Ubuntu環境或在windows底下使用虛擬機安裝Ubuntu。

- 在windows底下使用虛擬機安裝Ubuntu。

  - [請參考vmware匯入教學](https://github.com/zhl017/omiyage/blob/main/Documents/vmware%E5%8C%AF%E5%85%A5%E6%95%99%E5%AD%B8.md)。

- 直接安裝Ubuntu環境。
  
  - 在PC下載並安裝Ubuntu20.04。

    - [Ubuntu 20.04 LTS Desktop image (64-bit)](https://releases.ubuntu.com/20.04/ubuntu-20.04.6-desktop-amd64.iso)
    - 詳細Ubuntu步驟請參考[Ubuntu官方教學](https://www.ubuntu.com/download/desktop/install-ubuntu-desktop)。

  - 使用腳本安裝ROS noetic。**(需連接網路)**

    ```
    sudo apt update
    sudo apt upgrade
    wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_noetic.sh
    chmod 755 ./install_ros_noetic.sh
    bash ./install_ros_noetic.sh
    ```

##### 1.1.2. ROS相關packages安裝

- 安裝相關依賴ROS packages。**(需連接網路)**

  ```
  sudo apt-get install ros-noetic-joy ros-noetic-teleop-twist-joy ros-noetic-teleop-twist-keyboard ros-noetic-laser-proc ros-noetic-rgbd-launch ros-noetic-rosserial-arduino ros-noetic-rosserial-python ros-noetic-rosserial-client ros-noetic-rosserial-msgs ros-noetic-amcl ros-noetic-map-server ros-noetic-move-base ros-noetic-urdf ros-noetic-xacro ros-noetic-compressed-image-transport ros-noetic-rqt* ros-noetic-rviz ros-noetic-gmapping ros-noetic-navigation ros-noetic-interactive-markers
  ```

- 選擇下列方式安裝ROS packages。**(需連接網路)**

  - 腳本安裝（package會安裝於catkin_ws/src底下，若想安裝在其他workspace，請使用指令安裝另外創立worksapce。）
 
    ```
    wget https://raw.githubusercontent.com/zhl017/omiyage/main/Setup_script/turtlebot3_om_with_mecanum/pc_setup.sh && chmod +x pc_setup.sh && ./pc_setup.sh && rm pc_setup.sh
    ```

  - 指令安裝 
    ```
    sudo apt remove ros-noetic-turltebot3-msgs
    sudo apt remove ros-noetic-turtlebot3
    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    git clone -b mecanum-devel https://github.com/zhl017/turtlebot3_idm_custom
    git clone -b mecanum-devel https://github.com/zhl017/turtlebot3_manipulation_idm_custom
    git clone https://github.com/zhl017/turtlebot3_msgs_idm_custom
    cd ~/catkin_ws && catkin_make
    echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
    source ~/.bashrc
    ```

#### 1.2. SBC安裝（此步驟提供給自行安裝系統者，若出廠已安裝好系統可以跳過此步驟。）
> 使用者名稱 : ubuntu  
> 使用者密碼 : turtlebot

- 下載[Raspberry Pi映像檔](https://mega.nz/file/URkH2JDI#WpM04Y0Ol83TZz5XpjYXLJRageGnoA6knvVO7DxFLqs)。

- 燒入Raspbeery Pi映像檔。

  ![](https://github.com/ROBOTIS-GIT/emanual/blob/master/assets/images/platform/turtlebot3/setup/rpi_imager.gif)  

- 開機後遠端進入SBC並選擇下列方式安裝相關ROS packages。**(需連接網路)**

  - 腳本安裝（package會安裝於catkin_ws/src底下，若想安裝在其他workspace，請使用指令安裝另外創立worksapce。）
 
    ```
    wget https://raw.githubusercontent.com/zhl017/omiyage/main/Setup_script/turtlebot3_mecanum/sbc_setup.sh && chmod +x sbc_setup.sh && ./sbc_setup.sh && rm sbc_setup.sh
    ```

  - 指令安裝
    
    ```
    sudo apt remove ros-noetic-turltebot3-msgs
    sudo apt remove ros-noetic-turtlebot3
    sudo rm -r catkin_ws
    mkdir -p ~/caktin_ws/src
    cd ~/catkin_ws/src
    git clone -b mecanum-devel https://github.com/zhl017/turtlebot3_idm_custom
    git clone https://github.com/zhl017/turtlebot3_msgs_idm_custom
    git clone -b develop https://github.com/ROBOTIS-GIT/ld08_driver.git
    cd ~/catkin_ws/src/turtlebot3_idm_custom
    sudo rm -r turtlebot3_description/ turtlebot3_teleop/ turtlebot3_navigation/ turtlebot3_slam/ turtlebot3_example/
    cd ~/catkin_ws && catkin_make -j1
    source ~/.bashrc
    ```

#### 1.3. OpenCR安裝（此步驟提供給自行安裝系統者，若出廠已安裝好系統可以跳過此步驟。)

- 將OpenCR連接上SBC或PC上。

- 首次安裝可下載相關套件。

   ```
   sudo dpkg --add-architecture armhf
   sudo apt-get update
   sudo apt-get install libc6:armhf
   ```

- 設定板子窗口、型號，及刪除先前更新韌體包。

   ```
   export OPENCR_PORT=/dev/ttyACM0
   export OPENCR_MODEL=om_with_mecanum
   rm -rf ./opencr_update.tar.bz2
   ```

- 下載新的韌體包並解壓縮。

   ```
   wget https://github.com/zhl017/OpenCR-Binaries/raw/idm-devel/turtlebot3_idm_custom/ROS1/latest/opencr_update.tar.bz2
   tar -xvf opencr_update.tar.bz2
   ```

- 上傳文件至OpenCR。

   ```
   cd ./opencr_update
   ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr
   ```

- 確認終端機顯示 `[OK] jump_to_fw` 字樣表示燒入成功。

### 2. 網路設定

第一次操作可透過**網路線連接**或是自行接螢幕滑鼠至SBC上（後者可跳過2.1步驟）來遠端連接車子進行網路設定。

#### 2.1. 網路線連接
我們設定SBC網路孔固定IP，請使用網路線與PC進行連接。
> IP : **192.168.123.1**

1. PC連接後進入網路設定修改IP選項ipv4修改為手動設定IP。
    ```
    address : 192.168.123.2
    netmask : 255.255.255.0
    gateway : 192.168.123.255
    ```
    
2. 檢查IP位址。
    ```
    ifconfig
    ```
    確認是否有看到 ```192.168.123.2``` 的IP型態。
    
3. 檢查是否與SBC互通。
    ```
    ping 192.168.123.1
    ```

4. 遠端進入SBC並輸入密碼**turtlebot**。
    ```
    ssh ubuntu@192.168.123.1

#### 2.2. 設定SBC連接wifi環境
1. 遠端進入SBC後開啟wifi設定檔案。
    ```
    sudo nano /etc/netplan/50-cloud-init.yaml
    ````

2. 輸入想連接的wifi裝置與密碼，請參考[官方手冊](https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#configure-the-wifi-network-setting-1)。

3. 確認修改完畢後使用快捷鍵 ```ctrl + s``` 儲存以及快捷鍵 ```ctrl + x``` 離開。

4. 輸入指令重新載入配置。
    ```
    sudo netplan apply
    ```
    
#### 2.3. 設定bashrc檔案
確認PC與SBC連接到相同的wifi環境底下並確認各自的IP位址。

1. 修改「~/.bashrc」檔案。
    ```
    nano ~/.bashrc
    ```
    透過使用快捷鍵 ```alt+/``` 幫助您移動到文件最底部，並寫下下列訊息。

    - PC端加入下列參數
      ```
      export ROS_MASTER_URI=http://PC_IP:11311
      export ROS_HOSTNAME=PC_IP
      export TURTLEBOT3_MODEL=mecanum
      export MECANUM_TYPE=$(w210 or w350)
      ```
      >example.
      >PC_IP = 10.1.10.2  
      >export ROS_MATER_URI=http://10.1.10.2:11311  
      >export ROS_HOSTNAME=10.1.10.2

    - SBC端加入下列參數
      ```
      export ROS_MASTER_URI=http://PC_IP:11311
      export ROS_HOSTNAME=SBC_IP
      export TURTLEBOT3_MODEL=mecanum
      export MECANUM_TYPE=$(w210 or w350)
      ```
      >example.
      >SBC_IP = 10.1.10.5  
      >export ROS_MATER_URI=http://10.1.10.2:11311  
      >export ROS_HOSTNAME=10.1.10.5
   
    
3. 確認修改完畢後使用快捷鍵 ```ctrl+s``` 儲存以及快捷鍵 ```ctrl+x``` 離開。

4. 最後，輸入指令重新載入配置。
    ```
    source ~/.bashrc
    ```

## 如何運作
- **開機**  
確認PC與SBC都連到相同的網路且bashrc檔案已設定完畢。

1. 於**PC端**，執行 ROS Master。
    ```
    roscore
    ```

2. 於**PC端**，使用指令遠端SBC。
    ```
    ssh ubuntu@sbc_ip_address    # password : turtlebot
    roslaunch turtlebot3_bringup turtlebot3_robot.launch
    ```
    
    完成上述步驟就能正常遙控車子進行建圖與導航，若需要控制手臂請繼續執行步驟3、4。
    
3. 於**PC端**，執行 OpenManipulator Bringup

    ```
    roslaunch turtlebot3_manipulation_bringup turtlebot3_manipulation_bringup.launch
    ```
    
4. 於**PC端**，執行 move_group

    ```
    roslaunch turtlebot3_manipulation_moveit_config move_group.launch
    ```

- **基本遙控**
1. 於**PC端**，執行車輪遙控範例。
    ```
    roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
    
- **遙控手臂**

    下列兩種方式都可以遙控手臂
    
    - 透過 rviz 控制

    ```
    roslaunch turtlebot3_manipulation_moveit_config moveit_rviz.launch
    ```
        
    - 透過 gui 控制

    ```
    roslaunch turtlebot3_manipulation_gui turtlebot3_manipulation_gui.launch
    ```

- **SLAM (gmapping)**
1. 於**PC端**，執行SLAM建圖。
    ```
    roslaunch turtlebot3_manipulation_slam slam.launch
    ```
2. 於**PC端**，執行儲存地圖。
   ```
   rosrun map_server map_saver -f ~/map
   ```

- **Navigation**
1. 於**PC端**，執行Navigation。
    ```
    roslaunch turtlebot3_manipulation_navigation navigation.launch map_file:=$(map_file_path)
    ```
  
其他相關應用及更詳細解說請參閱 [官方電子手冊](https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/)。
