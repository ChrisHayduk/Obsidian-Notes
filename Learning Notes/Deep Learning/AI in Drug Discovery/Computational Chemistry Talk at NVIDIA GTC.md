Date: 03/20/2024
## Geometric Deep Learning in QM
- Use neural networks to provide estimation of the time-invariant Schrodinger equation
	- Allows us to provide almost linear time approximations of very expensive physical simulations (since neural networks are nearly linear in the system size N)
- Team contributed ANI Deep Neural Network
	- Approximates DFT equation
		- DFT equation takes 52 core hours, neural net approximation takes 0.3 GPU seconds
- Molecular representation: uses 5 angstrom radius balls to encode and compute local energies for each part of the molecule. The total energy is then the sum of the these regions

## Structure-Based Drug Discovery: Protein-Ligand Interactions
Links: 
- Exploring the frontiers of chemistry with a general reactive machine learning potential -[https://chemrxiv.org/engage/chemrxiv/article-details/6438627a73c6563f14cedeb3](https://chemrxiv.org/engage/chemrxiv/article-details/6438627a73c6563f14cedeb3)
- AIMNet2: A Neural Network Potential to Meet your Neutral, Charged, Organic, and Elemental-Organic Needs - https://chemrxiv.org/engage/chemrxiv/article-details/6525b39e8bab5d2055123f75

