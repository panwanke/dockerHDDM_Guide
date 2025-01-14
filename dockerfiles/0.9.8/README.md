# Docker Multi-platform Build Method

## 0. Installation of buildx
Make sure Docker version is 19.03 or higher.
Run `docker buildx install`.
Create a virtual environment for building using buildx: `docker buildx create --use --name mybuilder`.
You can check the created environments using `docker buildx ls`.

## 1. Build the two images separately
Note: Replace "hcp4715" with your own repo name and "hddm" and "0.9.8" with your own images and tag.
```
docker buildx build --platform linux/arm64 -t hcp4715/hddm:0.9.8-arm64 -f dockerfiles/0.9.8/Dockerfile.arm64 . --load
docker buildx build --platform linux/amd64 -t hcp4715/hddm:0.9.8-amd64 -f dockerfiles/0.9.8/Dockerfile.amd64 . --load
```
Alternatively, you can use `bake` instead of the above commands, but you need to configure the `.hcl` file in advance:
```
docker buildx bake -f docker-bake.hcl --load
```

Run tests:
- note: change the path `D:\dockerHDDM\tutorial` to your custom path
```
docker run -it --rm --cpus=8 -v D:\dockerHDDM\tutorial:/home/jovyan/work -p 8888:8888 hcp4715/hddm:0.9.8-amd64
docker run -it --rm --cpus=8 -v D:\dockerHDDM\tutorial:/home/jovyan/work -p 8888:8888 hcp4715/hddm:0.9.8-arm64
```

# 2. Push Images and Modify Manifest

First, push the two images, and then package the manifest for the two images.

Push images:
- `docker login`
- `docker push hcp4715/hddm:0.9.8-arm64`
- `docker push hcp4715/hddm:0.9.8-amd64`

Modify manifest:
- `docker manifest create hcp4715/hddm:0.9.8RC hcp4715/hddm:0.9.8-amd64 hcp4715/hddm:0.9.8-arm64`
- `docker manifest annotate hcp4715/hddm:0.9.8RC hcp4715/hddm:0.9.8-amd64 --os=linux --arch=amd64`
- `docker manifest annotate hcp4715/hddm:0.9.8RC hcp4715/hddm:0.9.8-arm64 --os=linux --arch=arm64`
- `docker manifest push hcp4715/hddm:0.9.8RC`