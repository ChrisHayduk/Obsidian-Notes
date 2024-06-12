## Fuzzy String Matching Basics

[Fuzzy string matching](https://en.wikipedia.org/wiki/Approximate_string_matching) is the technique of finding strings that match a pattern approximately (rather than exactly). The closeness of a match is measured in terms of the number of primitive operations necessary to convert the string into an exact match. This number is called the edit distance between the string and the pattern. The usual primitives are:
- Insertion
- Deletion
- Substitution
## Encoder-Only Transformer Basics

Encoder-only transformers like BERT take the full transformer model and remove the decoder layers (by contrast with decoder-only transformers like ChatGPT, which remove the encoder layer). This means that the output of an encoder-only transformer is actually a sequence with the same length as the input - that is, we input a sequence of tokens with length $k$ and get a $k \times d$ matrix as output, where each token has been replaced by a refined $d$ dimensional vector representation.

![[Pasted image 20240229001048.png]]
### Masked Language Modeling Objective

Encoder-only transformers are typically trained with a masked language modeling objective. What this means is that, with a given input sequence, we will randomly hide tokens from the model and make the model guess what token should go there (see below image). To do this, we had a masked language modeling head on top of the transformer encoder, which predicts logits for each token in the sequence (i.e. a prediction of which token in our vocabulary is MOST likely at each position in the input sequence). For the un-masked values, we don't care about this prediction because the model can "see" what tokens are supposed to go there. The prediction matters for the MASK tokens - here the model learns how to use the surrounding words in the context window to predict what the missing word is.

![[Pasted image 20240229001533.png]]

The masked language modeling (MLM) loss function is given below, where $M$ is the set of masked tokens in the input, $x_i$ is the correct token to complete index $i$ in the input, and $z_i$ is the vector representation of token $i$. We want to minimize the negative log probability of the correct token completion (i.e. maximize its probability). 

![[Pasted image 20240229005307.png]]

Fundamentally what's happening here is that the representations of the seen tokens in the sequence are refined and aggregated in such a way as to produce accurate predictions for the MASK token. In a sense, they are giving us a way to "lookup" the value of that token in the embedding space.
## Encoder-Only Transformer and Fuzzy String Matching

This "look up" that the encoder-only transformer is doing can be thought about in a similar vein to fuzzy string matching. The model weights encode some reshaping of the embedding space *based on the input data that the model has seen*. That is, they are a compressed form of the training data, where the compressed representation is designed specifically for the MLM objective. When we pass an input sequence at inference time through the weights, we are essentially performing a *lookup* against this compressed form of the training data, where the representation of our input sequence will be close to the most similar training examples the model saw.