# Optimality Gap of LLM-based Agents in Gridworld Problem

## Introduction
In this research project, we delve into the fascinating world of Reinforcement Learning (RL) by exploring a Taxi Gridworld problem. Our journey involves the use of advanced techniques like Large Language Models (LLM) and the Langchain library, showcasing how these tools can be leveraged to solve complex RL challenges.

![Taxi Grid world](https://gymnasium.farama.org/_images/taxi.gif)

## Understanding the Gridworld: 
We first familiarize ourselves with the Gridworld environment, its rules, and dynamics.

In this section, we developed an optimal policy as Benchmark leveraging Q-learning:

![Optimal Episode](https://github.com/XO-Appleton/RL-LLM-Optimality-Research/blob/main/outputs/optimal_episode.gif?raw=true)

## GPT Chat Completion: 
Utilizing LLM, particularly GPT-4, we explore how language models can aid in decision-making processes. We compared various
factor including prompt engineering techniques and environment encoding methods.

One Example: GPT-4-turbo with ToT prompting and location encoding

![GPT-4-turbo with ToT prompting and location encoding](https://github.com/XO-Appleton/RL-LLM-Optimality-Research/blob/main/outputs/gpt_4_tot_location_encoded.gif?raw=true)

## Langchain ReAct Agent: 
We integrate Langchain, a tool for combining LLMs with decision-making processes, to build an agent with enhanced problem solving capabilities.

It is also the only approach that successfully achieved the objective and demonstrated some interesting capabilities of problem solving such as taking contours and logical reasoning:

![GPT-4-turbo ReAct Agent](https://github.com/XO-Appleton/RL-LLM-Optimality-Research/blob/main/outputs/gpt_4_ReAcT_Agent.gif?raw=true)

## Foundings

![performance_comparison](https://github.com/XO-Appleton/RL-LLM-Optimality-Research/assets/41369365/15a521f5-b337-47e2-b99c-f1156520a2e3)

### Observations:
- Out of 10 different LLM-As-Policy agents that we tried, only the ReAct Agent was able to succeed.
- Most of the unsuccessful agents took 10 to 20 action steps and received -10 to -40 rewards.
- The GPT 3 model with location encodings and CoT prompting took 45 steps and received -70 in a single episode.
- Using the same encoding prompting method, the GPT-4-turbo approaches seemed to perform better than their GPT-3.5 counterpart.

### Location Encoding:
The steps taken and rewards can indicate the performance to some degree. However, it is affected by the randomness of the internal states of the LLM. As we have seen through the process, the LLMs struggled to understand the spatial relationships and retrieve location information most of the time. GPT 4 was occasionally able to spontaneously perform location encoding without given instruction and improve its performance.

The LLM also had a major issue understanding the position of the walls without encoding their locations. I've also tried other methods such as encoding the wall as (row, (cell_1, cell_2)) but it only made the performance worse as LLM became confused about the location encodings in general.
### Prompt Engineering:
Applying Zero-shot CoT did not really improve the performance of a LLM chat model, it could be due to our problem description or the nature of the task already encouraging the LLM to take a CoT approach. ToT on the other hand rather hurt the performance of the LLMs which might be due to the inferior performance of individual Expert.
### LLM as Agent:
Different from the simple one-pass GPT chat completions, building an LLM Agent significantly boosts its performance and enables iterative analysis and freedom of controlling the action directly. In an Agent pipeline, the LLM is able to make an analysis when the observation is different from what it expected and attempts to make new plans. However, it does have a significantly higher inference cost ($0.5 per episode with Agent Execution compared to a single pass costing around $0.03 per episode. Similar proportion in terms of runtime).

## Final Thoughts
Despite demonstrating progress in task comprehension and a much stronger ability for problem-solving compared to their precedents, LLMs are still far from taking on complex tasks in the real world. Even the current SOTA like GPT-4-Turbo has trouble keeping rules in a simple environment like not jumping over a wall despite being specified in the prompt.

It is evident that the performance of the LLM heavily relies on the presence of a well-engineered encoding algorithm to help the LLM perceive the environment and retrieve crucial information. However, the cost of developing such an algorithm could scale drastically with the complexity of the task. We can imagine that the states of a sufficiently intricate environment might not be able to be converted into a textual representation that is accurate, consistent, and complete.

That being said, the current LLMs are designed to imitate how humans write texts instead of focusing on problem-solving and math. It is intriguing how much progress we are going to see in the next couple of years with LLMs finetuned on logical reasoning.
