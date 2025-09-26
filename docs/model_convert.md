
## 各框架模型模型转换方法

### `tensorflow/keras` 转 `tflite`
```
https://www.tensorflow.org/lite/convert?hl=zh-cn
```

### `onnx/pnnx` 转 `ncnn`
```
https://convertmodel.com/
https://github.com/pnnx/pnnx
```

### `caffe/tensorflow/onnx/pytorch` 转 `paddle lite`
```
https://github.com/PaddlePaddle/X2Paddle
```

### `tensorflow,caffe,onnx,tflite,pytorch` 转 `MNN`
```
https://mnn-docs.readthedocs.io/en/latest/tools/convert.html
```

### `tensorflow,caffe,onnx,tflite,pytorch` 转 `TNN`
```
https://github.com/Tencent/TNN/blob/master/doc/en/user/convert_en.md
```

### `tensorflow,keras,caffe,pytorch` 转 `ONNX`
```
https://github.com/onnx/tutorials#converting-to-onnx-format
```

### `tensorflow,keras,caffe,pytorch` 转 `Tengine-Lite`
```
https://github.com/OAID/Tengine/blob/tengine-lite/doc/docs_zh/user_guides/convert_tool.md


ubuntu编译convert_tool时遇到类似如下protobuf版本问题：
Could NOT find Protobuf: Found unsuitable version "3.0.0", but required is at least "3.6.1" 

解决方法，下载最新protobuf cpp版本：
wget https://github.com/protocolbuffers/protobuf/releases/download/v21.12/protobuf-cpp-3.21.12.tar.gz
tar -xvf protobuf-cpp-3.21.12.tar.gz
编译安装：
cd protobuf-3.21.12
mkdir build
cd build
cmake ../cmake -DCMAKE_BUILD_TYPE=Release -Dprotobuf_BUILD_SHARED_LIBS=ON -Dprotobuf_BUILD_TESTS=OFF
make -j4 install

重新设置软连接:
cp ./libprotobuf.so.3.21.12.0 /usr/lib/x86_64-linux-gnu/
ln -fs libprotobuf.so.3.21.12.0  libprotobuf.so
ln -fs libprotobuf.so.3.21.12.0 libprotobuf.so.32
```



### yolov6 转 onnx/tnn
```
python3 ./tools/convert2tnn/converter.py onnx2tnn ./yolov6n.onnx -o ./yolov6n/ -v v2.0 -optimize -align
```

