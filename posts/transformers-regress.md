---
title: "Can You Force Local Attention in Cross-Attention Transformers?"
date: "2025-07-25"
description: "Case study on one of my recent optimization curiosities"
---

While I was learning about optimizing transformers, I had an idea: what if I could just switch to sliding attention (local attention) in the Decode stage of transformer-based models?

I decided to test this curiosity on Seed-VC, a cross-attention transformer that essentially acts as an audio upsampler. It takes a heavily stripped-down 32-class input and uses a rich 2048-class reference to predict the lost information and output a high-fidelity 2048-class result. 

Because of this built-in information bottleneck, I wondered if we could cheat the system during generation to save on compute.

![alt text](image.png)

The overall experiment resulted in failure, but at varying levels. 

### Failure Mode 1

The first mode of failure was a very naive sliding window approach. I used a size of 10 as the sliding window. By doing this, right at the prefill stage, the tokens (KV cached) were flushed out. Generally, these models have EOF (End of File) suppression until around the 10th token. So by token 10, the model was heading toward catastrophic failure. The reason for this is incomplete conditioning. 

The attention at every stage helps in figuring out the next token and also the EOF.

### Failure Mode 2

Quickly, the sliding window logic was changed to always keep the prefill stage output in the computation, applying the window logic only on the generated output. The result was random. It would work most of the time but with different length outputs, and sometimes it would miss inputs entirely. This is due to drifting. It gets into a different generation path and tries to correct itself. 

So for the most part, forcing a sliding attention local window on a model trained on full attention is highly erratic. To mitigate this, we likely need a summary of the generation, weighted more heavily on the recent past.