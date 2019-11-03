# 使用Docker 构建 Lean 大雕的 OpenWRT 路由器固件 编译环境

### 生成镜像文件

自己使用源码创建容器镜像
```
git clone https://github.com/lihaixin/openwrt-docker-builder.git
cd openwrt-docker-builder
docker build -t lihaixin/openwrt-docker-builder - < Dockerfile
```
或者直接下载现成镜像
```
docker pull lihaixin/openwrt-docker-builder
```

### 运行容器

自己更新到代码 根据自己设备配置菜单

```
mkdir -p sanjin
chmod +777 -R sanjin
docker run --rm -it --net=host -v `pwd`/sanjin:/home/sanjin lihaixin/openwrt-docker-builder
git clone https://github.com/coolsnowwolf/lede && cd lede
./scripts/feeds update -a && ./scripts/feeds install -a
make menuconfig
wget https://github.com/lihaixin/openwrt-docker-builder/raw/master/miniconfig
```

集成代码，和dl目录，只需要更新

```
mkdir -p sanjin
chmod +777 -R sanjin
docker run -itd --name openwrt lihaixin/openwrt-docker-builder
docker exec -it openwrt bash
git clone https://github.com/coolsnowwolf/lede && cd lede
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make download
scp -r root@192.168.2.102:/home/newlede/testlede/dl ~/lede/
mkdir ~/openwrt
```
```
docker stop openwrt
docker commit openwrt lihaixin/openwrt-docker-builder:dl
docker login
docker push
```

