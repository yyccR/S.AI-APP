# 推理框架依赖库编译(ios)

## ncnn动态库
```
nihui版本: https://github.com/nihui/ncnn/releases/tag/20221122
tecent版本: https://github.com/Tencent/ncnn/releases
```

## paddle-lite动态库
``` 
```

## tflie动态库
```
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
https://onnxruntime.ai/docs/build/ios.html

#  在编译其他架构前需要删掉build目录, 否则会有 `use of undeclared identifier 'cpuinfo_arm_linux_feature2_xxx` 问题, 原因在于不同架构里的pytorch_cpuinfo里面参数不同
```