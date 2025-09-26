# 推理框架依赖库编译(mac)

## ncnn动态库
```
nihui版本: https://github.com/nihui/ncnn/releases/tag/20221122
tecent版本: https://github.com/Tencent/ncnn/releases
```

## paddle-lite动态库
``` 
```

## tflie动态库
1. `clone`对应版本的tensorflow
```
git clone -b v2.10.0 https://github.com/tensorflow/tensorflow.git
```

2. `configure`配置
```
cd ./tensorflow/
./configure

Do you wish to build TensorFlow with ROCm support? [y/N]: n

Do you wish to build TensorFlow with CUDA support? [y/N]: n

Do you wish to download a fresh release of clang? (Experimental) [y/N]: n

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -Wno-sign-compare]: 

Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: n

Do you wish to build TensorFlow with iOS support? [y/N]: n
```   

3. 编译`tflite`动态库, 默认已经支持`NNAPI`, 首选使用`bazel`编译tf, 次选`cmake`, 在转换模型的时候如果用到`tf.lite.OpsSet.SELECT_TF_OPS`, `bazel`编译将支持tf本身部分算子, `cmake`则不支持这部分
```
# M1/M2... cpu
bazel build -c opt --config=macos_arm64 --cxxopt="-std=c++17" //tensorflow/lite:libtensorflowlite.dylib

# Intel cpu
bazel build -c opt --cxxopt="-std=c++17" //tensorflow/lite:libtensorflowlite.dylib
```

4. 构建 `libtensorflowlite_gpu_delegate.dylib`, 需要先构建1步骤

(1) `uname -a`查看cpu架构, 如果是`x86_64`, 替换`tensorflow/bazel-tensorflow/external/cpuinfo/BUILD.bazel`里面 `"cpu": "darwin"` 为`"cpu": "darwin_x86_64"`
(2) 在根目录编译,需指定`--cxxopt=-std=c++17`
```
bazel build -c opt --copt -Os --copt -DTFLITE_GPU_BINARY_RELEASE --copt -fvisibility=hidden --linkopt -s --strip always --cxxopt=-std=c++17 //tensorflow/lite/delegates/gpu:tensorflow_lite_gpu_dylib --apple_platform_type=macos --cpu=darwin_x86_64 --macos_cpus=x86_64

install_name_tool -id "/Users/yang/CLionProjects/test_tflite/tflite-2.10.0/tflite2.10.0_lib/mac-os/gpu/tensorflow_lite_gpu_dylib_bin" tensorflow_lite_gpu_dylib.dylib
```

## mnn动态库编译
```
```

## tnn动态库编译
```
https://github.com/Tencent/TNN/blob/master/doc/en/user/compile_en.md
```

## onnxruntime动态库编译
```
https://onnxruntime.ai/docs/build/inferencing.html#macos

# 在编译其他架构前需要删掉build目录, 否则会有 `use of undeclared identifier 'cpuinfo_arm_linux_feature2_xxx` 问题, 原因在于不同架构里的pytorch_cpuinfo里面参数不同

# macos
./build.sh --config RelWithDebInfo --build_shared_lib --parallel 8 --cmake_extra_defines CMAKE_OSX_ARCHITECTURES=x86_64
```


## tengine-lite动态库编译
```
git clone -b tengine-lite https://github.com/OAID/Tengine.git  Tengine

mkdir build
cd build
# 关闭online report, 否则会报错找不到 linux/if.h 头文件
cmake -DTENGINE_ONLINE_REPORT=OFF ..
make -j8
make install
```

## openvino动态库编译
```

```

## TVM动态库编译
```

```

## whisper.cpp(ggml)动态库编译
```shell

mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON ..
make
```


## sherpa-onnx 动态库编译
```shell

## 编译构建onnxruntime
git clone https://github.com/microsoft/onnxruntime.git -b v1.17.0
cd onnxruntime
./build.sh --config RelWithDebInfo --build_shared_lib --parallel 4 --cmake_extra_defines CMAKE_OSX_ARCHITECTURES=x86_64 --use_coreml --cmake_extra_defines FETCHCONTENT_TRY_FIND_PACKAGE_MODE=NEVER
sherpa_onnx_lib=$PWD/build/MacOS/RelWithDebInfo

## 下载sherpa-onnx所需的c头文件
cd ..
wget -q https://github.com/csukuangfj/onnxruntime-libs/releases/download/v1.17.0/onnxruntime-osx-x86_64-static_lib-1.17.0.zip
unzip onnxruntime-osx-x86_64-static_lib-1.17.0.zip
sherpa_onnx_header=$PWD/onnxruntime-osx-x86_64-static_lib-1.17.0/include

## 编译sherpa-onnx
cd ..
git clone https://github.com/k2-fsa/sherpa-onnx.git -b v1.9.10
cd sherpa-onnx
#set -ex
dir=$PWD/build-macos-x86_64
mkdir -p $dir
cd $dir
export SHERPA_ONNXRUNTIME_LIB_DIR=${sherpa_onnx_lib}
export SHERPA_ONNXRUNTIME_INCLUDE_DIR=${sherpa_onnx_header}

echo "SHERPA_ONNXRUNTIME_LIB_DIR: $SHERPA_ONNXRUNTIME_LIB_DIR"
echo "SHERPA_ONNXRUNTIME_INCLUDE_DIR $SHERPA_ONNXRUNTIME_INCLUDE_DIR"

cmake -DBUILD_PIPER_PHONMIZE_EXE=OFF \
    -DBUILD_PIPER_PHONMIZE_TESTS=OFF \
    -DBUILD_ESPEAK_NG_EXE=OFF \
    -DBUILD_ESPEAK_NG_TESTS=OFF \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_SHARED_LIBS=ON \
    -DSHERPA_ONNX_ENABLE_PYTHON=OFF \
    -DSHERPA_ONNX_ENABLE_TESTS=OFF \
    -DSHERPA_ONNX_ENABLE_CHECK=OFF \
    -DSHERPA_ONNX_ENABLE_PORTAUDIO=OFF \
    -DSHERPA_ONNX_ENABLE_JNI=OFF \
    -DSHERPA_ONNX_ENABLE_C_API=ON \
    -DCMAKE_INSTALL_PREFIX=./install  ..

# make VERBOSE=1 -j4
make -j4
make install/strip
```
