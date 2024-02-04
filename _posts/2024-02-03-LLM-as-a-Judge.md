---
layout: post
title: "Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena"
category: "NLP"
date: 2024-02-03
--- 

<br>


![Untitled](/assets/Judging%20LLM-as-a-Judge%20with%20MT-Bench%20and%20Chatbot%20A%20835d9a0ddfcc46658e6dd366a02c94c2/Untitled.png)

연방준비제도가 2차 시장에서 채권을 매입할 경우 그 결과에 대한 질문을 던진다. 이어서 연방준비제도의 행위가 일상 생활에 미치는 영향에 대해 세 가지 예시를 들어 설명하라는 후속 질문을 던진다. 즉 여러 turn에 걸쳐서 질문을 하는 multi-turn 형식이다. Assistant A는 LLaMA-13B 모델을, Assistant B는 같은 모델을 고품질 대화 데이터로 fine-tuning한 Vicuna-13B 모델을 사용했다. 이에 대해 GPT-4는 Assistant B의 답변이 더 적절하다고 판단했으며, 이는 사람의 판단에도 부합한다. Assitant B는 일상 생활에 미칠 구체적이고 관련 있는 예시를 제시했기 때문이다. **전통적인 벤치마크는 인간의 선호를 충분히 반영하지 못하는 경향**이 있다. 위의 예시에서 Assistant A와 Assistant B는 모두 전통적인 벤치마크에서 비슷한 성능을 보인다. 하지만 사람의 관점에서 볼 때 Assistant A의 답변은 적절하지 않다. 이로 인해 인간의 선호를 더 잘 반영하고 평가할 수 있는 새로운 벤치마크의 필요성이 대두되었다. 본 논문에서는 인간의 선호도를 반영하여 AI 챗봇의 대화 성능을 평가할 수 있는 두 가지 새로운 벤치마크를 소개하고 있다.


<br>

1. **MT-bench**: 80개의 고품질 multi-turn 질문으로 구성되었다. 챗봇의 multi-turn 대화 능력과 지시 사항을 따르는 능력을 평가한다. 이 두 가지 능력은 인간의 선호도를 평가하는 중요한 요소이다. 또한 MT-bench는 추론 능력이나 수학 문제 해결 능력으로도 챗봇을 평가할 수 있다. 총 8개의 카테고리(writing, roleplay, extraction, reasoning, math, coding, knowledge I (STEM), knowledge II (humanities/social science))가 있다. 각 카테고리에 대해 10개의 후속 질문을 디자인했다.
    
![MT-bench의 예시 (Writing, Math, Knowledge)](/assets/Judging%20LLM-as-a-Judge%20with%20MT-Bench%20and%20Chatbot%20A%20835d9a0ddfcc46658e6dd366a02c94c2/Untitled%201.png)
    
    
2. **Chatbot Arena**: 실생활과 유사한 상황에서 챗봇들이 어떻게 대응하는지 비교하고 평가하기 위해 개발된 벤치마크이다. 실제 시나리오에서 익명의 챗봇 간에 전투를 하는 크라우드소싱 플랫폼이다. 이 플랫폼에서 사용자들은 동시에 두 챗봇과 대화를 진행하고 자신이 선호하는 응답을 제공한 모델에 투표한다. 투표가 끝난 이후에는 모델의 정체가 공개된다. 한 달 동안 Chatbot Arena를 운영한 결과 약 3만 개의 투표가 수집되었다. 이 플랫폼은 사전에 정의된 질문을 사용하지 않기 때문에 사용자들의 다양한 관심사를 바탕으로 다양한 사용 사례를 수집할 수 있었다.

![사용자가 본인의 선호도에 따라 모델에 투표함](/assets/Judging%20LLM-as-a-Judge%20with%20MT-Bench%20and%20Chatbot%20A%20835d9a0ddfcc46658e6dd366a02c94c2/Untitled%202.png)



<br>
<br>


### LLM-as-a-Judge

인간의 선호도를 수집하는 것은 비용이 많이 들고 시간이 오래 걸린다. 이를 극복하기 위해 자동화된 접근 방식을 개발한다. MT-bench와 Chatbot Arena의 대부분의 질문들은 정해진 정답이 없는 개방형 질문들이라 rule-based로 답변을 평가하는 프로그램을 만드는 것은 적절하지 않다. 답변과 참조 답안 사이의 유사성에 기반한 전통적인 평가 지표(BLEU 등)도 적절하지 않다. LLM이 계속해서 발전함에 따라 많은 작업에서 인간 annotator를 대체할 가능성이 보인다. LLM을 judge로 사용하는 것의 장단점에 대해 논의해본다.


<br>

LLM-as-a-Judge를 사용하는 세 가지 변형 방법이 있다. 이 방법들을 독립적으로 사용하거나 결합하여 사용할 수 있다.

1. **Pairwise Comparison:** 질문과 두 가지 답변을 제시하면 어느 쪽이 더 적절한지 혹은 무승부를 선언할지 결정하도록 한다.
    
    ![Untitled](/assets/Judging%20LLM-as-a-Judge%20with%20MT-Bench%20and%20Chatbot%20A%20835d9a0ddfcc46658e6dd366a02c94c2/Untitled%203.png)
    
2. **Single Answer Grading**: 답변에 직접 점수를 매기도록 한다.
    
    ![Untitled](/assets/Judging%20LLM-as-a-Judge%20with%20MT-Bench%20and%20Chatbot%20A%20835d9a0ddfcc46658e6dd366a02c94c2/Untitled%204.png)
    
3. **Reference-Guided Grading**: 수학 문제와 같은 경우에는 참조할 만한 해설을 제공하는 것이 좋을 때가 있다. 
    
    ![Untitled](/assets/Judging%20LLM-as-a-Judge%20with%20MT-Bench%20and%20Chatbot%20A%20835d9a0ddfcc46658e6dd366a02c94c2/Untitled%205.png)
    


<br>

**LLM-as-a-Judge의 이점**: 인간의 개입이 필요한 부분을 줄여서 빠르고 많이 반복할 수 있게 한다. 또한 LLM judge는 점수 뿐만 아니라 설명도 제공한다.


<br>


**LLM-as-a-Judge의 단점**:

1. **Position Bias**: LLM이 특정 위치를 선호하는 경향을 보인다. GPT-4에게 GPT-3.5의 답변과 Vicuna-13B의 답변을 평가하게 했다. GPT-3.5가 첫 번째로 위치할 때 GPT-4는 GPT-3.5의 답변을 더 우수하다고 판단했고, 두 응답의 위치를 바꾸었을 때 GPT-4는 Vicuna-13B의 답변을 더 선호했다. 이 bias의 기원에 대해서는 훈련 데이터 때문일 수도 있고, 트랜스포머의 left-to-right 아키텍처 때문일 수도 있다.
2. **Verbosity Bias**: LLM이 더 길고 장황한 응답을 선호하는 경향을 보인다. 하지만 GPT-4는 다른 모델들보다 이 bias에 더 잘 방어한다.
3. **Self-Enhancement Bias**: 인간과 달리 일부 judge는 특정 모델을 선호한다. 예를 들어 GPT-4는 GPT-4의 답변을 10% 더 선호하며 Claude-v1은 Claude-v1의 답변을 25% 더 선호한다. 하지만 GPT-3.5는 GPT-3.5의 답변을 선호하지 않는다.
4. **Limited Capability in Grading Math and Reasoning Questions**: LLM은 수학 및 추론 능력이 뛰어나지 못하다. 정답을 모르기 때문에 이러한 질문들을 평가하는 데 실패하는 것이다. 그러나 GPT-4는 풀 수 있는 문제임에도 잘못된 답변에 휩쓸려서 잘못된 판단을 내린다. GPT-3.5와 Claude-v1 모두 비슷한 현상을 보인다.


<br>

**LLM-as-a-Judge의 단점 해결**:

1. **위치 변경하기**: 두 답변의 순서를 바꾸어서 LLM에게 두 번 답변을 내게 한다. 두 번 모두 같은 선호 답변을 내놓는다면 그 답변에 승리를 선언한다. 순서를 바꾼 후 결과가 다르면 무승부로 한다.
2. **Few-Shot Judge**: GPT-3.5와 Vicuna로 답변을 생성하고, GPT-4로 판단을 생성하게 해서 세 가지 좋은 예시를 만든다. 이렇게 했을 때 GPT-4의 판단의 일관성이 65.0%에서 77.5%로 올랐다. 하지만 높은 일관성이 높은 정확성을 의미하는 것은 아니며 few-shot 예시들이 오히려 새로운 편향을 만들지는 확실하지 않다. 그리고 더 긴 prompt는 API 호출 비용이 4배가 된다.
3. **Chain-of-Thought and Reference-Guided Judge**: 먼저 judge가 수학 문제를 풀게 하고 그 다음에 평가를 시작하게 한다. 그러나 이렇게 했을 때에도 LLM은 제시된 답변과 정확히 동일한 오류를 범하는 것을 확인했다. 혹은 LLM이 답변을 먼저 생성하게 하고 이것을 참조 답변으로 표시한다. 이때 기본 prompt보다 실패율이 70%에서 15%로 크게 개선되었다.


<br>

paper

[https://arxiv.org/abs/2306.05685](https://arxiv.org/abs/2306.05685)