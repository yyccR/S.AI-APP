
## paddle lite ocr
```shell
https://github.com/Mushroomcat9998/PaddleOCR/blob/main/deploy/lite/readme_en.md
```

> 下载 paddle 模型

```shell
# det 模型
https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_det_infer.tar

# cls 模型
https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_mobile_v2.0_cls_infer.tar

# rec 模型
https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_rec_infer.tar
```

> 安装依赖

```shell
pip3 install paddle_lite==2.12
```

> 转换成paddle-lite格式 arm架构

```shell
paddle_lite_opt --model_file=./ch_PP-OCRv4_det_infer/inference.pdmodel --param_file=./ch_PP-OCRv4_det_infer/inference.pdiparams --optimize_out=./ch_PP-OCRv4_det_opt --valid_targets=arm --optimize_out_type=naive_buffer
paddle_lite_opt --model_file=./ch_PP-OCRv4_rec_infer/inference.pdmodel --param_file=./ch_PP-OCRv4_rec_infer/inference.pdiparams --optimize_out=./ch_PP-OCRv4_rec_opt --valid_targets=arm --optimize_out_type=naive_buffer
paddle_lite_opt --model_file=./ch_ppocr_mobile_v2.0_cls_infer/inference.pdmodel --param_file=./ch_ppocr_mobile_v2.0_cls_infer/inference.pdiparams --optimize_out=./ch_ppocr_mobile_v2.0_cls_opt --valid_targets=arm --optimize_out_type=naive_buffer
```

> 转换成paddle-lite格式 x86架构

```shell
paddle_lite_opt --model_file=./ch_PP-OCRv4_det_infer/inference.pdmodel --param_file=./ch_PP-OCRv4_det_infer/inference.pdiparams --optimize_out=./ch_PP-OCRv4_det_opt --valid_targets=x86 --optimize_out_type=naive_buffer
paddle_lite_opt --model_file=./ch_PP-OCRv4_rec_infer/inference.pdmodel --param_file=./ch_PP-OCRv4_rec_infer/inference.pdiparams --optimize_out=./ch_PP-OCRv4_rec_opt --valid_targets=x86 --optimize_out_type=naive_buffer
paddle_lite_opt --model_file=./ch_ppocr_mobile_v2.0_cls_infer/inference.pdmodel --param_file=./ch_ppocr_mobile_v2.0_cls_infer/inference.pdiparams --optimize_out=./ch_ppocr_mobile_v2.0_cls_opt --valid_targets=x86 --optimize_out_type=naive_buffer
```
