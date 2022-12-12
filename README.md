# LLMAssistedResearch
<p align="center">
<a href="https://github.com/eiffl/LLMAssistedResearch/actions/workflows/build.yml">
<img src="https://github.com/eiffl/LLMAssistedResearch/actions/workflows/build.yml/badge.svg?branch=main" alt="Article status"/>
</a>
<a href="https://github.com/eiffl/LLMAssistedResearch/raw/main-pdf/arxiv.tar.gz">
<img src="https://img.shields.io/badge/article-tarball-blue.svg?style=flat" alt="Article tarball"/>
</a>
<a href="https://github.com/eiffl/LLMAssistedResearch/raw/main-pdf/ms.pdf">
<img src="https://img.shields.io/badge/article-pdf-blue.svg?style=flat" alt="Read the article"/>
</a>
<a href="https://colab.research.google.com/github/EiffL/LLMAssistedResearch/blob/main/notebooks/NzInference.ipynb" target="_blank"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>
</p>

My attempt at using ChatGPT to accelerate a research project. 

Here is the question: **using ChatGPT, how long would it take to go from a rough idea to a ready-to-submit paper?**

**[Current State]** Here is what I got out of *1h30 mins* of chatting with ChatGPT [here](https://github.com/EiffL/LLMAssistedResearch/blob/main-pdf/ms.pdf). The text comes from our conversation, where I tell it what I'm looking for, and provide feedback on the text it gives me. I have not written a single sentence there.

Obviously in a second stage, a human would need to go over the draft and do some editing. Experts may be able to notice a few things that are not quite right, in particular in the citations. 

**Note**: I am not taking sides on the question "should we use these models to write papers?" I am interested in the question "*can* we use these models to write papers?", and by that I mean can they be helpful to do science. I am absolutely not interested in completely automatically writing a paper from the title and list of authors (which it could do, but that wouldn't be very convincing if an expert read it), but whether you can write a paper by having a conversation with it and guiding it towards what you want.

## The things that blew my mind

#### It understands math and can modify equations from english prompts
> ChatGPT: In this analysis, we propose using a mixture of Beta distributions to model the redshift distribution for each tomographic bin. The redshift distribution is given by:
>$p(z) = \sum_{k=1}^{K} w_k \cdot Beta(z; \alpha_k, \beta_k)$

> EiffL: Can you modify this formula to include a rescaling of the distribution so that it is supported between [0, z_{max}] ?

> ChatGPT: Sure, here is a revised version of the previous paragraph that includes a formula for the mixture of Beta distributions model with a rescaling of the distribution:
>"In this analysis, we propose using a mixture of Beta distributions to model the redshift distribution for each tomographic bin. The redshift distribution is given by:
>$p(z) = \frac{1}{z_{max}} \sum_{k=1}^{K} w_k \cdot Beta\left(\frac{z}{z_{max}}; \alpha_k, \beta_k\right)$

## Here is what I did so far

### Step I: The idea (no ChatGPT inputs there)
I started by thinking about a question I'm curious about, which came up just in the last week: "Do we actually need to use photometric redshifts at all when we have cross-correlations with a secure clustering sample in a 3x2pt analysis? Can we not infer the full source distribution jointly with the cosmological posterior? How much information would we have left on cosmology if we did that?" 

I chose this question because it's fairly straightforward to setup a toy analysis to answer that question, so the biggest bottleneck (for me) would be paper writing. 

I did try to use ChatGPT to brainstorm a few project ideas, but nothing very interesting (to me) came out of it, everything was very generic.

### Step II: Getting the results working (a tiny amount of ChatGPT here for coding)
It took me a couple of hours to make this [notebook](main/notebooks/NzInference.ipynb). ChatGPT was no help there, the code I needed was very specialized, and also I reused a bunch of existing code. It did generate a mostly correct code for a normalizing flow though ^^ but in the end I used my own. 

For the experts: If you are curious btw, here are my posteriors on the source galaxy n(z) in a DES Y1 3x2pt analysis, without assuming any knowledge of photometric redshifts for the lensing source galaxies: 

![image](https://user-images.githubusercontent.com/861591/206940145-5175eb90-d641-4918-a6b4-1c0ade2ac478.png)

pretty cool, pretty cool :-)

### Step III: Writing (here I'm letting ChatGPT do all the work)

Finally, this is ChatGPT's turn to work! My approach to use it for paper writing is to start the conversation with the following prompt:
```
Let's write a scientific journal article, on the topic of observational cosmology, 
in the typical style of MNRAS articles. The paper will show how to perform joint 
inference of weak lensing source galaxy redshift distribution and cosmological 
parameters in a 3x2pt analysis, without needing any photometric redshift information. 
We will use variational inference to infer these distributions, and will demonstrate 
the approach in a DES Y1 3x2pt setting. 
Can you suggest a title for this article?
```
This is more or less all I need to tell it about the project at the beginning, it will generate a very appropriate, and I can interact with it to change it to my liking. It then generates automatically the abstract if I ask it.

For the introduction, it takes a tiny bit more work/prompt engineering to get it to write in the correct style. For instance I use the following prompt to make it uses proper citations, and make sure it includes an opening contextualisation pragraph:

```
Ok, now, please write the introduction for this paper, in the typical style of 
an MNRAS paper. Include specific citations in the format (Author et al., year) 
where appropriate. The introduction should begin by a paragraph introducing weak 
lensing in the context of Stage III and Stage IV cosmological surveys.
```

And it will generate a bunch of text, in particular saying things that are exactly on point, without me having to explain anything more than the previous prompts:
>Traditionally, this has been done using photometric redshifts, which are estimated from the observed colors of the galaxies (Zuntz et al., 2018). However, photometric redshifts are subject to significant uncertainties and biases, which can impact the accuracy of weak lensing measurements (Bernstein, 2012; Hildebrandt et al., 2020). In this paper, we present a novel approach for inferring the redshift distribution of weak lensing source galaxies directly from joint correlations with galaxy clustering, without the need for photometric redshifts. Our method uses variational inference to estimate the redshift distribution from the data, and we apply it to simulated data from the Dark Energy Survey Year 1 (DES Y1).

Note however that **it will struggle with citations, you should replace/check all of them by hand afterwards**. The very famous and typical ones are ok, like (Kilbinger et al. 2015), or (Laureijs et al. 2011). But this is because these are very common, there is very low variance there. For slightly more specific citations, it will pick the name of a likely author in this field, and a likely year ^^' Above all else, do not attempt to get it to give you the full bibliographic record for a citation, it's hilarious, it associates random authors, and even generates arxiv numbers that point to completely different papers (this is because there is no pattern to arxiv codes appart from the date).


After about 45 mins (not helped by the OpenAI server being overloaded), I got two pages of first draft text for an introduction and first part of the methodology section see [here](https://github.com/EiffL/LLMAssistedResearch/blob/main-pdf/ms.pdf). I struggled a bit to get it to write exactly the equations I was looking for, so this is the best I could do, and I would need to manually change a few things later. 


An open source scientific article created using the [showyourwork](https://github.com/showyourwork/showyourwork) workflow.
