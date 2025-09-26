
### 1. TODO:

- [x] 支持`ncnn`格式, 通过`dart->c`
- [x] 支持`tflite`格式, 通过`dart->c` 
- [x] 支持`paddle lite`格式, 通过`dart->c`
- [x] 支持`onnxruntime`格式, 通过`dart->c` 
- [x] 支持`mnn`格式, 通过`dart->c` 
- [x] 支持`tnn`格式, 通过`dart->c` 
- [ ] 支持`tengine lite`格式, 通过`dart->c` 
- [ ] 支持`openvino`格式, 通过`dart->c` 
- [ ] 支持`tvm`格式, 通过`dart->c` 
- [x] 支持`ggml`格式, 通过`dart->c`
- [x] 支持火山云api
- [x] 支持微软云api
- [ ] 支持阿里云api
- [ ] 支持谷歌云api
- [ ] 支持百度云api
- [x] 修复模型启动慢导致UI卡顿问题,改用isolate初始化模型,加上loading动画
- [x] 摄像头流处理+覆盖检测效果
- [x] 加载视频, 检测后展示效果
- [ ] 修复摄像头打开进行流计算，中间直接退出会闪退
- [ ] 视频文件流处理, 帧数据UInt8List转pixel  
- [x] 支持音频流处理,播放
- [ ] 删除file_picker在cache目录下的拷贝文件 
- [ ] 处理ffi调用c++时资源释放太慢导致越来越卡  
- [x] ffi异常捕获, 判断处理写入日志秘籍查看, 避免闪退 
- [ ] ncnn模型use_vulkan_compute=true时会触发闪退
- [x] 检测绘图保存图片时, ui.image.toByteData 耗时过长, flutter issue有提到升级3后才出现
- [x] 算法配置改成模板化, 提高可读性, 减少字符串匹配低级失误
- [ ] homepage页tabar左右切换后, 子页面的上下的滑动的事件响应慢, 需要左右滑动停顿一会才能上下滑动
- [x] 格式化home_my_page模型名字显示
- [x] 支持配置保存数据库, 我的模型页下拉刷新自动展示
- [x] 支持配置页配置完后进入推理页
- [x] **支持工作流, 自定义各种模型组合进行工作**
- [ ] **支持100种模型接口**
- [x] 支持工作流推理实时结果UI展示
- [x] 支持c++写日志, flutter端监听展示
- [x] 支持ai speech to speech推理
- [ ] `我的`页面完善
- [ ] `社区`页面完善
- [ ] `首页`交互完善
- [ ] 加入评估逻辑,加入类似[`paper with code`](https://paperswithcode.com/) sota榜单,支持研究者上传评估排行榜
- [ ] 支持类似[Math Solver](https://mathsolver.microsoft.com/zh)

### 2. 资源文件注意点:
1. 分类页面封面图宽高为(250x200), 保持此比例, 修改此比例需要同步修改 home_classes_page.dart 中container的宽高.


### 3. 推理框架依赖库注意点:
1. `ncnn`用nihui的`https://github.com/nihui/ncnn/releases/tag/20221122`才能在ndk21下跑通
2. `tflite`没有提供c++预编译,只能自己编译,编译用ndk21,不然没有toolchains工具
3. `paddle lite` Android用`armv7/armv8`下的 gcc,c++_shared,with_extra,with_cv,同时在`android/app/build.gradle`添加如下:
```
defaultConfig {
        externalNativeBuild {
            cmake {
                arguments "-DANDROID_STL=c++_shared"
            }
        }
    }
```
4. `onnxruntime`如果要使用nnapi, 需要把`onnxruntime/core/providers/nnapi/nnapi_provider_factory.h`头文件中的`#include onnxruntime_c_api.h`需要修改为`#include "onnxruntime/core/session/onnxruntime_c_api.h"`
5. `onnxruntime`默认是`cppFlags -fexceptions`,所以如果是`-fno-fexceptions`会编译不过,如果没有ncnn依赖库,只需要在`android/app/build.gradle`添加如下:
```
defaultConfig {
        externalNativeBuild {
            cmake {
                cppFlags '-fexceptions'
            }
        }
    }
```
如果项目里面有ncnn依赖库,则只需要修改`ncnn依赖库根目录/{abi}/lib/cmake/ncnn/ncnn.cmake`,将其中:
```
set_target_properties(ncnn PROPERTIES
  INTERFACE_COMPILE_OPTIONS "-fno-rtti;-fno-exceptions"
  ...
)

修改为：

set_target_properties(ncnn PROPERTIES
  INTERFACE_COMPILE_OPTIONS "-fno-rtti;-fexceptions"
  ...
)
```




