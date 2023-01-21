---
layout: jupyter
title: 자연어 처리 - Hugging Face - pipeline
published: true
date: 2023-01-19
description: Hugging Face - pipeline
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: Hugging Face - pipeline
use_math: true
---
---
# Hugging Face - pipeline

파이프라인은 전처리(preprocessing), 모델로 입력 전달, 후처리(postprocessing)의 3단계를 한번에 실행해주는 간편한 함수이다.  
Hugging face hub에서 원하는 모델을 자동으로 다운받아 캐싱하여 사용한다.

* sentiment-analysis - 감성 분류
* fill-mask(Mask filling) - 텍스트의 공백 채우기
* ner(Named entity recognition) - 개체명 인식
* question-answering - 질의 응답
* summarization - 자동 요약
* translaiton - 기계 번역

## 감성 분류

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
from transformers import pipeline
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
cls = pipeline("sentiment-analysis")

res = cls("I love you")[0]
res
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>

{:.output_stream}

```
No model was supplied, defaulted to distilbert-base-uncased-finetuned-sst-2-english and revision af0f99b (https://huggingface.co/distilbert-base-uncased-finetuned-sst-2-english).
Using a pipeline without specifying a model name and revision in production is not recommended.
All model checkpoint layers were used when initializing TFDistilBertForSequenceClassification.

All the layers of TFDistilBertForSequenceClassification were initialized from the model checkpoint at distilbert-base-uncased-finetuned-sst-2-english.
If your task is similar to the task the model of the checkpoint was trained on, you can already use TFDistilBertForSequenceClassification for predictions without further training.

```




{:.output_data_text}

```
{'label': 'POSITIVE', 'score': 0.9998656511306763}
```



## paraphrase
유사어

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
from transformers import AutoTokenizer, TFAutoModelForSequenceClassification
import tensorflow as tf
```

</div>

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
tok = AutoTokenizer.from_pretrained('bert-base-cased-finetuned-mrpc')
model = TFAutoModelForSequenceClassification.from_pretrained('bert-base-cased-finetuned-mrpc')

classes = ['no paraphrase', 'paraphrase']

test1 = 'My home is in Seoul city'
test2 = 'I live in Seoul'
test3 = 'I love yasick'

paraphrase = tok(test1, test2, return_tensors='tf')
no_paraphrase = tok(test1, test3, return_tensors='tf')

paraphrase_cls = model(paraphrase).logits
no_paraphrase_cls = model(no_paraphrase).logits

paraphrase_res = tf.nn.softmax(paraphrase_cls, axis=1).numpy()[0]
no_paraphrase_res = tf.nn.softmax(no_paraphrase_cls, axis=1).numpy()[0]

for i in range(2):
    print(f'{classes[i]}: {paraphrase_res[i]*100}')
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
Some layers from the model checkpoint at bert-base-cased-finetuned-mrpc were not used when initializing TFBertForSequenceClassification: ['dropout_183']
- This IS expected if you are initializing TFBertForSequenceClassification from the checkpoint of a model trained on another task or with another architecture (e.g. initializing a BertForSequenceClassification model from a BertForPreTraining model).
- This IS NOT expected if you are initializing TFBertForSequenceClassification from the checkpoint of a model that you expect to be exactly identical (initializing a BertForSequenceClassification model from a BertForSequenceClassification model).
All the layers of TFBertForSequenceClassification were initialized from the model checkpoint at bert-base-cased-finetuned-mrpc.
If your task is similar to the task the model of the checkpoint was trained on, you can already use TFBertForSequenceClassification for predictions without further training.

```

{:.output_stream}

```
no paraphrase: 9.993244707584381
paraphrase: 90.00675678253174

```

# Reference

* [hugging face](https://huggingface.co/)
