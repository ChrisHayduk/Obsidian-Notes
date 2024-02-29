## What is MSA?

[Multiple sequence alignment](https://en.wikipedia.org/wiki/Multiple_sequence_alignment) (MSA) refers to the process or the result of sequence alignment of three or more biological sequences, generally protein, DNA, or RNA. In many cases, the input set of query sequences are assumed to have an evolutionary relationship by which they share a linkage and are descended from a common ancestor. From the resulting MSA, sequence homology can be inferred and phylogenetic analysis can be conducted to assess the sequences' shared evolutionary origins. Visual depictions of the alignment (as in the image below) illustrate mutation events such as point mutations (single amino acid or nucleotide changes) that appear as differing characters in a single alignment column, and insertion or deletion mutations (indels or gaps) that appear as hyphens in one or more of the sequences in the alignment. Multiple sequence alignment is often used to assess sequence conservation of protein domains, tertiary and secondary structures, and even individual amino acids or nucleotides.

The MSA databases used by AlphaFold to identify evolutionarily similar sequences to the target sequence are:
- MGnify
- UniRef90
- Uniclust30
- BFD

![[Pasted image 20240228170146.png]]

## MSA Input to AlphaFold's Evoformer Module

![[Pasted image 20240228131146.png]]

## MSA Row-wise Gated Self-Attention

Row-wise attention builds attentions weights for residue pairs within the same sequence and integrates the information from the pair representation as an additional bias term. The updated MSA representation matrix thus ensures that each sequence has a *contextual representation* for its residues - that is, for sequence $k$, the embedding of the residue at index $i$ takes into account information from the residues at indices $1, \ldots, i-1, i+1, \ldots, r$.

![[Pasted image 20240228131252.png]]

## MSA Row-wise Gated Self-Attention

Column-wise attention lets the elements that belong to the same target residue exchange information *across* sequences in the MSA. The updated MSA representation matrix thus ensures that each residue has a *cross-sequence representation* - that is, for the embedding for residue $i$ in sequence $k$ also takes into account information from residue $i$ in sequences $1, \ldots, k-1, k+1, \ldots, s$.

![[Pasted image 20240228131637.png]]

## MSA Transition


After row-wise and column-wise attention the MSA stack contains a 2-layer MLP as the transition layer. The intermediate number of channels expands the original number of channels by a factor of 4

![[Pasted image 20240228131904.png]]

## Integration of MSA with Pair Representation 

The “Outer product mean” block transforms the MSA representation into an update for the pair representation. 

![[Pasted image 20240228132207.png]]

In particular, this step grabs two vectors of representations for residues $i$ and $j$, where the vectors span the representations of all sequences included in the MSA. The [outer product](https://en.wikipedia.org/wiki/Outer_product) step creates a matrix of all dot product combinations. In the below image, you can think of $u_1$ as "sequence 1, residue i", $u_2$ as "sequence 2, residue i", and so on. Similarly, you can think of $v_1$ as "sequence 1, residue j", $v_2$ as "sequence 2, residue j", and so on. Since these dot products gives us a measure of the **similarity** between the representation of "sequence k, residue i" and "sequence m, residue j", we can think of it as a matrix that captures the pairwise similarities between all residues at positions $i$ and $j$ in the MSA.

![[Pasted image 20240228132419.png]]

This matrix ends up being of shape $(s, c, c)$, where $s$ is the number of sequences and each $c$ dimension denotes the number of features for residue $i$ and $j$'s representations, respectively. AlphaFold then takes a mean over the $s$ dimension of the matrix. What this means intuitively is that we average the pairwise similarity of residue $i$ and residue $j$ across *all possible pairs* of sequences in the MSA matrix. This collapses matrix from shape $(s, c, c)$ to shape $(c, c)$.

The last step projects the features to from $(c, c)$ to $c_z$. This allows them to be added to each entry in the pairwise representation.

## Conclusion - What is the MSA Representation Doing in AlphaFold?

So, putting this all together - the MSA steps compute a representation that optimally captures similarity of residues, both:
1. **Within sequences** by using row-wise attention to attend across amino acids inside a given sequence
2. **Across sequences** by using column-wise attention to attend across sequences for a given amino acid index

This representation is then used to generate a measure of similarity between all possible residue pairs in the MSA representation. We then update the pair representation of the target sequence by adding in these values. In essence, we use the MSA to "find out" which residues are similar to which other residues, and then add this information to the pair representation so that the structure module can guess at which residues are in contact with one another (based on the fact that they co-evolve and are therefore similar in the MSA representation).