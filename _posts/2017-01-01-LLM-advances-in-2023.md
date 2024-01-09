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

![image](https://github.com/KokiYamanaka/kokiyamanaka.github.io/assets/107101940/59cd8197-4415-42a2-8efa-b94e8e272120){:width="300px"}


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
