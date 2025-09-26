
## whisper.tflite

> 依赖

```shell
pip3 install tensorflow<=2.12.0
pip3 install transformers
pip3 install datasets
```

> 用的transformers库下的whisper(en)转tflite


```python
# 下载whisper模型, 模型列表可在 https://github.com/openai/whisper#available-models-and-languages 查看, 这里从huggingface下载
import tensorflow as tf
from transformers import WhisperProcessor, TFWhisperForConditionalGeneration
from datasets import load_dataset

processor = WhisperProcessor.from_pretrained("openai/whisper-tiny.en")
model = TFWhisperForConditionalGeneration.from_pretrained("openai/whisper-tiny.en")

ds = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation")

inputs = processor(ds[0]["audio"]["array"], return_tensors="tf")
input_features = inputs.input_features

# 简单测试模型效果
generated_ids = model.generate(input_features)
transcription = processor.batch_decode(generated_ids, skip_special_tokens=True)[0]
transcription
```

```python
# 这里将上面从hf驾照的模型,套多一个generate类
class GenerateModel(tf.Module):
  def __init__(self, model):
    super(GenerateModel, self).__init__()
    self.model = model
  @tf.function(
    # shouldn't need static batch size, but throws exception without it (needs to be fixed)
    input_signature=[
      tf.TensorSpec((1, None, None), tf.float32, name="input_features"), 
    ],
  )
  def serving(self, input_features):
    outputs = self.model.generate(
      input_features,
      max_new_tokens=20, #change as needed
      return_dict_in_generate=True,
    )
    return {"sequences": outputs["sequences"]}

saved_model_dir = '/data/whisper/tf_whisper_saved'
tflite_model_path = '/data/whisper/whisper.tflite'

generate_model = GenerateModel(model=model)
tf.saved_model.save(generate_model, saved_model_dir, signatures={"serving_default": generate_model.serving})

# 转换模型, 动态量化 float32-input, int8-weight
converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir)
converter.target_spec.supported_ops = [
  tf.lite.OpsSet.TFLITE_BUILTINS, # enable TensorFlow Lite ops.
  tf.lite.OpsSet.SELECT_TF_OPS # enable TensorFlow ops.
]
converter.optimizations = [tf.lite.Optimize.DEFAULT]
tflite_model = converter.convert()

# 保存模型
with open(tflite_model_path, 'wb') as f:
    f.write(tflite_model)
```


```python
# tflite加载模型
tflite_model_path = '/data/whisper/whisper_tiny.en.tflite'
interpreter = tf.lite.Interpreter(tflite_model_path)

tflite_generate = interpreter.get_signature_runner()
generated_ids = tflite_generate(input_features=input_features)["sequences"]
transcription = processor.batch_decode(generated_ids, skip_special_tokens=True)[0]
transcription
```



> 用的transformers库下的whisper(多语言)转tflite, 目前转换还是不成功

```python
# 下载whisper模型, 模型列表可在 https://github.com/openai/whisper#available-models-and-languages 查看, 这里从huggingface下载
import tensorflow as tf
from transformers import WhisperProcessor, TFWhisperForConditionalGeneration
from datasets import load_dataset, Audio

processor = WhisperProcessor.from_pretrained("openai/whisper-tiny")
model = TFWhisperForConditionalGeneration.from_pretrained("openai/whisper-tiny")
forced_decoder_ids = processor.get_decoder_prompt_ids(language="chinese", task="transcribe")

ds = load_dataset("mozilla-foundation/common_voice_11_0", "zh-CN", split="validation")
ds = ds.cast_column("audio", Audio(sampling_rate=16_000))

inputs = processor(ds[1]["audio"]["array"], sampling_rate=ds[1]["audio"]["sampling_rate"], return_tensors="tf")
input_features = inputs.input_features

# 简单测试模型效果
generated_ids = model.generate(input_features, forced_decoder_ids=forced_decoder_ids)
transcription = processor.batch_decode(generated_ids, skip_special_tokens=True)[0]
transcription
```

```python
# 这里将上面从hf驾照的模型,套多一个generate类
class GenerateModel(tf.Module):
  def __init__(self, model):
    super(GenerateModel, self).__init__()
    self.model = model
  @tf.function(
    # shouldn't need static batch size, but throws exception without it (needs to be fixed)
    input_signature=[
      tf.TensorSpec((1, 80,3000), tf.float32, name="input_features"), 
    ],
  )
  def serving(self, input_features):
    outputs = self.model.generate(
      input_features,
      forced_decoder_ids=forced_decoder_ids,
      return_dict_in_generate=True,
    )
    return {"sequences": outputs["sequences"]}

saved_model_dir = '/data/whisper/tf_whisper_saved'
tflite_model_path = '/data/whisper/whisper_tiny.tflite'

generate_model = GenerateModel(model=model)
tf.saved_model.save(generate_model, saved_model_dir, signatures={"serving_default": generate_model.serving})

# 转换模型, 动态量化 float32-input, int8-weight
converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir)
converter.target_spec.supported_ops = [
  tf.lite.OpsSet.TFLITE_BUILTINS, # enable TensorFlow Lite ops.
  tf.lite.OpsSet.SELECT_TF_OPS # enable TensorFlow ops.
]
#converter.optimizations = [tf.lite.Optimize.DEFAULT]
tflite_model = converter.convert()

# 保存模型
with open(tflite_model_path, 'wb') as f:
    f.write(tflite_model)
```


```python
# tflite加载模型
tflite_model_path = '/data/whisper/whisper_tiny.tflite'
interpreter = tf.lite.Interpreter(tflite_model_path)

tflite_generate = interpreter.get_signature_runner()
generated_ids = tflite_generate(input_features=input_features)["sequences"]
transcription = processor.batch_decode(generated_ids, skip_special_tokens=True)[0]
transcription
```