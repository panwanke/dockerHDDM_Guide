# docker 多平台打包方法

## 0. buildx 安装
确保 Docker 版本为 19.03 或更高版本。
运行 `docker buildx install`。
通过 buildx 创建用于打包的虚拟环境。 `docker buildx create --use --name mybuilder`
可通过 `docker buildx ls` 查看创建的环境。

## 1. 分别对两个 images 进行 build
注意: 将 hcp4715 修改为自己的 rep name; 将 hddm 和 0.9.8 设置为自己的 images 和 tag
docker buildx build --platform linux/arm64 -t hcp4715/hddm:0.9.8-arm64 -f dockerfiles/0.9.8/Dockerfile.arm64 . --load
docker buildx build --platform linux/amd64 -t hcp4715/hddm:0.9.8-amd64 -f dockerfiles/0.9.8/Dockerfile.amd64 . --load

可使用 bake 替代以上代码，但需要提前配置 .hcl 文件
docker buildx bake -f docker-bake.hcl --load

运行测试
docker run -it --rm --cpus=8 -v D:\docker\dockerHDDM\hddm098\example:/home/jovyan/work -p 8888:8888 hcp4715/hddm:0.9.8-amd64
docker run -it --rm --cpus=8 -v D:\docker\dockerHDDM\hddm098\example:/home/jovyan/work -p 8888:8888 hcp4715/hddm:0.9.8-arm64


# 2. push images 和 修改 manifest

先将两个 images push，然后对两个 images 的 manifest 进行打包

push images
- `docker login`
- `docker push hcp4715/hddm:0.9.8-arm64`
- `docker push hcp4715/hddm:0.9.8-amd64`

修改 manifest
- `docker manifest create hcp4715/hddm:0.9.8RC hcp4715/hddm:0.9.8-amd64 hcp4715/hddm:0.9.8-arm64`
- `docker manifest annotate hcp4715/hddm:0.9.8RC hcp4715/hddm:0.9.8-amd64 --os=linux --arch=amd64`
- `docker manifest annotate hcp4715/hddm:0.9.8RC hcp4715/hddm:0.9.8-arm64 --os=linux --arch=arm64`
- `docker manifest push hcp4715/hddm:0.9.8RC`