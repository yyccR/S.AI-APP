## 如何使用 `S.AI` 部署测试 Yolov5

### 一.部署测试Yolov5(tags v6.0, v6.1, v6.2, v7.0)

1. 拉取代码, 切换到对应分支, v6.0/v6.1/v6.2/v7.0
```shell
git clone https://github.com/ultralytics/yolov5
cd yolov5
git checkout v7.0
```

2. 导出模型(以yolov5s.pt为例), pt->torchscript->pnnx->ncnn
```shell
# export to torchscript, result saved to yolov5s.torchscript
python export.py --weights yolov5s.pt --include torchscript

# download latest pnnx from https://github.com/pnnx/pnnx/releases
wget https://github.com/pnnx/pnnx/releases/download/20250430/pnnx-20250430-linux.zip
unzip pnnx-20250430-linux.zip

# convert torchscript to pnnx and ncnn, result saved to yolov5s.ncnn.param yolov5s.ncnn.bin
./pnnx-20250430-linux/pnnx yolov5s.torchscript.pt inputshape=[1,3,640,640]
```

3. 导入`S.AI`



### 二.部署测试yolov5-segment(v7.0)




### 三.部署测试yolov5-post(v7.0)
