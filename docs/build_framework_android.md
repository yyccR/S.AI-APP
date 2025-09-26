# 推理框架依赖库编译(android)
- android ndk 版本21.4.7075529

## ncnn动态库
```
nihui版本: https://github.com/nihui/ncnn/releases/tag/20221122
tecent版本: https://github.com/Tencent/ncnn/releases
```

## paddle-lite动态库
``` 
android:  inference_lite_lib.android.armv8.gcc.c++_shared.with_extra.with_cv
ios: inference_lite_lib.ios.armv8.gcc.c++_shared.with_extra.with_cv

mac下编译示例:
1. 下载 3.10 <= cmake <=3.19.3, ![https://cmake.org/files/](https://cmake.org/files/)
   export PATH=/path/to/cmake/bin/:$PATH
2. android studio下载任意版本ndk,
   export ANDROID_NDK=/path/to/ndk/version
   export NDK_ROOT=/path/to/ndk/version
3. git clone https://github.com/PaddlePaddle/Paddle-Lite.git -b 分支版本
4. 修改Paddle-Lite/lite/tools/build_android.sh
   ANDROID_API_LEVEL=30
   ANDROID_STL=c++_shared
   WITH_EXTRA=ON
   WITH_JAVA=OFF
   WITH_CV=ON
   WITH_LOG=OFF
5. ./lite/tools/build_android.sh

```

## tflie动态库

0. 为什么要使用tflite c++库
```
1. 自由, tf事实上有提供Android或者IOS依赖库,但是对于其中有些功能没有很好封装, 比如int8模型格式获取对应的zero/scale参数比较麻烦, c++库可以直接从模型文件拿到

2. 速度, 虽然有人测过java端其实和c++端对模型的推理调用速度上没有太大差别,但是如果流数据是: dart->java->c++ 明显会比dart->c++多了不必要的开销

3. 迁移, 如果使用Java端依赖, 则IOS需要重新实现一遍, 耗时耗力, c++小改则Android/ios可以一起用  
```


1. `clone`对应版本的tensorflow
```
git clone -b v2.10.0 https://github.com/tensorflow/tensorflow.git
下载对应bazel版本: https://github.com/bazelbuild/bazel/tags
例如: bazel-6.5.0-darwin-x86_64
chmod +x bazel-6.5.0-darwin-x86_64

修改 /Users/yang/opt/tensorflow/tensorflow/lite/CMakeLists.txt

option(TFLITE_ENABLE_INSTALL "Enable install rule" OFF)
option(TFLITE_ENABLE_RUY "Enable experimental RUY integration" OFF)
option(TFLITE_ENABLE_RESOURCE "Enable experimental support for resources" ON)
option(TFLITE_ENABLE_NNAPI "Enable NNAPI (Android only)." ON)
cmake_dependent_option(TFLITE_ENABLE_NNAPI_VERBOSE_VALIDATION "Enable NNAPI verbose validation." OFF
                       "TFLITE_ENABLE_NNAPI" ON)
option(TFLITE_ENABLE_MMAP "Enable MMAP (unsupported on Windows)" ON)
option(TFLITE_ENABLE_GPU "Enable GPU" ON)
option(TFLITE_ENABLE_METAL "Enable Metal delegate (iOS only)" OFF)
option(TFLITE_ENABLE_XNNPACK "Enable XNNPACK backend" ON)
option(TFLITE_ENABLE_EXTERNAL_DELEGATE "Enable External Delegate backend" ON)

```

2. `configure`配置
```
cd ./tensorflow/
./configure

Do you wish to build TensorFlow with ROCm support? [y/N]: n

Do you wish to build TensorFlow with CUDA support? [y/N]: n

Do you wish to download a fresh release of clang? (Experimental) [y/N]: n

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -Wno-sign-compare]: 

Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: y

Please specify the home path of the Android NDK to use. [Default is /Users/yang/library/Android/Sdk/ndk-bundle]: /Users/yang/Library/Android/sdk/ndk/21.4.7075529

Please specify the (min) Android NDK API level to use. [Available levels: ['16', '17', '18', '19', '21', '22', '23', '24', '26', '27', '28', '29', '30']] [Default is 21]: 30

Please specify the home path of the Android SDK to use. [Default is /Users/yang/library/Android/Sdk]: 

Please specify the Android SDK API level to use. [Available levels: ['11', '24', '28', '29', '30', '31', '33']] [Default is 33]: 33

Please specify an Android build tools version to use. [Available versions: ['29.0.2', '30.0.2', '30.0.3', '32.0.0']] [Default is 32.0.0]: 

Do you wish to build TensorFlow with iOS support? [y/N]: n
```   

3. 编译`tflite`动态库, 默认已经支持`NNAPI`, 首选使用`bazel`编译tf, 次选`cmake`, 在转换模型的时候如果用到`tf.lite.OpsSet.SELECT_TF_OPS`, `bazel`编译将支持tf本身部分算子, `cmake`则不支持这部分
```
# arm64-v8a
bazel build -c opt --config=android_arm64 --define tflite_with_nnapi=true --define=xnn_enable_arm_i8mm=false --cxxopt="-std=c++17" //tensorflow/lite:libtensorflowlite.so

# armeabi-v7a
bazel build -c opt --config=android_arm --define=xnn_enable_arm_i8mm=false --cxxopt="-std=c++17" //tensorflow/lite:libtensorflowlite.so
```

4. 编译`gpu delegate`动态库
```
# arm64-v8a
bazel build -c opt --config android_arm64 --copt -Os --copt -DTFLITE_GPU_BINARY_RELEASE --define=xnn_enable_arm_i8mm=false --linkopt -s --strip always //tensorflow/lite/delegates/gpu:libtensorflowlite_gpu_delegate.so

# armeabi-v7a
bazel build -c opt --config android_arm --copt -Os --copt -DTFLITE_GPU_BINARY_RELEASE --define=xnn_enable_arm_i8mm=false --linkopt -s --strip always //tensorflow/lite/delegates/gpu:libtensorflowlite_gpu_delegate.so
```

## mnn动态库编译
```
mac下编译示例:
1. 下载 cmake 最新版, ![https://cmake.org/files/](https://cmake.org/files/)
2. export PATH=/path/to/cmake/bin/:$PATH
3. git clone https://github.com/alibaba/MNN.git -b 2.8.2
4. 修改 MNN/project/android/build_64.sh
   cmake ../../../ \
    -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/26.2.11394342/build/cmake/android.toolchain.cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DANDROID_ABI="arm64-v8a" \
    -DANDROID_STL=c++_shared \
    -DMNN_USE_LOGCAT=false \
    -DMNN_BUILD_BENCHMARK=ON \
    -DMNN_USE_SSE=OFF \
    -DMNN_SUPPORT_BF16=OFF \
    -DMNN_BUILD_TEST=ON \
    -DMNN_OPENCL=ON \
    -DMNN_NNAPI=ON \
    -DMNN_ARM82=ON \
    -DMNN_SUPPORT_BF16=ON \
    -DANDROID_NATIVE_API_LEVEL=android-30  \
    -DMNN_BUILD_FOR_ANDROID_COMMAND=true \
    -DNATIVE_LIBRARY_OUTPUT=. -DNATIVE_INCLUDE_OUTPUT=. $1 $2 $3
   make -j4
5. 构建arm64-v8a, 
   cd MNN/project/android && mkdir build_arm64 && cd build_arm64 && ../build_64.sh

6. 修改 MNN/project/android/build_32.sh
   cmake ../../../ \
    -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/26.2.11394342/build/cmake/android.toolchain.cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DANDROID_ABI="armeabi-v7a" \
    -DANDROID_STL=c++_shared \
    -DMNN_USE_LOGCAT=false \
    -DMNN_BUILD_BENCHMARK=ON \
    -DMNN_USE_SSE=OFF \
    -DMNN_SUPPORT_BF16=OFF \
    -DMNN_BUILD_TEST=ON \
    -DMNN_OPENCL=ON \
    -DMNN_NNAPI=ON \
    -DMNN_ARM82=ON \
    -DMNN_SUPPORT_BF16=ON \
    -DANDROID_NATIVE_API_LEVEL=android-30  \
    -DMNN_BUILD_FOR_ANDROID_COMMAND=true \
    -DNATIVE_LIBRARY_OUTPUT=. -DNATIVE_INCLUDE_OUTPUT=. $1 $2 $3
   make -j4
5. 构建armeabi-v7a, 
   cd MNN/project/android && mkdir build_arm32 && cd build_arm32 && ../build_32.sh

```

## tnn动态库编译
```
https://github.com/Tencent/TNN/blob/master/doc/en/user/compile_en.md
```

## onnxruntime动态库编译
```
https://onnxruntime.ai/docs/build/android.html

# 下面每个架构编译完拿build/Android/Release/libonnxruntime.so即可, 在编译其他架构前需要删掉build目录, 否则会有 `use of undeclared identifier 'cpuinfo_arm_linux_feature2_xxx` 问题, 原因在于不同架构里的pytorch_cpuinfo里面参数不同

mac下编译示例:
1. 下载 cmake 最新版, ![https://cmake.org/files/](https://cmake.org/files/)
2. export PATH=/path/to/cmake/bin/:$PATH
3. git clone https://github.com/microsoft/onnxruntime.git -b v1.17.0
4. 构建arm64-v8a, 只能用于ort格式的模型
   ./build.sh --android --config Release --parallel 8 --build_shared_lib --minimal_build --minimal_build extended --android_sdk_path /Users/yang/Library/Android/sdk/ --android_ndk_path /Users/yang/Library/Android/sdk/ndk/26.2.11394342/ --android_abi arm64-v8a --android_api 30 --android_cpp_shared --use_nnapi
5. 构建arm64-v8a, 可以用于ort格式的模型,也可以支持onnx格式的模型
   ./build.sh --android --config Release --parallel 8 --build_shared_lib --android_sdk_path /Users/yang/Library/Android/sdk/ --android_ndk_path /Users/yang/Library/Android/sdk/ndk/26.2.11394342/ --android_abi arm64-v8a --android_api 30 --android_cpp_shared --use_nnapi
5. 构建armeabi-v7a, 只能用于ort格式的模型
   ./build.sh --android --config Release --parallel 8 --build_shared_lib --minimal_build --minimal_build extended --android_sdk_path /Users/yang/Library/Android/sdk/ --android_ndk_path /Users/yang/Library/Android/sdk/ndk/26.2.11394342/ --android_abi armeabi-v7a --android_api 30 --android_cpp_shared --use_nnapi
6. 构建armeabi-v7a, 可以用于ort格式的模型,也可以支持onnx格式的模型
   ./build.sh --android --config Release --parallel 8 --build_shared_lib --android_sdk_path /Users/yang/Library/Android/sdk/ --android_ndk_path /Users/yang/Library/Android/sdk/ndk/26.2.11394342/ --android_abi armeabi-v7a --android_api 30 --android_cpp_shared --use_nnapi
7. 构建macos
   ./build.sh --config RelWithDebInfo --build_shared_lib --parallel 8 --cmake_extra_defines CMAKE_OSX_ARCHITECTURES=x86_64
```

## tengine-lite动态库编译
```
https://github.com/OAID/Tengine/blob/tengine-lite/doc/docs_zh/source_compile/compile_android.md

git clone -b tengine-lite https://github.com/OAID/Tengine.git Tengine
# arm64-v8a
cd Tengine
mkdir build-android-aarch64
cd build-android-aarch64
cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI="arm64-v8a" -DANDROID_ARM_NEON=ON -DANDROID_PLATFORM=android-21 ..
make -j$(nproc)
make install

# armeabi-v7a
cd Tengine
mkdir build-android-armv7
cd build-android-armv7
cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI="armeabi-v7a" -DANDROID_ARM_NEON=ON -DANDROID_PLATFORM=android-19 ..
make -j$(nproc)
make install

```

## openvino动态库编译
```
https://github.com/openvinotoolkit/openvino_contrib/wiki/How-to-build-ARM-CPU-plugin

mkdir openvino_android
cd openvino_android
export WORK_DIR=`pwd`

git clone --recurse -b 2022.3.0 https://github.com/openvinotoolkit/openvino.git
git clone --recurse https://github.com/openvinotoolkit/openvino_contrib.git

wget https://dl.google.com/android/repository/commandlinetools-linux-7583922_latest.zip
unzip commandlinetools-linux-7583922_latest.zip
yes | ./cmdline-tools/bin/sdkmanager --sdk_root="$WORK_DIR/android-tools" --licenses
./cmdline-tools/bin/sdkmanager --sdk_root="$WORK_DIR/android-tools" --install "ndk-bundle"

mkdir openvino_android
mkdir "$WORK_DIR/openvino_android/openvino_build"
mkdir "$WORK_DIR/openvino_android/openvino_install"
      
cmake -GNinja \
      -DCMAKE_BUILD_TYPE=Release \
      -DTHREADING=SEQ \
      -DIE_EXTRA_MODULES="$WORK_DIR/openvino_contrib/modules" \
      -DCMAKE_TOOLCHAIN_FILE="/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake" \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_PLATFORM=31 \
      -DENABLE_SAMPLES=OFF \
      -DENABLE_OPENCV=OFF \
      -DENABLE_CLDNN=OFF \
      -DENABLE_VPU=OFF \
      -DENABLE_GNA=OFF \
      -DENABLE_MYRIAD=OFF \
      -DENABLE_TESTS=OFF \
      -DENABLE_GAPI_TESTS=OFF \
      -DENABLE_INTEL_MYRIAD=OFF \
      -DENABLE_INTEL_MYRIAD_COMMON=OFF \
      -DBUILD_java_api=OFF \
      -DARM_COMPUTE_SCONS_JOBS=$(nproc) \
      -DENABLE_BEH_TESTS=OFF \
      -DCMAKE_INSTALL_PREFIX="$WORK_DIR/openvino_android/openvino_install" \
      -B "$WORK_DIR/openvino_android/openvino_build" -S "$WORK_DIR/openvino"

ninja -C "$WORK_DIR/openvino_android/openvino_build"
ninja -C "$WORK_DIR/openvino_android/openvino_build" install



/Users/yang/Library/Android/sdk/ndk/21.4.7075529/toolchains/aarch64-linux-android-4.9/prebuilt/darwin-x86_64/aarch64-linux-android/bin/strip /Users/yang/opt/openvino_android/openvino_android/openvino_install_v8a/runtime/lib/aarch64/*.so

/Users/yang/Library/Android/sdk/ndk/21.4.7075529/toolchains/arm-linux-androideabi-4.9/prebuilt/darwin-x86_64/bin/arm-linux-androideabi-strip /Users/yang/opt/openvino_android/openvino_android/openvino_install/runtime/lib/armv7-a/*.so

cmake -DCMAKE_BUILD_TYPE=Release \
      -DTHREADING=SEQ \
      -DIE_EXTRA_MODULES="/Users/yang/opt/openvino_android/openvino_contrib/modules" \
      -DCMAKE_TOOLCHAIN_FILE="/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake" \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_PLATFORM=30 \
      -DENABLE_SAMPLES=OFF \
      -DENABLE_OPENCV=OFF \
      -DENABLE_CLDNN=OFF \
      -DENABLE_VPU=OFF \
      -DENABLE_GNA=OFF \
      -DENABLE_MYRIAD=OFF \
      -DENABLE_TESTS=OFF \
      -DENABLE_GAPI_TESTS=OFF \
      -DENABLE_INTEL_MYRIAD=OFF \
      -DENABLE_INTEL_MYRIAD_COMMON=OFF \
      -DBUILD_java_api=ON \
      -DARM_COMPUTE_SCONS_JOBS=$(nproc) \
      -DENABLE_BEH_TESTS=OFF ..
      
cmake --build ./build_arm64_v8a -j8

cd ..
cmake --install build_arm64_v8a --prefix install_arm64_v8a

/Users/yang/Library/Android/sdk/ndk/21.4.7075529/toolchains/aarch64-linux-android-4.9/prebuilt/darwin-x86_64/aarch64-linux-android/bin/strip /Users/yang/opt/openvino_android/openvino/install_arm64_v8a/runtime/lib/aarch64/*.so

/Users/yang/Library/Android/sdk/ndk/21.4.7075529/toolchains/arm-linux-androideabi-4.9/prebuilt/darwin-x86_64/bin/arm-linux-androideabi-strip /Users/yang/opt/openvino_android/openvino_android/openvino_install/runtime/lib/armv7-a/*.so

cmake -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake -DANDROID_NDK=/Users/yang/Library/Android/sdk/ndk/21.4.7075529 -DCMAKE_BUILD_TYPE=Release  -DANDROID_ABI="arm64-v8a" ..
make -j8
```

## openvino 静态库构建
```

cmake -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_SHARED_LIBS=OFF \
      -DTHREADING=SEQ \
      -DIE_EXTRA_MODULES="/Users/yang/opt/openvino_android/openvino_contrib/modules" \
      -DCMAKE_TOOLCHAIN_FILE="/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake" \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_PLATFORM=30 \
      -DENABLE_SAMPLES=OFF \
      -DENABLE_OPENCV=OFF \
      -DENABLE_CLDNN=OFF \
      -DENABLE_VPU=OFF \
      -DENABLE_GNA=OFF \
      -DENABLE_MYRIAD=OFF \
      -DENABLE_TESTS=OFF \
      -DENABLE_GAPI_TESTS=OFF \
      -DENABLE_INTEL_MYRIAD=OFF \
      -DENABLE_INTEL_MYRIAD_COMMON=OFF \
      -DBUILD_java_api=ON \
      -DARM_COMPUTE_SCONS_JOBS=$(nproc) \
      -DENABLE_BEH_TESTS=OFF ..
      
      
cmake -GNinja \
      -DCMAKE_BUILD_TYPE=Release \
      -DTHREADING=SEQ \
      -DIE_EXTRA_MODULES="/Users/yang/opt/openvino_android/openvino_contrib_2022.3/modules" \
      -DCMAKE_TOOLCHAIN_FILE="/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake" \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_PLATFORM=30 \
      -DENABLE_SAMPLES=OFF \
      -DENABLE_OPENCV=OFF \
      -DENABLE_CLDNN=OFF \
      -DENABLE_VPU=OFF \
      -DENABLE_GNA=OFF \
      -DENABLE_MYRIAD=OFF \
      -DENABLE_TESTS=OFF \
      -DENABLE_GAPI_TESTS=OFF \
      -DENABLE_INTEL_MYRIAD=OFF \
      -DENABLE_INTEL_MYRIAD_COMMON=OFF \
      -DBUILD_java_api=ON \
      -DARM_COMPUTE_SCONS_JOBS=$(nproc) \
      -DENABLE_BEH_TESTS=OFF \
      -DCMAKE_INSTALL_PREFIX="/Users/yang/opt/openvino_android/openvino_install_arm64_v8a" \
      -B "/Users/yang/opt/openvino_android/openvino_build_arm64_v8a" -S "/Users/yang/opt/openvino_android/openvino_2022.03.0"

ninja -C "/Users/yang/opt/openvino_android/openvino_build_arm64_v8a"
ninja -C "/Users/yang/opt/openvino_android/openvino_build_arm64_v8a" install

```


## TVM动态库编译
```

# arm64-v8a
cmake ../ \
      -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DANDROID_ABI="arm64-v8a" \
      -DANDROID_STL=c++_static \
      -DCMAKE_BUILD_TYPE=Release \
      -DANDROID_NATIVE_API_LEVEL=android-30  \
      -DANDROID_TOOLCHAIN=clang++

# armeabi-v7a
cmake ../ \
      -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DANDROID_ABI="armeabi-v7a" \
      -DANDROID_STL=c++_static \
      -DCMAKE_BUILD_TYPE=Release \
      -DANDROID_NATIVE_API_LEVEL=android-30  \
      -DANDROID_TOOLCHAIN=clang++


```


## whisper.cpp(ggml) 动态库编译
```shell
# arm64-v8a
cmake -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_SHARED_LIBS=ON \
      -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_NATIVE_API_LEVEL=android-30  \
      -DCMAKE_C_FLAGS="-march=armv8.2-a+fp16 -O3 -ffunction-sections -fdata-sections" \
      -DCMAKE_CXX_FLAGS="-march=armv8.2-a+fp16 -O3 -ffunction-sections -fdata-sections"\
      -DCMAKE_EXE_LINKER_FLAGS="-Wl,--gc-sections -Wl,--exclude-libs,ALL -Wl,-flto" \
      ..
make     
      
cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=arm64-v8a \
      -DANDROID_NATIVE_API_LEVEL=android-30  \
      ..
cmake --build .     

# armeabi-v7a
cmake -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_SHARED_LIBS=ON \
      -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=armeabi-v7a \
      -DANDROID_NATIVE_API_LEVEL=android-30  \
      -DCMAKE_C_FLAGS="-O3 -ffunction-sections -fdata-sections" \
      -DCMAKE_CXX_FLAGS="-O3 -ffunction-sections -fdata-sections"\
      ..
make

cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_TOOLCHAIN_FILE=/Users/yang/Library/Android/sdk/ndk/21.4.7075529/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=armeabi-v7a \
      -DANDROID_NATIVE_API_LEVEL=android-30  \
      ..
cmake --build .      

```


## sherpa(ncnn) 动态库编译
```shell
0. git clone https://github.com/k2-fsa/sherpa-ncnn.git

# arm64-v8a
1. 如果用vulkan, 打开 build-android-arm64-v8a-with-vulkan.sh 否则打开 build-android-arm64-v8a.sh
2. 修改 ANDROID_NDK 为自己的ndk目录, 例如 /Users/yang/Library/Android/sdk/ndk/21.4.7075529
3. (如果用vulkan) 修改 VULKAN_SDK 为自己的ndk目录, 例如 /Users/yang/opt/vulkan/vulkansdk-macos-1.2.189.0/macOS
4. 修改cmake参数 -DSHERPA_NCNN_ENABLE_C_API=ON 表示采用c-api
5. 修改cmake参数 -DANDROID_PLATFORM=android-30 原本Android版本太低
6. 执行 ./build-android-arm64-v8a-with-vulkan.sh 或者 ./build-android-arm64-v8a.sh

# armeabi-v7a
1. 打开 build-android-armv7-eabi.sh
2. 修改 ANDROID_NDK 为自己的ndk目录, 例如 /Users/yang/Library/Android/sdk/ndk/21.4.7075529
3. 修改cmake参数 -DSHERPA_NCNN_ENABLE_C_API=ON 表示采用c-api
4. 修改cmake参数 -DANDROID_PLATFORM=android-30 原本Android版本太低
5. 执行 ./build-android-armv7-eabi.sh
```