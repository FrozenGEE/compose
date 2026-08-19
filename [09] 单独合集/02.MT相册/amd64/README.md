#  mtphotos/mt-photos-ai 镜像具体tag如下

- latest：基于openvino文件夹打包，推荐Intel Xeon、Core系列 CPU机型安装这个镜像
```
image: mtphotos/mt-photos-ai:latest
```
- onnx-latest:基于onnx文件夹打包，如果您的CPU不在OpenVINO的支持列表中或者是AMD CPU，请使用该镜像，例如N5105
```
image: mtphotos/mt-photos-ai:onnx-latest
```
- cuda-latest:基于cuda文件夹打包，nvidia显卡机型可以安装这个镜像
```
image: mtphotos/mt-photos-ai:cuda-latest
```
