# Html2Pdf安装

## 1、下载安装包

在下面的Html2Pdf安装包下载列表页面，选择自己需要的操作系统Html2Pdf安装包进行下载

```shell
http://wkhtmltopdf.org/downloads.html
```



## 2、安装

Centos8.1执行更新报错

```shell
# yum update
Failed to download metadata for repo 'AppStream'
Error: Failed to download metadata for repo 'AppStream'
```

解决办法:

```shell
# cd /etc/yum.repos.d/
# sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
# sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
# yum update -y
```

```shell
# rpm -ivh wkhtmltox-0.12.6-1.centos8.x86_64.rpm
```

安装wkhtmltopdf的rpm安装包时报异常

```shell
error: Failed dependencies:
	xorg-x11-fonts-75dpi is needed by wkhtmltox-1:0.12.6-1.centos8.x86_64
	xorg-x11-fonts-Type1 is needed by wkhtmltox-1:0.12.6-1.centos8.x86_64
```

解决办法:

```shell
# yum search 75dpi
# yum install xorg-x11-fonts-75dpi.noarch

# yum search xorg-x11-fonts-Type1
# yum install xorg-x11-fonts-Type1.noarch

# rpm -ivh wkhtmltox-0.12.6-1.centos8.x86_64.rpm 
```

## 3、测试

```shell
# wkhtmltopdf www.bing.com a.pdf
```

能生成 a.pdf 文件 表示安装成功

