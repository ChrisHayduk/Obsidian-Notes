**Link:** https://www.youtube.com/watch?v=AKMuA_TVz3A
**Speakers:** Ilya Sutskever
## Summary
In this talk, Ilya starts from a central problem in machine learning: we have strong theoretical guarantees on the performance of supervised learning algorithms, but a comparative sparsity of theoretical results related to unsupervised learning. One contributor to the difficulty of theoretical results is that, quite often, we don't actually *care* about the unsupervised learning objective itself, but are instead using it as a proxy for some other downstream task that does not have a large enough dataset to train from scratch. An example of this is using a large transformer trained with BERT loss to then classify articles into one of a number of custom-defined categories. Ilya sidesteps this issue by reformulating unsupervised learning not as a learning problem, but as a compression problem. In particular, he asserts that all good unsupervised predictors must be good compressors, and all good compressors must be good unsupervised predictors. He then uses results from Kolmogorov complexity to derive some results relating to unsupervised models, asserting that Kolmogorov complexity provides an upper-bound for unsupervised learning performance (i.e. the Kolmogorov compressor would also be the best unsupervised learning model possible). He also provides an equation to relate conditional compression (i.e. supervised learning) to joint compression (i.e. unsupervised learning), asserting that we can just "compress" (or predict) everything and arrive at similarly performant model.
## Thoughts & Ideas
- What does it mean for one compressor to be *better* than another? There seem to be two metrics along which we could classify neural networks as compressors. Which is better and how should they be weighed?:
	- Size (i.e. GBs on disk or number of parameters)
	- Compression loss (i.e. error on test distribution)
- If this analogy between unsupervised learning and compression holds (there are many unverified claims in this talk to work through first), are there any results from information theory that can be used to improve or better understand unsupervised learning?
- In the talk, Ilya makes the claim that autoregression is better than BERT because the task is harder. This to me implies that for equivalent performance (i.e. test error) on a given dataset, a model trained using AR will need to either be trained for longer or be larger than a BERT model on the same dataset. It may be interesting then to train a much larger protein language model using autoregressive loss and train it for much longer. (e.g. something on the order of 100B parameters)
## Notes
- Statistical learning theory (concerning supervised learning) gives conditions under which learning *must* succeed:
	- Low training error + more training data than "degrees of freedom" (i.e. parameters) = low test error
- (**NB: The following is a result for supervised learning)** In the below image, $D$ is the test set, $S$ is the training set, $\mathcal{F}$ is the set of all function, $f$ is a particular function, and $t$ some threshold for the difference in error between the test & training sets (i.e. how much higher is the error on the test set than the train set). The result then states that the probability that there is *at least one* $f \in \mathcal{F}$ where the difference in test and train set errors is $> t$ is less than or equal to the cardinality (size) of $\mathcal{F}$ **divided by** $e^{|S| t^2}$. That is, we divide by $e$ raised to the power of the size of the training set times the square of the desired threshold. So, we can decrease the probability that there is any $f \in \mathcal{F}$ with the difference in train and test errors greater than $t$ in two ways: increase $t$ (i.e. be more relaxed with how well the function fits the test set in comparison to the train set) **or** increase the size of the training set $S$. This result shows that as we increase the size of the train set to infinity, the error on the test set and training set will converge to the same value.
	![[Pasted image 20240605133906.png]]
	- Important piece here is that: **training distribution and test distribution must be the same!** If they are then we know that performing well on the training set + having a very large training set will lead to very low test error
- Unsupervised learning traditionally has not had as "nice" performance guarantees
- Big obstacle in unsupervised learning theory: you optimize one objective (e.g. reconstruction loss in BERT) but actually care about *another* objective (e.g. sentiment prediction or text classification)
	- Gives us no reason to expect theoretically for this to work
	- Ilya presents a mathematical framework that may suggest why it actually works
- Ilya likes distribution matching as a metaphor for unsupervised learning: we have input data $X$, an output distribution $Y$, and want to learn a function $F$ such that $F(X) \sim Y$ (i.e. $F(X)$ has the same distribution as $Y$)
- Framework rests on *compression*
	- **Investigate this**: Ilya claims there is one-to-one correspondence between compressors and predictors on a dataset (i.e. every compressor is a predictor and every predictor is a compressor)
	- Given two datasets $X$ and $Y$ and compress them jointly (i.e. concatenate them) with a compressor $C$. 
		- **Claim from Ilya (verify this):** A good compressor will use pattern in $X$ to help compress $Y$
	- **Claim from Ilya (verify this):**  $|C(\text{concat}(X, Y))| < |C(X)| + |C(Y)| + O(1)$. That is, the length of the compressor of the joint distribution of $X$ and $Y$ is less than the sum of the lengths of the compressor of $X$ and the compressor of $Y$ plus some constant. 
		- **My claim (prove this):** The worst case is when $X$ and $Y$ are totally uncorrelated (i.e. one gives no information about the other), in which case $|C(\text{concat}(X, Y))|$ should probably equal $|C(X)| + |C(Y)|$ 
		- **Claim from Ilya (verify this):** The gap in $|C(\text{concat}(X, Y))|$ and $|C(X)| + |C(Y)| + O(1)$ is the mutual information of $X$ and $Y$, i.e. their "shared structure". The larger the gap is between these two values, the more shared structure $X$ and $Y$ have.
		- **Claim from Ilya (verify this):** If there exists an $F$ such that $F(X) \sim Y$ (i.e. they have the same distribution), then a good compressor of the joint distribution $X, Y$ will notice and exploit this
- Formalization of framework (he uses the words compression and prediction interchangeably here):
	- Consider an ML algorithm $A$ that tries to compress $Y$ and has access to $X$ (i.e. it can use $X$ to help compress $Y$). Our "regret" of using this algorithm is the amount of "value" we left on the table when using $X$ to compress $Y$. That is, it is the amount of structure in $X$ that *could have been used* to compress $Y$ but wasn't.
	- [Kolmogorov complexity](https://en.wikipedia.org/wiki/Kolmogorov_complexity) $K(X) = \text{length of the shortest program that can output } X$. If $C$ is a computable compressor, then for all $X$, $K(X) < |C(X)| + K(C) + O(1)$ . That is, the Kolmogorov complexity (i.e. shortest program that can output $X$) is less than the length of the compressor $C$ *plus* the shortest program that can simulate a machine that runs $C$ *plus* a constant $O(1)$
	- [The Kolmogorov complexity is uncomputable](https://en.wikipedia.org/wiki/Kolmogorov_complexity#Uncomputability_of_Kolmogorov_complexity). In other words, there is no program which takes any string $s$ as an input and produces the integer $K(s)$ as an output. A short sketch of the proof is: assume for contradiction that there *is* a function $F$ of finite length that can compute the Kolmogorov complexity for any string $s$. Then use this function $F$ nested inside another function $G$ that searches for and outputs a program with Kolmogorov complexity greater than $K(G)$. But if the program it outputs has Kolmogorov complexity greater than $K(G)$, by definition of Kolmogorov Complexity it cannot be output by our function $G$, a contradiction.
	- **Claim from Ilya (verify this):** A deep NN / transformer is a massively parallel transformer that has a large amount of resources. NNs can simulate programs and SGD searches over programs/circuits, producing a miniature Kolmogorov compressor
	- **Claim from Ilya (verify this):** Conditional Kolmogorov complexity is the solution to unsupervised learning because it provides a lower-bound on the length of the program needed to compress $Y$ while using $X$:
		- If $C$ is a computable compressor, then for all $x$ we have $K(Y|X) < |C(Y|X)| + K(C) + O(1)$. 
		- **Claim from Ilya (verify this):** "Just compress everything" also works (i.e. compress joint distribution instead of conditional distribution):
			- $K(X, Y) = K(X) + K(Y|X) + O(\log(K(X,Y)))$
				- Can think of conditional Kolmogorov compressor as supervised learning where $X$ is used to "predict" (compress) $Y$ and joint Kolmogorov compressor as unsupervised learning over the dataset of $X$ concatenated with $Y$. So you can throw everything into the compression and pay the "cost" of $K(X) + O(\log(K(X,Y)))$ on the task of predicting/compressing both $Y$ and $X$ jointly when compared to $K(Y|X)$
	- **Claim from Ilya (verify this):** Joint compression is just maximum likelihood:
		- If $X = x_1, \ldots, x_N$ is a dataset, then $\sum_i - \log P(x_i | \theta)$ is the cost of compressing the dataset. Trivial to add some other dataset to the sum (as in the joint compression setup). And SGD over big NNs is our program search to find the compressor
- **Claim from Ilya (verify this):** Autoregressive loss is better than BERT loss for scaled-up models because it's a much harder prediction task. You need to use long-scale context to make a prediction on the next token, whereas for BERT many times the future tokens that it can peek ahead at will reveal the answer much more easily
## See Also
1. [[Kolmogorov Complexity]]
2. [[Shannon Entropy]]