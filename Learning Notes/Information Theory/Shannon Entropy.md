## Overview
The entropy of a random variable is the average level of "surprise" or "information" inherent to the variable's outcome. That is, as a distribution becomes more uncertain, the "surprise" or "information gained" from a random variable taking on any particular value becomes higher.

Mathematically for a random variable $X$ which takes values in the set $\mathcal{X}$ and is distributed according to $p: \mathcal{X} \to [0,1]$, the entropy is
$$H(X) := - \sum_{x \in \mathcal{X}} p(x) \log p(x)$$
where $\log$ is typically base 2 (giving us the amount of information in bits).

Looking at the term inside the summation ($- p(x) \log p(x)$) says is that values of $x$ that are very likely (i.e have very high probability) have very low entropy, because $p(x) \log p(x) \to 0$ as $p(x) \to 1.$ Similarly, values of $x$ that are highly unlikely unlikely (i.e. have very low probability) have very low entropy because $p(x) \log p(x) \to 0$ as $p(x) \to 0$. Thus, distributions with high certainty (i.e. most values of $x$ with near-zero probability and a few with near-one probability) will have very low entropy.

Now let's look at entropy for a uniform distribution. Suppose $X$ is random variable over the $\mathcal{X}$ with $|\mathcal{X}| = n$. Then,

$$
\begin{align*}
H(X) &= - \sum_{x \in \mathcal{X}} \frac{1}{n} \log \frac{1}{n}\\
&= - \frac{1}{n} \sum_{x \in \mathcal{X}} \log n^{-1}\\
&= \frac{1}{n} \sum_{x \in \mathcal{X}} \log n\\
&= \frac{1}{n} \cdot n \cdot \log n\\
&= \log n
\end{align*}
$$
Now we know $\log n \to \infty$ as $n \to \infty$, so our uniform distribution approaches an infinite number of possible variables we approach infinite entropy.
## Relationship to Information Content
Consider an event $E$ with probability $p(E)$. The amount of information that this event provides us with is given by $$I(E) =\log \left ( \frac{1}{p(E)} \right)$$
Thus, we approach infinite information as $p(E) \to 0$ (i.e. we are *extremely* surprised by the result) and approach 0 information as $p(E) \to 1$ (i.e. the outcome was a foregone conclusion).

Using properties of logs, we can reformulate the information content of $E$ as $$I(E) = - \log (p(E))$$
**Entropy is just the average of the information content over all possible events E**. Suppose we drew $E$ from a set of possible events $\mathcal{E}$. In this case, $I$ is a function of $\mathcal{E}$, and to find the average of a function of a discrete random variable, we take, $$\mathbb{E}(I(E)) = - \sum_{E \in \mathcal{E}} p(E) \log(p(E)) $$ where $\mathbb{E}$ is the expected value operator. This is exactly the formula for entropy provided previously.
## Connection to Thermodynamic Entropy

In the Overview section, we derived the entropy of a discrete uniform random variable with $n$ possible values, yielding $H(X) = \log n$. Now if we multiply this value of the Boltzmann constant $k_B$, we get $$k_B H(X) = k_B \log n$$
which **exactly** reflects the thermodynamic definition of entropy provided by Boltzmann. To restate how that is formulated, suppose that a system is in a fixed macroscopic state (e.g. with a fixed total energy $E$). Then it will have some number $W$ of microscopic states (which are configurations of the molecules making up the system, described by their individual positions and velocities) that potentially produce its fixed macroscopic state. We can then define the thermodynamic entropy $S$ as $$S = k_B \ln W$$
Hence, the Shannon entropy of a uniform random variable and the Boltzmann entropy of an isolated system at thermodynamic equilibrium are precisely the same up to multiplication by a constant (namely $k_B$).
### Why Do We Multiply by a Constant in Boltzmann Entropy?
$k_B$ has units of energy divided by temperature. Thus, it provides a link between temperature (a macroscopic quantity) and the energy of particles (a microscopic quantity). For example, in the context of an ideal gas, the thermal energy of a particle is proportional to the temperature, with the Boltzmann constant being the proportionality factor. Mathematically, the average kinetic energy of a particle in an ideal gas is given by $$ \langle E_{\text{kinetic}} \rangle = \frac{3}{2} k_B T$$
In multiplying $\ln W$ by $k_B$ in the definition of the Boltzmann energy $S$, we imbue the unit-less $\ln W$ with units of Joules per Kelvin, matching the original thermodynamic conception of entropy as proposed by Rudolf Clausius. Clausius's entropy is defined as the quotient of an infinitesimal amount of heat to the instantaneous temperature, given mathematically as $$dS = \frac{\partial Q_{\text{rev}}}{T}$$
So we *don't* multiply by a constant in Shannon Entropy because we're okay with it being unit-less. In Boltzmann Entropy, we need to align with the original conception of entropy as heat per temperature, and so multiplying by $k_B$ provides us with those units and rescales the values to match Clausius's Entropy.
## Connection to Data Compression
Suppose we have a stochastic process $X$ with countable indices (i.e. time is discrete). Then we can define a sequence of joint entropies $H_n(X_1, X_2, \ldots, X_n)$ for every $n \in \mathbb{N}$. If the limit exists, **the entropy rate** is defined as $$H(X) := \lim_{n \to \infty} \frac{1}{n} H_n \tag{1}$$
**Joint entropy** is a measure of uncertainty associated with a set of variables. Note that a joint Shannon entropy of two discrete random variables $X$ and $Y$ with images $\mathcal{X}$ and $\mathcal{Y}$ is given by $$H(X, Y) = - \sum_{x \in \mathcal{X}} \sum_{y \in \mathcal{Y}} P(x,y) \log_2 \left [ P(x,y) \right ]$$
where $x$ and $y$ are particular values of $X$ and $Y$ respectively, $P(x, y)$ is the joint probability of these values occurring together, and $P(x, y) \log_2 \left [ P(x,y) \right ]$ is defined to be 0 if $P(x,y) = 0$. For $n$ random variables, this expands to $$H(X_1, \ldots, X_n) = - \sum_{x_1 \in \mathcal{X}_1} \cdots \sum_{x_n \in \mathcal{X}_n} P(x_1, \ldots, x_n) \log_2 \left [ P(x_1, \ldots, x_n) \right ] \tag{2}$$
**Thus (1), the entropy rate of a data source, is the average number of bits per symbol needed to encode it.** Why? Because $H_n$ gives us the total number of bits of uncertainty for the *entire sequence* of length $n$ (given by equation (2)), where we consider each element of the sequence an event (and hence the length $n$ sequence is a set of events which we compute a joint probability over). We then divide by $n$, yielding the average bits per symbol for a sequence of length $n$. By performing a limiting process, we get the entropy rate for the data source (i.e. data generating process) itself.

For compression to be lossless, then the compressed message has the same quantity of information as the original message, but communicated in fewer characters. It there has more information (higher entropy) per character. Therefore, the entropy rate of the compressed message will be higher than that of the uncompressed message.
## Entropy of a Sequence

## Data as a Markov Process

## See Also
1. [[Kolmogorov Complexity]]
2. [[An Observation on Generalization]]