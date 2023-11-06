---
layout: post
title: "SentencePiece: A Simple and Language Independent Subword Tokenizer and Detokenizer for Neural Text Processing"
category: "NLP"
date: 2023-11-06
---
 <br>

SentencePiece는 **pre-tokenization 없이** 원시 데이터에 바로 적용할 수 있는 tokenization 기법이다. 원래는 subword tokenizer를 적용하기 위해서 먼저 input 데이터에 pre-tokenization을 했어야 했다. 여기서 pre-tokenization이란 텍스트를 공백이나 구두점 기준으로 분리하는 것이다. 하지만 SentencePiece는 pre-tokenization 과정이 필요 없기 때문에 **어떤 언어에도 적용할 수 있다**는 이점이 있다. 그리고 SentencePiece의 모델 파일은 **자체적으로 완결성**을 가지고 있어서 텍스트의 normalization과 subword 분할 과정을 완벽히 재연할 수 있다. 이로 인해서 SentencePiece는 안정적이고 일관된 텍스트 처리 도구가 된다. 뿐만 아니라 SentencePiece는 tokenization 기능 뿐만 아니라 텍스트를 직접 id sequence로 변환하는 기능도 한다.

<br>
<br>


**Four Components of SentencePiece**

1. Normalizer: 데이터를 표준적인 형태로 변환한다. 예를 들어 'é'라는 문자를 'e’로 변경한다.
2. Trainer: 위에서 normalize된 텍스트를 사용하여 subword로 분할하는 모델을 훈련시킨다. BPE나 Unigram과 같은 subword 분할 모델은 매개변수로 선택된다.
3. Encoder: Normalizer를 실행하고 Trainer로 텍스트를 subword로 tokenization한다.
4. Decoder: Encoder와 반대로 subword sequence를 받아서 원래의 정규화된 텍스트로 재구성한다.

<br>
<br>


**Lossless Tokenization**

일반적으로 언어 의존적으로 tokenization을 하면 원시 데이터와 tokenization된 형태 사이에 완벽한 상호 변환이 불가능하다. 

![Untitled](/assets/SentencePiece%20A%20Simple%20and%20Language%20Independent%20Su%206d7bbff526ba441ba4c64b18b039759f/Untitled.png)

여기서 tokenization된 형태에는 공백이 없기 때문에 원시 데이터와 똑같이 복원될 수 없다. 원본 텍스트로 정확하게 되돌리기 위해서는 언어 별로 특정 처리가 필요합니다. 예를 들어 일본어와 같은 경우 공백이 없기 때문에 원시 데이터로 복원할 때 공백의 부재는 중요한 고려 사항이 아니다.

![Untitled](/assets/SentencePiece%20A%20Simple%20and%20Language%20Independent%20Su%206d7bbff526ba441ba4c64b18b039759f/Untitled%201.png)

SentencePiece는 Decoder와 Encoder로 이러한 문제를 해결했다. SentencePiece에서는 normalize된 텍스트를 재생산하기 위한 모든 정보가 Encoder의 출력 값에 보존된다. SentencePiece는 공백도 일반 기호 _(U+2581)로 처리한다.

![Untitled](/assets/SentencePiece%20A%20Simple%20and%20Language%20Independent%20Su%206d7bbff526ba441ba4c64b18b039759f/Untitled%202.png)

Tokenization된 결과를 보면 공백이 포함되어 있어서 원본 데이터로 복원할 때 어떠한 모호함도 없다.

<br>
<br>


**Efficient Subword Training and Segmentation**

Subword 분할을 하기 전에 pre-tokenization을 한 이유는 효율성 때문이었다. 그러나 공백을 사용하지 않는 언어에서는 이 방식이 적용되기 어려울 수 있다. 또한 pre-tokenization은 텍스트의 원형을 보존하는 lossless tokenization을 어렵게 만들 수 있다. 이러한 문제를 해결하기 위해 SentencePiece는 pre-tokenization 과정을 생략했다. 대신에 효율성을 유지하기 위해 다른 기술을 사용한다. 예를 들어 training 속도를 빠르게 하기 위해 SentencePiece는 binary heap과 같은 알고리즘을 사용한다. 이 알고리즘은 BPE의 계산 비용을 $O(N^2)$에서 $O(N \log N)$로 줄일 수 있다.

<br>
<br>


**Self-contained Models**

어떻게 데이터가 pre-tokenization되었는지에 따라 BLUE 점수에 차이가 있었다. 따라서 SentencePiece는 전처리에 대한 모든 규칙과 매개변수를 모델 파일 자체에 포함시켰다. 그래서 동일한 모델 파일을 사용하는 한 동일한 실험 설정을 재현할 수 있다. 즉 모델 파일에 의해서만 결정되며 외부 의존성이 아예 없다.

<br>
<br>


**Result**

![Untitled](/assets/SentencePiece%20A%20Simple%20and%20Language%20Independent%20Su%206d7bbff526ba441ba4c64b18b039759f/Untitled%203.png)

실험의 baseline으로 word 기반 모델을 사용했다. 당연히 SentencePiece는 subword 단위로 분할하기 때문에 word 모델과 비교했을 때 성능이 우수하다. 그리고 pre-tokenization 과정이 있는 것과 없는 것에 큰 차이가 없는 것을 확인할 수 있다. 즉 pre-tokenization이 무조건적으로 BLEU 점수를 향상시키지 않는다. 영어를 일본어로 번역했을 때에는 pre-tokenization을 사용했을 때 오히려 성능이 떨어졌다. 또한 일본어에 SentencePiece를 적용했을 때 성능 향상이 뚜렷하다. 이는 일본어가 공백이 없는 언어이기 때문이다. 일본어 번역 과정에서 pre-tokenization이 큰 제한 요소였음을 시사한다.

![Untitled](/assets/SentencePiece%20A%20Simple%20and%20Language%20Independent%20Su%206d7bbff526ba441ba4c64b18b039759f/Untitled%204.png)

먼저 영어 데이터에서는 SentencePiece를 사용하든 subword-nmt를 사용하든 training을 하거나 분할을 할 때 pre-tokenization을 수행하는 것이 속도에 큰 영향을 미치지 않는다. 그러나 일본어 데이터에서는 SentencePiece를 사용했을 때 속도 면에서 현저한 차이를 보여준다. 이는 SentencePiece가 일본어와 같은 공백이 없는 언어에 효율적으로 작동하여 속도를 크게 향상시킬 수 있음을 나타낸다.

<br>
<br>


paper

[https://arxiv.org/abs/1808.06226](https://arxiv.org/abs/1808.06226)