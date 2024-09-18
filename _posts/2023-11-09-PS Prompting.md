---
layout: post
title: "Plan-and-Solve Prompting: Improving Zero-Shot Chain-of-Thought Reasoning by Large Language Models"
category: "NLP"
date: 2023-11-09
---

<br>


Plan-and-Solve (PS) Prompting은 모델이 복잡한 문제를 해결할 때 구체적인 계획을 세우고 그 계획에 따라 문제를 해결하도록 유도하는 방식이다. Zero-shot-CoT 방법은 세 가지 문제가 있다. 계산 실수, 단계를 빠트리는 것, 그리고 문제를 잘못 이해하는 것이다. 이 중에서 중간 단계를 빠트리는 것을 해결하기 위해서 **PS Prompting**을 제안했다. 이 방법은 전체 문제를 작은 subtask로 나누어 계획을 세우고 그 계획대로 각 단계를 해결하는 것이다. 또한 계산 실수를 줄이고 추론 단계의 질을 높이기 위해서 PS Prompting에 더 자세한 지시 사항을 추가하여 **PS+ Prompting**을 제안한다. PS+ Prompting은 수학 문제 뿐만 아니라 더 넓은 범위의 문제까지 해결할 수 있게 한다.

<br>
<br>


### 👉 **PS Prompting**

![Untitled](/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled.png)

> *Let’s first understand the problem and devise a plan to solve the problem. Then, let’s carry out the plan and solve the problem step by step.*
> 

이 지시 사항은 모델이 계획을 세우고 그 계획을 따르도록 한다. 

<br>
<br>


### 👉 **PS+ Prompting**

![Untitled](/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%201.png)

PS+ Prompting은 PS Prompting에 몇 가지 지시 사항을 더 추가한 것이다.

> *Extract relevant variables and their corresponding numerals*
> 

이를 추가하여 모델이 중간 단계를 뛰어넘지 않도록 한다.

> *Calculate intermediate results (pay attention to calculation and commonsense)*
> 

”*pay attention to calculation*”를 추가하여 모델이 최대한 정확하게 계산하도록 한다.

<br>
<br>


### ▶️ Step 1

![Untitled](/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%202.png)

추론 과정을 생성하기 위한 prompting이다. “*Let’s first understand the problem and devise a plan to solve the problem. Then, let’s carry out the plan and solve the problem step by step*”를 통해 모델이 추론 과정을 생성하게 한다.

### ▶️ Step 2


<img src="/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%203.png" alt="Untitled" class="center-image3">

이후 Step 1에서 생성된 추론 과정과 합하여 최종 답을 이끌어내게 한다. “*Therefore, the answer (arabic numerals) is*”와 같은 answer extraction prompting을 통해 답을 내게 한다.

<br>
<br>



### **Results**

이 기법을 GPT-3에 적용시켜서 실험을 진행했다. 그리고 Arithmetic Reasoning, Commonsense Reasoning, Symbolic Reasoning 데이터셋으로 평가하였다.

<br>

**Arithmetic Reasoning**

![Untitled](/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%204.png)

PS+ Prompting과 PS Prompting은 모든 데이터셋에 대해서 Zero-shot-CoT보다 성능이 좋다. PS+ Prompting는 Zero-shot-PoT보다 6개 중 5개의 데이터셋에서 좋은 성능을 보였다. PS Prompting는 Zero-shot-PoT보다 6개 중 3개의 데이터셋에서 좋은 성능을 보였다. PS+ Prompting는 Manual-CoT보다는 성능이 낮지만 Auto-CoT보다는 성능이 높다. 

<br>

**Commonsense Reasoning**

<img src="/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%205.png" alt="Untitled" class="center-image1">



PS+ Prompting이 PS Prompting보다 성능이 좋기에 PS+ Prompting만 포함시켰다. Manual-CoT보다는 성능이 낮지만 Zero-shot-CoT보다는 성능이 높다.

<br>

**Symbolic Reasoning**

<img src="/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%206.png" alt="Untitled" class="center-image1">



Last Letters 문제에 대해서는 PS+ Prompting은 Manual-CoT와 Zero-shot-CoT보다 성능이 높다. Coin Flip 문제에 대해서는 Manual-CoT보다 성능이 낮지만 Zero-shot-CoT보다는 성능이 높다.

<br>

<img src="/assets/Plan-and-Solve%20Prompting%20Improving%20Zero-Shot%20Chain%20110e862b3c9d4de7beba672a80fd43fa/Untitled%207.png" alt="Untitled" class="center-image1">

Variable definition과 Plan existence와 calculation error, missing-reasoning step error 사이 상관관계를 보면 음수이다. 즉 Variable definition과 계획이 없으면 calculation error와 missing-reasoning step error이 늘어날 수 있다.

<br>
<br>

paper

[https://arxiv.org/abs/2305.04091](https://arxiv.org/abs/2305.04091)