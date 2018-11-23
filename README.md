# shell_for_ss_ssr_ssrr_kcptun_bbr_installation
参数设置已屏蔽，如需指定参数，取消参数设置部分“read”部分注释即可！
<a name="Install_command">安装命令：
```Bash
wget --no-check-certificate -O /root/ss_ssr_ssrr_kcp_bbr.sh https://raw.githubusercontent.com/Jenking-Zhang/shell_for_ss_ssr_ssrr_kcptun_bbr/master/ss_ssr_ssrr_kcp_bbr.sh
chmod +x /root/ss_ssr_ssrr_kcp_bbr.sh
/root/ss_ssr_ssrr_kcp_bbr.sh install 2>&1 | tee install.log
```
OPENVZ开启了TAP/TUN会提示安装BBR！

<a name="Install_command">非OPENVZ安装BBR：
```Bash
wget -O /root/bbr_kvm.sh --no-check-certificate https://raw.githubusercontent.com/Jenking-Zhang/shell_for_ss_ssr_ssrr_kcptun_bbr/master/bbr_kvm.sh
chmod +x /root/bbr_kvm.sh
/root/bbr_kvm.sh 2>&1 | tee bbr_install.log
```
需执行两次，自动重启，第一次安装内核，第二次安装BBR暴力版。非OPENVZ架构的VPS，建议先更换内核，安装BBR后，再安装SS/SSR/SSRR！

查看系统上glibc的版本号，最低需要2.14，
<a name="Install_command">执行命令：
```Bash
rpm -qa |grep glibc
```
<a name="Install_command">或
```Bash
yum  list installed |grep glibc
```
OpenVZ 更新glibc到2.15
方法一：rpm安装
<a name="Install_command">先下载以下几个文件：
```Bash
wget http://ftp.redsleeve.org/pub/steam/glibc-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/glibc-common-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/glibc-devel-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/glibc-headers-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/nscd-2.15-60.el6.x86_64.rpm
```
<a name="Install_command">然后安装
```Bash
rpm -Uvh glibc-2.15-60.el6.x86_64.rpm \
glibc-common-2.15-60.el6.x86_64.rpm \
glibc-devel-2.15-60.el6.x86_64.rpm \
glibc-headers-2.15-60.el6.x86_64.rpm \
nscd-2.15-60.el6.x86_64.rpm
```
<a name="Install_command">方法二：编译安装（上面的方式行不通，再用编译的方式，编译的方式相对时长会久很多）
```Bash
wget http://ftp.gnu.org/gnu/glibc/glibc-2.15.tar.gz
wget http://ftp.gnu.org/gnu/glibc/glibc-ports-2.15.tar.gz
tar -zxf glibc-2.15.tar.gz
tar -zxf glibc-ports-2.15.tar.gz
mv glibc-ports-2.15 glibc-2.15/ports
mkdir glibc-build-2.15
cd glibc-build-2.15
../glibc-2.15/configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
make all && make install
```
<a name="Install_command">检查是否成功安装
```Bash
ldd --version
```
<a name="Install_command">成功安装的输入如下
```Bash
Copyright (C) 2012 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
Written by Roland McGrath and Ulrich Drepper.
```
