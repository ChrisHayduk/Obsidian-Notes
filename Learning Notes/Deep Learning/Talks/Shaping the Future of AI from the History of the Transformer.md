Link: https://www.youtube.com/watch?v=orDKvo8h71o
Speaker: Hyung Won Chung
## Summary
Hyung Won Chung presents a framework for understanding change in dynamic environments. He asserts that the main steps are:
1. Identify the dominant forces behind the change
2. Understand the dominant driving forces
3. Predict the future trajectory

Since AI research is moving too fast, he says that staying at the forefront of the field is impossible, and we should instead look at old ideas and understand *how* they changes using the above framework in order to predict the future of AI. This works better than trying to just stay at the forefront of AI research because it's much easier to see in retrospect what the important ideas were, so you can much more easily identify the important changes. 99.9% of papers published will have virtually no impact, and it can be close to impossible to identify the 0.1% that will in the day, week, or month that they're published. Hence, you can look at papers from 2+ years ago that you *know* really changed the field, identify how they changed the field, break down the forces that made those changes possible & the reasons those changes were advantageous, then use that knowledge to identify how the field will change *now*.

Chung then uses the evolution of the transformer as a case study, emphasizing how elements from the encoder-decoder transformer were gradually removed to arrive at the much more general decoder-only transformer with weaker inductive biases. In particular, he emphasizes that the decoder-only transformer now:
1. Treats input & target the same using shared parameters
2. Uses causal self-attention rather than all-to-all attention for the input (i.e. move from bidirectional attention to unidirectional)
3. Allows the input & target representations to interact at every layer, rather than only using the final layer representation of the input to interact with the target

All of these changes made the transformer architecture much more general and reflects the trend that Chung asserts is most important in AI - namely, that compute & data are increasing exponentially, and as these become more abundant, we should use fewer inductive biases in our models and allow them to learn the proper biases during training.
## Thoughts & Ideas
- Very useful framework and presentation. He leaves it while only addressing steps (1) and (2) in his framework (i.e. identifying and understanding the dominant driving forces behind change) without going to step (3) (i.e. predicting the future trajectory). Some areas where I think even *more* structure could be removed from transformers (i.e. make them have even fewer inductive biases so they can take better advantage of more data and more compute):
	- Predict *k* tokens in the future instead of just 1 token at a time, using concepts from the Medusa papers. This will allow the transformer weights to encode knowledge of how to plan multiple steps into the future, rather than one step at a time
	- Make tokenization a learnable part of the model, rather than a static vocabulary that is applied. We may be training models at the wrong level of granularity with the way current tokenization schemes break up words.
## Notes
- Starts by asserting that the pace of AI research is too fast to keep up with today - instead of trying to stay at the frontier (an impossible task), it makes more sense to look at the change/derivative and identify the forces driving that change
	- Here he makes an analogy to gravitation. Modeling a pen dropping in a room in full fidelity is impossibly hard because we need to do computational fluid dynamics on the air molecules. But this detail obscures the true nature of gravitation. If we abstract things away, we know the pen should follow a trajectory defined by $z(t) = \frac{1}{2}gt^2$ where $z(t)$ is the height of the pen at time $t$. The same needs to be done for AI - we ignore the myriad details and zero-in on the important aspects of what is changing (same way we focus on the important aspects of change in an object's position - namely, gravity).
- His framework for studying change is:
	1. Identify the dominant forces behind the change
	2. Understand the dominant driving forces
	3. Predict the future trajectory
- WHY does this work better than trying to just stay at the forefront of AI research? My thoughts: it's much easier to see in retrospect what the important ideas were, so you can much more easily identify the important changes. 99.9% of papers published will have virtually no impact, but it can be close to impossible to identify the 0.1% that will in the day/week/month that they're published. Hence, you can look at papers from 2+ years ago that really changed the field, identify how they changed the field, break down the forces that made those changes possible & the reasons those changes were advantageous, then use that knowledge to identify how the field will change *now*.
- He asserts that there is one main change driving everything in AI: more & cheaper compute (see below image, trend is above exponential since the chart is log-scale).
![[Pasted image 20240613183746.png]]
- The main way to take advantage of this trend of AI is by listening to the bitter lesson - as we get access to more compute and more data, we need to use more general models that have fewer/weaker inductive biases.
	- This notion is captured by the below image. If we have small amounts of compute & data, a highly-structured model works well (think linear regression or SVMs). As we get to more compute & data, a less structured model begins to outperform the highly-structured model (e.g. CNNs on images). Finally, as we scale up compute & data in the limit, much less structured models begin to dominate (e.g. vision transformers). With more data and compute available, the models can *learn* better biases than human programmers can encode by hand into the architecture.
![[Pasted image 20240613183910.png]]
- A key takeaway here is that - much of AI research should be focused on **removing parts that are no longer needed**, but it isn't! This is a failure of research, there should be more of a drive to make models increasingly general as data and compute becomes more available
- So with the above, we have identified the driving forces behind the change in AI models over the decade or two (point 1 in his approach to studying change). Now we move on to step 2 - understanding the dominant driving forces, which he does by studying the evolution from encoder-decoder transformers, to encoder-only transformers, and finally to decoder-only transformers.
- Summary of the differences between encoder-decoder and decoder-only:
![[Pasted image 20240613184532.png]]
- The additional structures in the encoder-decoder model compared to the decoder-only model are thus:
	- Having a separate encoder and decoder, which assumes that the input and target sequences are sufficiently different that they need different parameters to handle them
	- The decoder in an encoder-decoder transformer can only access the representation from the final layer of the encoder. This assumes that the low-level features learned by early layers of the encoder are not necessary for the decoder
	- All-to-all interaction among the input sequence elements is used in the encoder layer. This assumes that better representations are generated for earlier tokens by using later context
- Decoder-only transformer removes all of these assumptions:
	- Input & target are treated the same (sharing parameters in the causal self-attention matrices of the decoder-only transformer)
	- The target can references the input's encodings at all layers of a decoder-only transformer (i.e. in every single causal self-attention block, the output token is attending to the input representation *in that layer*)
	- Causal self-attention is used for the input, removing the all-to-all interactions. With sufficient data, bidirectionality does not help learning, while unidirectionality (causal attention) makes the engineering challenge for multi-turn chatbots much easier