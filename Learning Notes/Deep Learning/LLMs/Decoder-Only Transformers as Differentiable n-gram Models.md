## $n$-gram Model Basics

[$n$-gram models](https://en.wikipedia.org/wiki/Word_n-gram_language_model) use the previous $n$ tokens to make a prediction for token $n+1$. Typically, a conditional probability distribution is created by looking at all occurrences of a particular $n$-gram and counting the tokens that follow it, then dividing by the total number of occurrences of that $n$-gram. In this way, we have a conditional probability distribution for every $n$-gram that appears in our dataset. Below is an example of calculating the probability of the sentence "I saw the read house" using a bigram model.

![[Pasted image 20240227215748.png]]

Formally, this looks like:

![[Pasted image 20240227223331.png]]

where the conditional probability can be calculated as:

![[Pasted image 20240227223411.png]]
## Transformer Model Basics

The base of the transformer model is the attention mechanism, pictured below.

![[Pasted image 20240227214848.png]]

Intuitively, attention can be understood as a continuous key-value store. The attention operation is performed by taking an $n \times d$ query matrix $Q$ and multiplying it by the transpose of an $n \times d$ key matrix $K$. In this case, $n$ is the max number of tokens in a sequence and $d$ is the dimension of the embedding vector for each token. 

This gives a dot product of every query with every key. Query vectors that point in the same (or similar) direction as key vectors result in larger dot product magnitudes, which gives us a measure of *similarity* between each query and every possible key. We finally apply $\texttt{softmax}$ so that a single key is selected most prominently from the set of keys. (Since we use $\texttt{softmax}$ instead of $\texttt{argmax}$, the transformer's key selection step remains differentiable.) 


![[Pasted image 20240227214635.png]]

The output of the above step is the **attention matrix**. The attention matrix is essentially a probability distribution over the keys for each query in our input sequence. By multiplying the attention matrix with the value matrix, we obtain a matrix of **weighted sums of all the values**, where row $i$ in the resulting matrix is the weighted sum vector for index $i$ in the input sequence, weighted by index $i$'s query similarity with the key matrix.
### Where do Q, K, V come from?

It is important to note that the query, key, and value matrices are **projections of the input itself** (see the below image). That is, the fully connected layer is responsible for perturbing each input token's embedding into a space that makes it usable for the attention operation. In particular, it is important to note that these fully connected layers **are NOT followed by nonlinearities**! This means that we are not "transforming" or "deforming" the space that these embedding vectors are living in. Instead, we're applying a linear transformation with the same dimension as the input space, which in some sense is simply shifting them around in embedding space. Applying a linear transformation can effect the following changes to the input space:
1. **Scaling**: Each axis of the input space can be stretched or compressed.
2. **Rotation**: The input space can be rotated about the origin.
3. **Shearing**: Axes of the input space can be skewed, changing the angle between them without altering their scaling.
4. **Reflection**: The input space can be mirrored across a line in two dimensions or a plane in higher dimensions.

![[Pasted image 20240227215510.png]]

It is also important to note that the embeddings for each token are a kind of representation of that token **embedded in its context**. That is, the embeddings are context-aware and differ based on the token's surrounding words and its position in the sequence.
## Decoder-Only Transformers as Big n-gram Models

So, given the above discussion, if our value (V) matrix projection is learned well, an intuitive interpretation of what it is doing is **shifting each vector embedding in our input sequence to a position that helps us to find our next continuation in the embedding space**. 

In particular, since a fully-connected layer without a non-linearity can be interpreted as a general linear model, the output $\vec{y}$ for a given input $\vec{x}$ is actually interpreted to be the conditional mean of outputs given $\vec{x}$, $\mathop{\mathbb{E}} (\vec{y} | \vec{x})$, under some assumptions of linearity. 

$\vec{y}$ in this case can be thought of as the vector representation, starting from $\vec{x}$, that contributes **most** to finding the $n+1$st vector that will continue our length $n$ sequence **given** the other $n-1$ vectors in the sequence. 

A linear layer at the end then uses these refined $n$ vector embeddings to create a probability distribution over the $k$ tokens in our vocabulary. The fully connected layer responsible for output, rather than acting elementwise like the fully connected layers we discussed previously that are internal to the transformer, actually compute a sort of weighted average of the $n$ vector embeddings to output a value for each of the $k$ vocabulary tokens. We then use $\texttt{softmax}$ to turn this output into a distribution that can be sampled from. 

But this is **precisely** the definition of what an $n$-gram model would predict, except this time we are using a continuous representation of the $n$ input tokens rather than using the discrete tokens themselves.
## Next Steps: Do Some Empirical Testing

Could be very interesting to look at how the vector projections of each token evolve over the course of the forward pass. ie. for token $i$ in the sequence, at each layer, map embedding $i$ in the value matrix back to its closest token and see the value