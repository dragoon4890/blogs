---
title: "Setting up a small tts model for Android"
date: "2026-01-29"
description: "Some notes"
---


I was thinking of making this personal project for a long of time. It has been maybe like 2 months 
since i thought of it. Leaving that part aside, a TTS ( text to speech) model is crucial for this project

Rewriting this blog for the first time since 29th Jan.

My end goal is running these models on phones, preferably older ones ( Snapdragon 660), so not really looking for a big model.

At a quick glance, kokoro 82m looks neat, need to try that. 

According to the HuggingFace page, it looks like it uses something called G2P model. I had a quick look at it. I don't really understand the need 
for grapheme. It's a representation dividing words into subparts. These subparts are later converted to Phonemes. Phonemes are standard and guide how to speak. 
