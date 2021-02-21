参考：
官方教程：https://tianchi.aliyun.com/competition/entrance/231759/tab/174

# 如何使用docker提交代码
## 1.安装docker
Ubuntu系统下：
```
    $ sudo apt-get update
    $ sudo apt install docker.io
```
## 2.开通阿里云容器服务
阿里云容器镜像服务 https://www.aliyun.com/product/acr?
先创建命名空间，再创建镜像仓库
创建好后使用公网地址进行build与push操作

## 3.构建镜像并推送
可直接拉取天池构建好的镜像：
`docker pull registry.cn-shanghai.aliyuncs.com/tcc-public/python:3`
#### 1.准备配置文件
创建`Dockerfile`文件：
`touch Dockerfile`
`Dockerfile`内容如下：
```
    # Base Images
    \#\# 从天池基础镜像构建
    FROM registry.cn-shanghai.aliyuncs.com/tcc-public/python:3

    \#\# 把当前文件夹里的文件构建到镜像的根目录下
    ADD . /

    \#\# 指定默认工作目录为根目录（需要把run.sh和生成的结果文件都放在该文件夹下，提交后才能运行）
    WORKDIR /

    \#\# 镜像启动后统一执行 sh run.sh
    CMD ["sh", "run.sh"]
```
#### 2.构建镜像并推送
在服务器上直接操作:
`docker build -t [镜像仓库公网地址]:[版本号] .`
例如：
`docker build -t registry.cn-shenzhen.aliyuncs.com/test_for_tianchi/test_for_tianchi_submit:1.0 .`
构建好后可先进行测试：
CPU镜像：`docker run your_image sh run.sh`
GPU镜像：`nvidia-docker run your_image sh run.sh`
进行推送：
`docker push [镜像地址]:[版本号]`
例：
`docker push registry.cn-shenzhen.aliyuncs.com/test_for_tianchi/test_for_tianchi_submit:1.0`

## 3.提交结果
在比赛页面点击提交结果，输入镜像地址、用户名、密码。
![提交结果](pushcode.PNG)