
Date: 03/05/2024
## AlphaFold Multimer for Interaction Prediction by Ernst Schmid

Website: https://predictomes.org/

- Created method to measure confidence of protein-protein interactions by:
	- Finding interacting residue pairs between the two proteins (where "interacting" is residues that are close together)
	- Looking at confidence (pLDDT) of the individual interacting residues
	- Finding the estimated relative distance error of the pair of interacting residues (i.e. how confident is AlphaFold in the distance between these two residues)

We can make predictions of protein-protein interactions by looking at an overall score of AlphaFold's "confidence" in the residue interactions using the above methods. Sometimes AlphaFold will just place residues of two different chains in a multimer near each other arbitrarily, so it is important to distinguish between arbitrary placements and asserted interactions.

Below is a selection from their matrix looking at proteins involved in human genome maintenance. Each cell in the matrix represents a score of the interactions between proteins $i$ and $j$ for some indices $i$ and $j$ in the set. Colored squares in the matrix represent predictions in which at least one pair of interfacial residues is closer than 8Å and meets minimum confidence criteria (PAE < 15 and pLDDT > 50).

![[Pasted image 20240305110428.png]]

Also interesting part of the talk - he created an easy-to-use interface for AlphaFold structure prediction. Allows users to submit jobs using a UI which get sent to private cloud GPUs. After the job finishes, it runs through an analysis pipeline that checks the protein-protein interaction confidence, and then finally saves the results to Dropbox.

## Foundational AI Models for Biological Systems by Le Song

Company Website: https://www.biomap.com/

- Biology as a multiscale, multimodal information system
	- Multiple different types of signals between systems
	- Network effects of interacting/cascading/inhibiting signals
	- Multiple relevant scales (e.g. organism, cell, molecule, etc.)
- High dimensional space in biology but small data (especially labeled data)
- Idea is to mitigate the lack of labeled data and small per-modality data by dumping everything into a transformer trained using self-supervision. 
	- In order of lowest scale to highest scale:
		- Genomes
		- RNA sequences
		- RNA structures
		- Protein sequences
		- Protein structures
		- Interactions
		- Cells
		- Molecular perturbations
		- Spatial data
	- They haven't built this yet, focusing on separate modalities at the moment (built a protein language model and a gene language model)
- Built a 100B protein language model
	- Interestingly, instead of just masked language modeling, they used masked language modeling + causal language modeling as the objective
		- This allows for both protein encoding and protein generation
			- Use reinforcement learning to optimize protein generation for specific objectives (e.g. function predictions)
	- Found that proteins with specific gene ontology labels cluster in embedding space without training on function information