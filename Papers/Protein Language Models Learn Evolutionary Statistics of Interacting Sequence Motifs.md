**Link:** https://www.biorxiv.org/content/10.1101/2024.01.30.577970v1
**Authors:** Zhidian Zhang, Hannah K. Wayment-Steele, Garyk Brixi, Haobo Wang, Matteo Dal Peraro, Dorothee Kern, Sergey Ovchinnikov
## Summary

The authors proposed three hypotheses for how protein language models learn to enable highly accurate protein structure prediction. These are:
1. **ESM-2 truly has learned protein folding physics**
2. **ESM-2 is performing "full fold lookup"** - that is, it has memorized a collections of folds and, given an entire protein sequence, it matches contact predictions to a particular fold class
3. **ESM-2 has memorized motifs represented as pairs of fragments**

![[Pasted image 20240227224005.png]]

The authors ruled out hypothesis 1 due to many non-physical folding conformations that ESMFold predicted, with a high number of exposed hydrophobic molecules. Hypothesis 2 was not fully ruled out, but the authors landed on Hypothesis 3 as the most likely answer. 

They did this by extracting a categorical Jacobian matrix as follows. They mutated each residue in the length $L$ sequence to each of $A$ possible amino acid tokens and calculated how each of these $L \times A$ mutations perturbs the probabilities of each amino acid across all positions output by the language model, i.e. the logits. Since there are $L$ positions in the sequence and the logits are values predicted for each of the $A$ possible tokens, we have that each one of the $L \times A$ mutations has a corresponding output of size $L \times A$ (the logits at each position of the sequence for that proposed mutation). Accordingly, the shape of the categorical Jacobian $\mathbf{J}$ is $L \times A \times L \times A$.

The authors used the Jacobian tensor to calculate a predicted contact map of size $L \times L$ (i.e. which amino acids are in contact with which other amino acids in the output structure). Critically, this prediction is made *without* folding the protein.

The contact matrix from the categorical Jacobian is calculated using the below set up with $J$ in place of $W$:

![[Pasted image 20240227225836.png]]

The categorical Jacobian out-performed standard linear models in performing contact prediction and performed similarly to the dedicated contact prediction head in ESM-2, showing that the weights of ESM-2 directly encode some co-evolutionary information.

To further verify that ESM-2 was looking up local motifs (instead of full folding structures), the authors masked a large number of amino acids in the sequences and then examined contact prediction performance as they unmasked amino acids with one of a few patterns:
1. Unmasking amino acids flanking the contact point
2. Unmasking random amino acids
3. Unmaking random amino acids, avoiding the nearest 30 amino acids to the contact point

They hypothesized that a model that had memorized full folds would be able to recover contacts better by randomly unmasking amino acids (since this on average would reveal enough information to recover the full structure faster), while a model that had memorized local motifs would better be able to recover contacts by gradually unmasking residues next to the contact in question (thus revealing the motif that the contact is a part of more quickly).

We can see below that random and random while avoiding the nearest 30 amino acids approaches (the magenta and light blue lines) perform significantly worse than the flanking approach, indicating that ESM-2 likely memorizes local motifs.

![[Pasted image 20240227230538.png]]
## Thoughts & Ideas

1. The authors mention that "A downside of such compression is that within-family evolutionary effects such as multiple stable conformations are inaccurately predicted by ESM-2, a clear area for future improvement." How could we improve that while still keeping a language mode base?
2. Interestingly, the authors assert that "Storing the co-evolutionary statistics of all known protein families in UniProt (roughly 20,000), assuming an average length of 256, would require 261 billion parameters." Could it be possible that if ESM-2 was scaled up significantly, we would see some emergent capabilities as it is able to memorize this amount of co-evolutionary data?