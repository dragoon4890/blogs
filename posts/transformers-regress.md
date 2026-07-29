---
title: "Can I revert to local attention in  cross attention transformers?"
date: "2025-07-25"
description: " Case study on one of later curiosities"
---


While I was learning about optimizing transformers, I had an idea. If I could just switch to sliding attention (local attention) in Decode stage of transformers based.
Specifically, Seed-VC, it works like an upsampler, taking  0-31 class input (codebook size is 32) and another reference input (2048 codebook),
outputting a 2048 codebook ouput. 
Essentially, it has information bottleneck on the data provided. 32 class inputs are stripped down information which is our input, which we try to predict the lost actual information
using a information rich 2048 class output. The model is trained in such a way, that in layman's terms, it takes 32 tokens refers to 2048 class tokens provided, to produce output in 2048.


![alt text](image.png)


The overall experiment shows failure but at varying levels. 

The first mode of failure, was a very naive approach of sliding window. I used size of 10 as sliding window. By doing this, at just the prefill stage, the tokens (KV cached) were flushed out. Generally, these models have EOF suppression until some tokens ( 10), so by the 10th token. It was going towards catastrophic failure. The reason for this is incomplete conditioning. 
The attention at every stage helps in figuring out the next token and also EOF.

Quickly, sliding window logic was changed to always keep the prefill stage output in the computation, and apply the window logic on the generated output. The result was random, it 
would work most of the time but with different length outputs and sometimes it would miss inputs. This is due to drifting, it gets into a different generation path and maybe corrects itself. 

So for the most part, the sliding attention on local window, if it's trained on full, is highly erratic. To mitigate, we can go for a summary of the generation, weighted more on recent past etc.