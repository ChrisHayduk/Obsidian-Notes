
This note draws on the following notes:
1. [[MSA]] in AlphaFold
2. [[Encoder-Only Transformers with MLM Objective as Continuous Fuzzy String Matching]]
3. [[Protein Language Models Learn Evolutionary Statistics of Interacting Sequence Motifs]]

The key ideas are the following:
1. AlphaFold starts by searching genetic databases to identify evolutionarily similar proteins to the target protein. These clustered proteins are then put through a multiple sequence alignment (MSA). The MSA matrix is passed through both row-wise (looking across residues within a sequence) and column-wise (looking across sequence for a *specific* residue) to create representations the full-context into account. This updated matrix is then collapsed into pairwise similarity scores for all residues $i$ and $j$ in the target sequence, based on the sequences included in the MSA.
2. ESM-2, the language model components of ESMFold, uses an encoder-only transformer with the target sequence as input in place of AlphaFold's genetic search and MSA matrix. The MLM training objective of the encoder-only transformer forces the weights of the model to encode information about the amino acid embedding space that help it to predict MASK tokens. Thus, proteins that have similar completions at residue $k$ will have similar average encodings.
3. Using the above, we can think of ESM-2 (and by extension ESMFold), as using the encoding matrix to *look up* common motifs in proteins and base the structural folding on this motif look up.