##Remix OS for PC 安装与Xposed框架安装


###一、remix Os下载

>http://www.jide.com/remixos-for-pc

本教程适用于RemixOS_64位系统

###二、RemixOS安装

#####注：操作全在windows下进行，此系统安装后与windows构成双系统

1、下载完成后解压，点击Remix\_OS\_for\_PC\_Installation_Tool.exe文件。

2、单击浏览选择解压包下的iso文件，单击确定进行安装。

3、安装完成后重启，在启动界面选择Remixos引导启动。

4、进行必要初始化设置，至此安装完成。

###三、Xposed框架安装

1、下载必要文件：

	链接：http://pan.baidu.com/s/1jIsmN9W 密码：pm72

2、下载完成后解压，找到Android-X86-installer文件夹，运行注册工具（杀毒 软件可能会报毒，直接信任）

3、注册成功后，运行Android-X86-installer.exe，点击辅助功能后选rom工具点开。

4、在remixos目录下，（你安装在C盘，即在C盘会有这个目录）找到system.sys，复制到桌面。

5、解压system.sfs（桌面上）

6、解包system.img（解压system.sfs会在桌面生成一个UnpackedSFS文件夹，system.img在此文件夹下）。

7、放入Superuser，找到刚刚解压的xposed-v85-sdk23-x86_64文件夹，复制rfiles64\_5  文件夹里的所有内容到桌面UnpackedSFS\system\_文件夹下。

8、放入xposed，找到刚刚解压的xposed-v85-sdk23-x86_64文件夹，复制system文件夹里的所有内容到桌面UnpackedSFS\system\_文件夹下。

9、替换initrd.img，将解压得到的initrd.img复制到C盘的RemixOS文件夹内，替换原有文件。

10、system.img打包，打包后在UnpackedSFS文件夹下会生成system.img。将此文件复制到C盘RemixOS文件夹下，并删除system.sfs文件。

###注：以下步骤在RemixOS下进行

11、重启进入remix os打开re浏览器在system文件夹找到run.sh修改权限位777运行一次。

12、安装XposedInstaller\_by\_dvdandroid-release.apk后重启电脑。

