---
title:  "llm advances in 2023"
mathjax: true
categories: media
---

## Table of contents 
Theoretical improvements
* Hardware Optimization 
* Model Architecture
* Metrics 
* Prompt Engineering
* Retrievals
* Applications 

## Thereotical improvements
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


Recall that, each RNN hidden state's can't remember a long range of sequence. Selective SSM is essentially an "upgraded version" of RNN. Upgraded means the hidden state, which acts as a memory unit, stores a longer window of past sequences. In other words, LLM can remember a broader context in a sentence. In each RNN's hidden state, longer sequence memory retention is achieved by filteringout irrelevant tokens and compressing the relevant tokens.
This mechanism is called the “selective”.

|![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/83ce145b-5543-4056-8657-d28f02e612ed)|

The in depth versions of this mechanism works as follows. 
In the realm of each RNN's hidden state, extending memory for longer sequences involves a selective mechanism. This process entails filtering out irrelevant tokens and compressing pertinent ones. The underlying mechanism, termed "selective," operates with the following intricacies: 

$$ h'(t) = Ah(t) + Bx(t) $$

$$ y(t) = Ch(t) $$

where matrices $A$, $B$, $C$ capture pertinent information from the input sequence.

Equation (1) signifies that the next state of the system is updated by considering the current hidden state and incorporating a temporal hidden state. The resulting y(t) represents our output sequence derived from the final hidden state matrix.

Illustratively, consider the phrase "The cat sat on the mat." SSM computes word correlations by dynamically adjusting the windowing size, a parameter fine-tuned through learnable processes like gradient descent. This dynamic windowing size enables the model to flexibly adapt its behavior to the specificities of encountered data at each step.

## MathJax

You can enable MathJax by setting `mathjax: true` on a page or globally in the `_config.yml`. Some examples:

[Euler's formula](https://en.wikipedia.org/wiki/Euler%27s_formula) relates the  complex exponential function to the trigonometric functions.

$$ e^{i\theta}=\cos(\theta)+i\sin(\theta) $$

The [Euler-Lagrange](https://en.wikipedia.org/wiki/Lagrangian_mechanics) differential equation is the fundamental equation of calculus of variations.

$$ \frac{\mathrm{d}}{\mathrm{d}t} \left ( \frac{\partial L}{\partial \dot{q}} \right ) = \frac{\partial L}{\partial q} $$

The [Schrödinger equation](https://en.wikipedia.org/wiki/Schr%C3%B6dinger_equation) describes how the quantum state of a quantum system changes with time.

$$ i\hbar\frac{\partial}{\partial t} \Psi(\mathbf{r},t) = \left [ \frac{-\hbar^2}{2\mu}\nabla^2 + V(\mathbf{r},t)\right ] \Psi(\mathbf{r},t) $$

## Code

Embed code by putting `{{ "{% highlight language " }}%}` `{{ "{% endhighlight " }}%}` blocks around it. Adding the parameter `linenos` will show source lines besides the code.

{% highlight c %}

static void asyncEnabled(Dict* args, void* vAdmin, String* txid, struct Allocator* requestAlloc)
{
    struct Admin* admin = Identity_check((struct Admin*) vAdmin);
    int64_t enabled = admin->asyncEnabled;
    Dict d = Dict_CONST(String_CONST("asyncEnabled"), Int_OBJ(enabled), NULL);
    Admin_sendMessage(&d, txid, admin);
}

{% endhighlight %}

## Gists

With the `jekyll-gist` plugin, which is preinstalled on Github Pages, you can embed gists simply by using the `gist` command:

<script src="https://gist.github.com/5555251.js?file=gist.md"></script>

## Images

Upload an image to the *assets* folder and embed it with `![title](/assets/name.jpg))`. Keep in mind that the path needs to be adjusted if Jekyll is run inside a subfolder.

A wrapper `div` with the class `large` can be used to increase the width of an image or iframe.

![Flower](https://user-images.githubusercontent.com/4943215/55412447-bcdb6c80-5567-11e9-8d12-b1e35fd5e50c.jpg)

[Flower](https://unsplash.com/photos/iGrsa9rL11o) by Tj Holowaychuk

## Embedded content

You can also embed a lot of stuff, for example from YouTube, using the `embed.html` include.

{% include embed.html url="https://www.youtube.com/embed/_C0A5zX-iqM" %}
