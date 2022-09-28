# Counter-Strike 服务器配置（Linux）
  ## 一、设置root账户密码
  
    //  第一次使用Linux服务器时，先设置root账户的密码
    sudo passwd root
  
  ## 二、创建新用户
  
    //  SteamCMD不建议使用root用户运行
    
      #-m：自动创建用户目录
      #-s：指定用户的shell解释器，这里设为bash
      useradd -m -s /bin/bash steam
      
    //  设置用户steam的密码
      sudo passwd steam
      
    //  将steam用户添加到 sudo 用户组（在root用户下操作）
      sudo adduser steam sudo
    
    //  切换到新建的steam 用户：
      su steam
      
   ## 三、SteamCMD安装
      
      //  安装过程中任何的(Y/n)? 均输入y并Enter
      //  以下代码建议来两遍，确保成功安装
      
      sudo apt update && sudo apt install software-properties-common
      
      sudo add-apt-repository multiverse
      
      sudo dpkg --add-architecture i386
      
      sudo apt install lib32gcc1 libsdl2-2.0-0:i386
      
      sudo apt install steamcmd

      按Tab键选中Ok，按Enter
      
      按DownArrow（方向键下）选中I AGREE,按Enter
      
   ## 四、进入SteamCMD控制台
   
      steamcmd
   
   ## 五、下载游戏服务器文件
      
      //  设置文件下载位置为绝对路径
      force_install_dir /home/steam/cs1.6
      
      //  登录steam，鉴于本人使用匿名登录指令：login anonymous会出现无法连接官方服务器的问题，因此换自己的steam id登录
      login 自己的steam帐号
      
      //  输入密码和手机令牌
      
      //  下载 CS1.6 游戏服务器文件，若报错则再输入一遍app_update 90 validate，直到出现“Success! App '90' fully installed.”
      
      app_set_config 90 mod cstrike 
      
      app_update 90 validate
      
      //  输入quit并回车，退出SteamCMD
      
  ## 六、启动服务器程序
  
      cd
      
      mkdir /home/steam/.steam/sdk32
      cp /home/steam/.steam/steamcmd/linux32/steamclient.so /home/steam/.steam/sdk32
      
      cd ~/cs1.6/cstrike/
      touch listip.cfg banned.cfg && chmod 775 listip.cfg banned.cfg
      
      cd ..
      
      sudo chmod a+x hlds_run hlds_linux hltv
      
      // 启动 cs1.6 服务器，服务器端口为 27015，地图为“de_dust2”，最大容纳人数为16，要关闭按住Ctrl+C即可，此行命令要在目录~/cs1.6下运行
      ./hlds_run -game cstrike +port 27015 +map de_dust2 +maxplayers 16
      
  ## 七、开放服务器端口
     
      云服务器管理界面中找到防火墙，添加两个协议分别为TCP和UDP的规则，端口范围均为27015，可满足基本运行需求
     
  ## 参考文档：(https://www.wlplove.com/archives/83/#menu_index_2)
  
# 插件安装破解服务器正版限制

  ## 一、安装Metamod
  
    本机下载metamod_1.3.0.86.zip，并解压缩。
  
    su steam
  
    cd ~/cs1.6/cstrike
  
    mkdir addons
  
    cd addons
  
    mkdir metamod
  
    cd ~/cs1.6
  
    sudo chmod a+w cstrike cstrike/addons cstrike/addons/metamod
    
    //  以上代码结束后，将解压缩文件中的addons/metamod目录中所有文件上传至服务器~/cs1.6/cstrike/addons/metamod
  
    vi cstrike/liblist.gam
  
    //  找到gamedll_linux "dlls/cs.so"这一行
    //  按下i，移动光标到这一行，替换为gamedll_linux "addons/metamod/metamod_i386.so"
    //  按下Esc，输入:wq，按回车
  
  ## 二、安装Rehlds
  
    本机下载rehlds-bin-3.11.0.767.zip，并解压缩。
    
    rm ~/cs1.6/engine_i486.so
    
    //  以上代码结束后，将解压缩文件中的bin/linux32/engine_i486.so上传至服务器的~/cs1.6目录
    
  ## 三、安装reunion
  
    本机下载reunion_0.1.92.zip，并解压缩。
    
    mkdir ~/cs1.6/cstrike/addons/reunion
    
    sudo chmod a+w ~/cs1.6/cstrike/addons/reunion
    
    //  以上代码结束后，将解压缩文件中的bin/Linux/reunion_mm_i386.so上传至服务器的~/cs1.6/cstrike/addons/reunion目录
    
    vi ~/cs1.6/cstrike/addons/metamod/plugins.ini
  
    //  按下i，粘贴linux addons/reunion/reunion_mm_i386.so。
    //  按下Esc，输入:wq，按回车
    
    sudo chmod a+w cs1.6
    
    //  上传之前的解压文件夹中的reunion.cfg到~/cs1.6
    
    重新启动服务器程序，如./hlds_run -game cstrike +port 27015 +map de_dust2 +maxplayers 16
    
    然后输入meta list，回车
    出现以下内容表示安装成功
    
    Currently loaded plugins:
      description      stat pend  file              vers      src   load  unlod
    [1] Reunion          RUN   -    reunion_mm_i386.  v0.1.0.9  ini   Start Never 
    1 plugins, 1 running

# 安装插件增加游戏功能

  ## 安装 AMX Mod X
  
    //  本机下载amxmodx.tar.gz，上传至~/cs1.6/cstrike/addons目录，并在该目录解压
  
    tar -zxvf amxmodx.tar.gz
  
    vi metamod/plugins.ini
  
    //  按i，新一行添加linux addons/amxmodx/dlls/amxmodx_mm_i386.so，按Esc，输入:wq，按回车
  
    重新启动服务器程序，如./hlds_run -game cstrike +port 27015 +map de_dust2 +maxplayers 16
  
    然后输入meta list，回车
    出现以下内容表示安装成功
  
    Currently loaded plugins:
    description      stat pend  file              vers      src   load  unlod
    [ 1] Reunion          RUN   -    reunion_mm_i386.  v0.1.0.9  ini   Start Never
    [ 2] AMX Mod X        RUN   -    amxmodx_mm_i386.  v1.8.2    ini   Start ANY  
    2 plugins, 2 running
