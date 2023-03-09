---
layout: jupyter
title: 음성 처리(STT) - wav2vec(Huggingface)
published: true
date: 2023-02-01
description: wav2vec(Huggingface)
comments: true
categories:
 - STT/TTS
tags:
 - python
 - STT/TTS
toc: true
toc_sticky: true
toc_label: wav2vec(Huggingface)
use_math: true
---
---
# wav2vec(Huggingface)

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
!pip install transformers datasets jiwer
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
from transformers import Wav2Vec2ForCTC, Wav2Vec2Processor
from datasets import load_dataset
import soundfile as sf
import torch
from jiwer import wer
```

</div>

wer: word error rate  
infer중에 몇번 틀렸는지, 낮을 수록 좋다.  
acc = True/Total

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
processor = Wav2Vec2Processor.from_pretrained("kresnik/wav2vec2-large-xlsr-korean")
model = Wav2Vec2ForCTC.from_pretrained("kresnik/wav2vec2-large-xlsr-korean").to("cuda")
ds = load_dataset("kresnik/zeroth_korean", "clean")
```

</div>

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
test_ds = ds['test']
```

</div>

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
type(test_ds)
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>




{:.output_data_text}

```
datasets.arrow_dataset.Dataset
```



<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
def map_to_array(batch):
    speech, sr = sf.read(batch['file'])
    batch['speech'] = speech
    return batch
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
test_ds = test_ds.map(map_to_array)
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>


{:.output_data_text}

```
  0%|          | 0/457 [00:00<?, ?ex/s]
```


<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
def map_to_pred(batch):
    inputs = processor(batch["speech"], sampling_rate=16000, return_tensors="pt", padding="longest")
    input_values = inputs.input_values.to("cuda") # melspectrogram
    # gradient(경사하강법)
    with torch.no_grad():
        logits = model(input_values).logits
    predicted_ids = torch.argmax(logits, dim=-1)
    transcription = processor.batch_decode(predicted_ids)
    batch["transcription"] = transcription
    return batch

result = test_ds.map(map_to_pred, batched=True, batch_size=16)
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>


{:.output_data_text}

```
  0%|          | 0/29 [00:00<?, ?ba/s]
```


<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
print("WER", wer(result["text"], result["transcription"])) # 원래 정답과 예상치 비교(에러 확률; 낮을수록 좋음)
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>

{:.output_stream}

```
WER 0.04773377503388044

```
