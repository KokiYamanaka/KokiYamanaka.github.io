---
title:  "LLM recap in 2023"
mathjax: true
categories: media
---

## Abstract 

## Table of contents 
* Hardware Optimization 
* Model Architecture
* Metrics 
* Prompt Engineering
* Retrievals
* Applications 

### Hardware Optimization 
Large Language Models (LLMs) requires substantial GPU memory for training, faces high cost issue for small companies and inference, posing challenges for deployment on low-GPU devices like taxi monitors in self-driving scenarios. To address this, Quantization and optimized memory allocation have been introduced. [QLORA](https://arxiv.org/abs/2305.14314)
 efficiently fine-tunes LLMs by quantizing pretrained model weights into fewer bits and introducing new domain-specific weights. Quantization can be also thought as a compression process, employed in Image Compression as well. For example, [IGS (Improved gray-scale)](https://inst.eecs.berkeley.edu/~ee225b/sp14/homework/IGS.pdf) utilizes quantization to compress a less meaningful pixel area image of a bottle from 32 bits to 16 bits.
[LLM in a Flash](https://arxiv.org/abs/2312.11514) which enables effective execution of LLMs exceeding DRAM capacity by storing parameters on flash memory (memory is stored when computer is off). Additionally, it loads parameters only for recent tokens, reusing activations from previously computed tokens for optimal efficiency. From these, we expect to have rise in using [light-weight LLM](https://hanlab.mit.edu/blog/tinychat) in low-gpu devices. One example is OpenAI's [AI Device](https://www.bloomberg.com/news/articles/2023-12-26/apple-iphone-design-head-tang-tan-to-work-with-jony-ive-sam-altman-on-ai-tech).


|![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/59cd8197-4415-42a2-8efa-b94e8e272120){:width="500px"} | ![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/00b2177c-8a3d-4649-b75c-210c96885de1){:width="500px"}|

<small>
<p style="text-align: center;">Scenario where light-weight LLM is crucial : Using GPT 3.5 Turbo to drive the vehicle from Turing Inc</p> 
 
[video](https://www.youtube.com/watch?v=B7iBtwQflIE) 


### Model Architecture
Previously, transformers, which uses self attention mechanism (weigh importance of each word with other words to predict the next word) has quadratic time complexity. Now, [MAMBA](https://arxiv.org/abs/2312.00752) lowers this time complexity. The key idea behind this is the Selective SSM (Selective State Space Model). The state space idea is similar to the widely known Hidden Markov Model (HMM), which assumes likelyhood of transitioning to the next state only depends current state, ignoring all the historical states. [Jim Simons, a mathematician who beated the wall street, also achieved a good profit by using HMM]

<br>
Recall that, each RNN hidden state's can't remember a long range of sequence. Selective SSM is essentially an "upgraded version" of RNN. Upgraded means the hidden state, which acts as a memory unit, stores a longer window of past sequences. In other words, LLM can remember a broader context in a sentence. In each RNN's hidden state, longer sequence memory retention is achieved by filteringout irrelevant tokens and compressing the relevant tokens.
This mechanism is called the “selective”.

|![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/83ce145b-5543-4056-8657-d28f02e612ed)|

The in depth versions of this mechanism works as follows. 
In the realm of each RNN's hidden state, extending memory for longer sequences involves a selective mechanism. This process entails filtering out irrelevant tokens and compressing pertinent ones. The underlying mechanism, termed "selective," operates with the following intricacies: 

$$ h'(t) = Ah(t) + Bx(t)  $$ 

$$ y(t) = Ch(t)  $$

<small>
where matrices $$A$$, $$B$$, $$C$$ capture pertinent information from the input sequence, $$x(t)$$ are word embeddings, 
 
and $$h(t)$$ are hidden state are time $$t$$. 

First equation signifies that the next state of the system $$ h'(t) $$ is updated by considering the current hidden state $$Ah(t)$$ and incorporating a temporal hidden state $$ Bx(t) $$. This temporal component is sequence length adjustable. The resulting $$y(t)$$ represents our output sequence derived from the final hidden state matrix.
<br>
For instance, consider the phrase "The cat sat on the mat." SSM computes word correlations by dynamically adjusting the windowing size, a parameter fine-tuned through learnable processes like gradient descent. This dynamic windowing size enables the model to flexibly adapt its behavior to the specificities of encountered data at each step.

### Metrics 
I believe there are 2 main research trend on metrics. One to evaluate inherent capabilities of Foundational Models such as generation efficiency, and another to evaluate the capabilites of Foundation Models on specific domain. 

The first kind includes creating new kind of metrics drawing from other fields to evaluate Models. [Elo Uncovered](https://arxiv.org/abs/2311.17295), studies the suitability of using chess game ranking to evaluate the capabilities of LLM. It focuses to prove 2 axioms, reliability and transitivity. 
Transitivity in evaluations eliminates the need for costly head-to-head model comparisons. If Model A is deemed superior to Model B and Model B is superior to Model C, transitivity suggests that Model A is also superior to Model C ($$A$$ > $$B$$ and $$B$$ > $$C$$ implies $$A > C$$). 

The second trend, more prevalent in industry, involves evaluating foundational models based on their domain-specific capabilities. An example is the paper [Evaluation of Large Language Models for Decision Making in Autonomous Driving](https://arxiv.org/abs/2312.06351) by Turing. Key considerations include using a couple models, identifying the essential LLM capabilities (spatial awareness and traffic rule adherence) needed for the task (driving in complex scenarios), setting up a design for quantitative evaluation. 
 

<p align="center">
  <img width="460" height="300" src="https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/bd509cfe-d55d-400b-97a5-d45b8bf432dd">
</p>

|<small>
Comparison of LLMs’ accuracy for spatial-aware decision-making (SADM), following the
traffic rules (FTR), both combined (SADM&FTR)
 
For decision making task, initially finding out the kind capabitilies LLM needed is crucial for production as it reduces the development time.  

### Prompt Engineering 		
This topic may seem ridiculous to some, as it gives the notion of "no-code." However, it becomes crucial for organizations aiming to build LLM-products under cost constraints, as fine-tuning incurs high expenses. In early 2022, CoT ([Chain of Thought](https://arxiv.org/abs/2201.11903)) suggested that LLMs generate sequences in a "think step-by-step" process. This led to various variants such as ToT ([Tree of Thought](https://arxiv.org/abs/2305.10601)), CoC ([Chain of Code](https://arxiv.org/abs/2312.04474)), and [System 2 Attention](https://arxiv.org/abs/2312.04474). 

ToT breaks down problems into smaller components organized in a tree-like structure. It allows for backtracking to revise decisions, scores intermediate solutions, and offers a more cost-effective computation compared to CoT.

CoC  aims to enhance reasoning by "emulating" problem-solving through coding. The process involves solving problems by writing code, and the model then generates output text similar to the code written. This gives LLM to higher-level reasoning, allowing the model to understand and articulate solutions in a manner akin to traditional programming. An example scenario could be solving a math problem, such as finding the average number of people, where the model leverages coding logic to produce a coherent and structured textual output.

Q : find average 10, 15, 20, 25, 30

Traditional 
```
A : The average of 10, 15, 20, 25, and 30 is 20.
```

Chain of code
```python
step1 : 
numbers = [10, 15, 20, 25, 30]
average = sum(numbers) / len(numbers)
print("The average is:", average)
    
step2 : 
A : The average of 10, 15, 20, 25, and 30 is 20.
```
|<small>
Simulation between usual genearation vs generation with CoC

System 2 Attention, inspired by cognitive science and similar the book [Thinking, Fast and Slow](https://www.amazon.ca/Thinking-Fast-Slow-Daniel-Kahneman/dp/0374533555) argues, adapts the dual-system theory for prompt engineering. It basically suggest LLM to illuminate input sentences that requiring deeper reasoning, and re-encoding the identified segnments. It shifts from fast, intuitive System 1 processing to slow, focused System 2 reasoning by emphasizing specific input sections. For instance, when LLM encouters long input sequences where sub-sequences requires some arithmetic calculation, this mechanism comes into play. 


These papers essentially propose LLM to think/generate in different ways inspired by cognitive science or psychology or perhaps in other way how "humans" solve problems. Some are somewhat equivalent to what traditional machine learning is doing. ToT is akin to a decision tree in traditional machine learning, while CoT mimics traditional programming, producing output rather than rules (an ML technique). 

 
### Retrievals
They are frequently employed to enhance Large Language Models (LLMs) with increased context awareness. There exist pre and post retrieval frameworks. Former performs a query and latter (filtering, reranking, prompt compression) refines the query for accuracy. The former seems to have more attention by major conferences. [Jina](https://arxiv.org/abs/2307.11224), 8k token text embedding model, suggests accurate context awareness occur when longer sequences are stored per unit retrieval vector, while [Dense X Retrieval](https://arxiv.org/abs/2312.06648), opposes Jina, saying that shorter sequences are more accurate. Dense X says "proposition" (short and clear statement) as per unit vector is better. 

|![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/b9b99e86-98cf-4687-8b2a-b94aebd56ddd)|![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/a0f0b605-276d-4a74-9459-c9fc937f7a7b)|



In the Tower of Pisa example, proposition retrieval efficiently conveys the angle without unnecessary details: "The Leaning Tower of Pisa now leans at about 3.99 degrees."




## Applications 
One interesting applications of LLM that I've seen is how LLM is used in Trading. Till now, we've used stock features (e.g. price, volume) follow by traditional Time Series or Machine Learning models to perform prediction. However, more "publicly available information" are needed as sources for predictions. These additional sources can be "textual data". It could be news headlines or summaries, crypto analytic report, etc. We need to use ingest these sources to LLM, and provide useful insights to make prediction. 

For instance, "[Can ChatGPT Forecast Stock Price Movements? Return Predictability and Large Language Models](https://arxiv.org/abs/2304.07619)" utilized LLM to analyze public sentiments towards a certain stock price. It uses larger parameter models like ChatGPT4 to produce a sharpe ratio (returns with good degree of risks) of 3.8 (>3 is generally good) on some stock prices. Rating, explaination, confidence of generation is also asked by LLM to measure model's explanability. 

|![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/3c9f9e48-17ab-4ec8-8511-ac01e4cf8047)|
<small> 
Cumulative Returns of Investing $1 (Without Transaction Costs)

The task left for us is to "deepen" LLM insights. The focus should extend beyond surface-level information, exploring more profound layers of textual data for a nuanced understanding of market dynamics. Some Quant research institution from China has been doing this"[Integrating Stock Features and Global Information via Large Language Models for Enhanced Stock Return Prediction](https://arxiv.org/abs/2310.05627)". They created Local-Global model to extract deeper level insights and SCRL (Self-Correlated Reinforcement Learning) to focus on aligning news text with stock features on same semantic space. Align here means creating a common quantitatitve space for better sentiment analysis.

![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/b8e8ff7e-2e8d-44ae-ba68-f0e6490dbfcd)
|<small>
Self-Correlated Reinforcement Learning with Local Global Model

Also, "[NuScenes-MQA: Integrated Evaluation of Captions and QA for Autonomous Driving Datasets using Markup Annotations](https://arxiv.org/abs/2312.06352)", creates QA datasets enclosed with markups. Markups for instance, highlight important objects captured by a car camera (e.g., "How many <obj> cars </obj> are in <cam>front</cam> of the ego car?"). These highlighted details are then sent to Large Language Models (LLMs) to make driving decisions. Although it's not the most effective, but we may use apply these technique in Quant Trading. One may take Bitcoin news represented in jpg format as input data to construct or train visual model. 
