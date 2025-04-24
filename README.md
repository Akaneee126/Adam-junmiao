# Adam-junmiao
部署君瞄时可能产生的一些错误

在安装依赖时，可能产生错误并提示rosdep update

我们需要初始化并更新rosdep

执行
sudo rodep init
rosdep update

不过90%的概率会失败，因为http://raw.githubusercontent.com这个地址被墙了，所以实行本地化安装

git clone https://github.com/ros/rosdistro.git

如果你的虚拟机或者设备配置了vpn且遇到了如下问题

unable to access 'https://github.com/ros/rosdistro.git/': Failed to connect to 172.20.10.1 port 1082 after 11284 ms:

说明你的VPN在工作，或者说在错误的工作，导致无法连接到正确的网址，这个时候我们就需要 

1：检查并取消环境变量中的代理设置
unset http_proxy
unset https_proxy
unset all_proxy
# 可能还有别的，比如 no_proxy 之类的，一并检查

2：检查并取消 Git 的代理配置
git config --global --get http.proxy
git config --global --get https.proxy
git config --global --unset http.proxy //如果输出了代理地址
git config --global --unset https.proxy

3：检查并暂时关闭 VPN 或其他代理软件
执行完这些操作通常就能正常git了，如果还不行的话，推荐使用国内的镜像站如清华大学的镜像站https://mirrors.tuna.tsinghua.edu.cn/

配置20-default.list文件
运行

sudo rosdep init
重新生成/etc/ros/rosdep/sources.list.d/20-default.list. 没有则手动创建/etc/ros/rosdep/sources.list.d/20-default.list.

如果sudo rosdep init成功，直接跳过以下创建文件步骤

进入到/etc/ros/目录下

cd /etc/ros
创建rosdep文件

sudo mkdir rosdep && cd rosdep
创建sources.list.d文件

sudo mkdir sources.list.d && cd sources.list.d
创建20-default.list文档

sudo gedit 20-default.list
将20-default.list里面内容修改为下面的代码,注意内容/home/zjl修改为自己的记录路径

# os-specific listings first
yaml file:///home/zjl/rosdistro/rosdep/osx-homebrew.yaml osx

# generic
yaml file:///home/zjl/rosdistro/rosdep/base.yaml
yaml file:///home/zjl/rosdistro/rosdep/python.yaml
yaml file:///home/zjl/rosdistro/rosdep/ruby.yaml
gbpdistro file:///home/zjl/rosdistro/releases/fuerte.yaml fuerte

# newer distributions (Groovy, Hydro, ...) must not be listed anymore, they are being fetched from the rosdistro index.yaml instead
3. 编辑以下文件
/etc/ros/rosdep/sources.list.d/20-default.list
    /usr/lib/python2.7/dist-packages/rosdep2/gbpdistro_support.py
    /usr/lib/python2.7/dist-packages/rosdep2/rep3.py
    /usr/lib/python2.7/dist-packages/rosdistro/__init__.py
搜索文件中的 https://raw.githubusercontent.com/ros/rosdistro/master字段，将其替换为file:///home/yourname/rosdistro(后面的部分保持不变，最好备份一下)

接下来就可以rosdep了
