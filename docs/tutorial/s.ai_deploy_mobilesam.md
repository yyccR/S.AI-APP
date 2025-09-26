
## 部署MobileSAM

1. clone代码
```shell
git clone https://github.com/ChaoningZhang/MobileSAM.git
```

2. 导出onnx格式
```shell
python3 scripts/export_onnx_model.py --checkpoint ./weights/mobile_sam.pt --model-type vit_t --output ./mobile_sam.onnx
```

