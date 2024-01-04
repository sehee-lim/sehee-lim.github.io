---
layout: post
title: "Encouraging Divergent Thinking in Large Language Models through Multi-Agent Debate"
category: "NLP"
date: 2024-01-04
--- 

<br>

Self-reflection은 내놓은 답변에 대해 LLM이 스스로 피드백을 하고 답변을 수정하는 방법이다. 하지만 이러한 방법은 Degeneration-of-Thought이라는 문제점이 있다. 즉 LLM이 답변에 대해 확신이 있으면 그 답변이 틀렸더라도 새로운 방향의 답변을 생성하기 힘들다는 것이다. 이 논문에서는 이러한 DoT 문제를 해결하기 위해 Multi-Agent Debate (MAD) 방법을 제안한다. 이 방법에서는 여러 에이전트가 “tit for tat” 상태에서 자신의 주장을 표현하고, 판사가 토론 과정을 관리하여 최종 해결책을 도출한다.

> *Once the LLM has established confidence in its answers, it is unable to generate novel thoughts later through self-reflection even if the initial stance is incorrect.*
> 

![MAD 방법이 Self-Reflection 방법보다 더 다양한 답변을 내놓는 것을 확인할 수 있다.](/assets/Encouraging%20Divergent%20Thinking%20in%20Large%20Language%20M%20f74de652019b4493956d62e4a30fa4be/Untitled.png)

MAD 방법이 Self-Reflection 방법보다 더 다양한 답변을 내놓는 것을 확인할 수 있다.


<br>
<br>

**Contributions**

1. Self-Reflection 방법에서 Degeneration-of-Thought(DoT) 문제를 정의하고 Multi-Agent Debate(MAD)라는 해결책을 제시함
2. MAD 방법을 사용하면 GPT-3.5-Turbo를 사용했음에도 GPT-4의 성능을 뛰어넘음
3. 좋은 성능을 위해서는 토론에서 적응적인 break 전략과 적당한 수준의 “tit for tat” 상태가 필요함, 또한 서로 다른 LLM이 에이전트에 사용된다면 LLM이 공정한 판사 역할을 잘 하지 못함

![Iteration이 길어지면 점점 성능이 저하됨](/assets/Encouraging%20Divergent%20Thinking%20in%20Large%20Language%20M%20f74de652019b4493956d62e4a30fa4be/Untitled%201.png)

Iteration이 길어지면 점점 성능이 저하됨

![Agent들이 서로 반대되는 의견을 내게 지나치게 강제하면 성능이 오히려 저하됨](/assets/Encouraging%20Divergent%20Thinking%20in%20Large%20Language%20M%20f74de652019b4493956d62e4a30fa4be/Untitled%202.png)

Agent들이 서로 반대되는 의견을 내게 지나치게 강제하면 성능이 오히려 저하됨

![판사는 반박하는 쪽을 선호하는 경향이 있음, 또한 같은 모델을 사용하는 측으로 기우는 경향이 있음 (서로 다른 LLM이 에이전트로 사용되면 LLM은 공정한 판사 역할을 하지 못함)](/assets/Encouraging%20Divergent%20Thinking%20in%20Large%20Language%20M%20f74de652019b4493956d62e4a30fa4be/Untitled%203.png)

판사는 반박하는 쪽을 선호하는 경향이 있음, 또한 같은 모델을 사용하는 측으로 기우는 경향이 있음 (서로 다른 LLM이 에이전트로 사용되면 LLM은 공정한 판사 역할을 하지 못함)

<br>
<br>

paper

[https://arxiv.org/pdf/2305.19118.pdf](https://arxiv.org/pdf/2305.19118.pdf)