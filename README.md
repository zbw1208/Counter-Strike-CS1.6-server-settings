# Counter-Strike 服务器配置（Linux）
  一、设置root账户密码
  
    //  第一次使用Linux服务器时，先设置root账户的密码
    sudo passwd root
  
  二、创建新用户
  
    //  SteamCMD不建议使用root用户运行
    
      #-m：自动创建用户目录
      #-s：指定用户的shell解释器，这里设为bash
      useradd -m -s /bin/bash steam
      
    //  设置用户steam的密码
      sudo passwd steam
      
    //  将steam用户添加到 sudo 用户组（在root用户下操作）：
      sudo adduser steam sudo
    
    //  切换到新建的steam 用户：
      su steam
      
   三、SteamCMD安装
      
      //  安装过程中任何的(Y/n)? 均输入y并Enter
      //  以下代码建议来两遍，确保成功安装
      
      sudo apt update && sudo apt install software-properties-common
      
      sudo add-apt-repository multiverse
      
      sudo dpkg --add-architecture i386
      
      sudo apt install lib32gcc1 libsdl2-2.0-0:i386
      
      sudo apt install steamcmd

      按Tab键选中Ok，按Enter
      
      按DownArrow（方向键下）选中I AGREE,按Enter
      
   四、进入SteamCMD控制台
   
      steamcmd
   
   五、下载游戏服务器文件
      
      //  设置文件下载位置为绝对路径
      force_install_dir /home/steam/cs1.6
      
      //  登录steam，鉴于本人使用匿名登录指令：login anonymous会出现无法连接官方服务器的问题，因此换自己的steam id登录
      login 自己的steam帐号
      
      //  输入密码和手机令牌
      
      //  下载 CS1.6 游戏服务器文件，若报错则再输入一遍app_update 90 validate，直到出现“Success! App '90' fully installed.”
      
      app_set_config 90 mod cstrike 
      
      app_update 90 validate
      
      //  输入quit并回车，退出SteamCMD
      
  六、启动服务器程序
  
      cd
      
      mkdir /home/steam/.steam/sdk32
      cp /home/steam/.steam/steamcmd/linux32/steamclient.so /home/steam/.steam/sdk32
      
      cd ~/cs1.6/cstrike/
      touch listip.cfg banned.cfg && chmod 775 listip.cfg banned.cfg
      
      cd ..
      
      sudo chmod a+x hlds_run hlds_linux hltv
      
      // 启动 cs1.6 服务器，服务器端口为 27015，地图为“de_dust2”，最大容纳人数为16，要关闭按住Ctrl+C即可
      ./hlds_run -game cstrike +port 27015 +map de_dust2 +maxplayers 16
      
  七、开放服务器端口
     
      云服务器管理界面中找到防火墙，添加两个协议分别为TCP和UDP的规则，端口范围均为27015，可满足基本运行需求
     
  [参考文档]：(https://www.wlplove.com/archives/83/#menu_index_2)

