# s3fs-easyinstall
### 简介

s3fs 能帮助您快速使用华为企业云对象存储功能，在Linux/Mac OS X 系统中把hwclouds OBS bucket 挂载到本地文件
系统中，方便快捷的通过本地文件系统操作OBS 上的对象，实现数据的的共享，以及各种二次开发。

s3fs-easyinstall 为常见的linux发行版制作了安装包，方便您的安装使用，目前提供的安装包有：
- Ubuntu-14.04
- CentOS-7.0/6.5/5.11

### 安装

选择对应的安装包下载安装，建议选择最新版本。

- 对于Ubuntu14，安装命令为：
```
wget https://github.com/hyperwd/s3fs-easyinstall/releases/download/v1.80/s3fs_1.80_ubuntu14.04_amd64.deb -O s3fs_1.80_ubuntu14.04_amd64.deb
sudo apt-get update
sudo apt-get install gdebi-core
sudo gdebi s3fs_1.80_centos7.0_x86_64.rpm
```

- 对于CentOS7，安装命令为：
```
sudo yum remove fuse*
wget https://github.com/hyperwd/s3fs-easyinstall/releases/download/v1.80/s3fs_1.80_centos7.0_x86_64.rpm -O s3fs_1.80_centos7.0_x86_64.rpm
sudo yum localinstall s3fs_1.80_centos7.0_x86_64.rpm
```

- 对于CentOS6，安装命令为：
```
wget https://github.com/hyperwd/s3fs-easyinstall/releases/download/v1.80/s3fs_1.80_centos6.5_x86_64.rpm -O s3fs_1.80_centos6.5_x86_64.rpm
sudo yum localinstall s3fs_1.80_centos6.5_x86_64.rpm
```

- 对于CentOS5，安装命令为：
```
wget https://github.com/hyperwd/s3fs-easyinstall/releases/download/v1.80/s3fs_1.80_centos5.11_x86_64.rpm -O s3fs_1.80_centos5.11_x86_64.rpm
sudo yum localinstall s3fs_1.80_centos5.11_x86_64.rpm --nogpgcheck
```

### 挂载示例

获取AK,SK信息，将其存放在/etc/passwd-ossfs 文件中，注意这个文件的权限必须正确设置，建议设为640。或者将其存放在~/.passwd-s3fs文件中，权限建议600

假设我想要将`obs-411174` 这个桶挂载到云主机`/tmp/obs`目录下，AccessKeyId(AK)是`OBIOPI`,
Secret Access Key(SK)是`rdh8s1`

```
echo "OBIOPI:rdh8s1" > /etc/passwd-s3fs
chmod 640 /etc/passwd-s3fs
mkdir /tmp/obs
s3fs obs-411174 /tmp/obs -o url=http://obs.myhwclouds.com
grep s3fs /etc/mtab #验证挂载信息
df -Th /tmp/obs #验证目录
```
或者

```
echo "OBIOPI:rdh8s1" > ~/.passwd-s3fs
chmod 600 ~/.passwd-s3fs
mkdir /tmp/obs
s3fs obs-411174 /tmp/obs -o url=http://obs.myhwclouds.com
grep s3fs /etc/mtab #验证挂载信息
df -Th /tmp/obs #验证目录
```

卸载桶:

```
umount /tmp/ossfs # root user
fusermount -u /tmp/ossfs # non-root user
```
- 使用`ossfs --version`来查看当前版本，使用`ossfs -h`来查看可用的参数

#### 常用设置
- s3fs支持多组AK,SK的使用，即同时挂载多组OBS桶。编辑/etc/passwd-s3fs或~/.passwd-s3fs
        
        bucket1:AK:SK
        bucket2:AK:SK
- 默认挂载命令，赋予挂载文件夹（如/tmp/obs）root权限，allow_other参数可以开放其他用户访问,注意：allow_other是赋予挂载目录其他用户访问的权限，不是里面的文件！如果您要更改文件夹中的文件，请用chmod命令

        
        s3fs bucket_name your_mount_point -o url=http://obs.myhwclouds.com -o allow_other          
- allow_other默认赋予挂载目录777权限，我想让挂载目录的权限为770，该怎么办？我想直接将挂载文件夹(如/tmp/obs)赋予普通用户，该怎么办？更多权限设置，请参考FAQ

- fusermount -u your_ount_point卸载出错,fusermount: failed to unmount /your_mount_point: Device or resource busy


        fusermount -uz your_mount_point
      
 
#### 高级设置

- 可以添加`-f -d`参数来让ossfs运行在前台并输出debug日志，重新挂载，打开debug，然后重复你出错的步骤，再次查看/tmp/s3fs.log,尝试理解分析错误，或者发给我
      
        s3fs -d -f bucket_name your_mount_point -o url=http://obs.myhwclouds.com > /tmp/s3fs.log 2>&1

### 常见问题
FAQ

### 相关链接

* [libfuse](https://github.com/libfuse/libfuse)
* [s3fs](https://github.com/s3fs-fuse/s3fs-fuse) - 通过fuse接口，mount s3 bucket到本地文件系统。

### 联系我们

### License
