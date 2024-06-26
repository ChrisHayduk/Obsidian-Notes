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
	- They haven't built this yet, focusing on separate modalities at the moment (built a protein language model and a genomics language model)
- Built a 100B protein language model
	- Interestingly, instead of just masked language modeling, they used masked language modeling + causal language modeling as the objective
		- This allows for both protein encoding and protein generation
			- Use reinforcement learning to optimize protein generation for specific objectives (e.g. function predictions)
	- Found that proteins with specific gene ontology labels cluster in embedding space without training on function information

## BioPlex 3D: Predicting Structures for Protein-Protein Interactions across the Human Interactome by Ed Huttlin

Website: https://bioplex.hms.harvard.edu/

- Understanding cellular functions at a systems level will require comprehensive knowledge of the interactome - the catalog of all interactions among human proteins
- BioPlex is a tool they used to view network interactions of human proteome (similar to how our knowledge graph presents protein-protein interactions)
	- This is an abstracted view - ignores protein structure, which governs function, including protein-protein interactions. Without structure, we don't know how or why the interactions in BioPlex are happening
	- Goal: incorporate structural information to deepen our understanding of each interaction and its associated biology
		- Obstacle: There are not enough structures in the PDB to map onto all known interactions (lose something like 90% of interactions if you filter out those that don't have a structure)
- To mitigate the above obstacle, they have been using AlphaFold to predict structures for all known protein-protein interactions in the human proteome
	- Critically, they use AlphaFold-multimer to predict how the protein complexes form as part of the interactions
- Use AlphaFold structures to explore the interactome at different scales
	- One interesting example: they look at the set of all proteins that interact with a particular protein and cluster the interacting proteins based on which pocket they bind to

## Multi-modal and Generative Models for Drug Design by Marinka Zitnik

- Three core topics:
	- Integration of cellular contexts with 3D protein structure
	- Multimodal protein prediction
	- Protein optimization

### Integration of cellular contexts with 3D protein structure

Paper link: https://www.biorxiv.org/content/10.1101/2023.07.18.549602v1

Some proteins' functions depend upon the cellular context that they exists within. How can we develop models that dynamically adjust their outputs to biological contexts in which they operate?

This group developed a model called PINNACLE that is a multi-scale graph neural network for precise protein representation learning and cell type- and state-specific prediction. PINNACLE integrates single cell transcriptomics data with a protein interaction network, celltype interaction network, and tissue hierarchy to generate protein representations with celltype resolution.

PINNACLE is a self-supervised model, optimizing to predict links between entities in the multi-scale graph (see below image). Results in MANY embeddings per protein, each tailored to a specific cell type. PINNACLE propagates message on proteins, celltypes, and tissues using attention mechanisms specific to each node and relationship type:
- The protein-level objective function, which considers self-supervised link prediction on the protein interactions and celltype-identity classification on the protein nodes, enables PINNACLE to produce an embedding space that captures both the topology of the celltype-specific protein interaction networks and the celltype identity of proteins.
- The celltype- and tissue-specific components in celltype- and tissue-specific objective functions are based solely on self-supervised link prediction to learn cellular and tissue organization.
- Such information is propagated to the protein representations using an attention bridge, imposing tissue and cellular organization to the protein representations.

PINNACLE enables contextualized, precise predictions of drug effects across cell types and cell states.

![[Pasted image 20240305122111.png]]
### Multimodal protein prediction

Paper links: 
- SIPT: https://www.nature.com/articles/s42256-023-00647-z
- Mulitmodal language model: 

Need to repurpose/change concepts from language modeling to customize them for biological sequences. There are opportunities for integrating biologically relevant information into large language models to make them more powerful.

Some examples:
- Structure-inducing pre-training (SIPT) allows users to specify the structure they want via a pre-training graph. Input to language model is both the pre-training graph and the protein sequences (see below image).
- Multimodal language model that takes as input both a sequence and a natural language, plain-text description of the sequence. They use gene ontology, PDB, etc. descriptions of function to create an instruction-tuning dataset that allows researchers to use natural language to specify protein function for generation.

![[Pasted image 20240305124012.png]]
### Protein optimization

Paper link: https://www.biorxiv.org/content/10.1101/2024.02.25.581968v1

Designing small-molecule-binding proteins, such as enzymes and biosensors, is crucial in protein biology and bioengineering. Generating high-fidelity protein pockets—areas where proteins interact with ligand molecules—is challenging due to complex interactions between ligand molecules and proteins, flexibility of ligand molecules and amino acid side chains, and complex sequence-structure dependencies. Here, the speaker introduces PocketGen, a deep generative method for generating the residue sequence and the full-atom structure within the protein pocket region that leverages sequence-structure consistency.

PocketGen consists of a bilevel graph transformer for structural encoding and a sequence refinement module that uses a protein language model (pLM) for sequence prediction. The bilevel graph transformer captures interactions at multiple granularities (atom-level and residue/ligand-level) and aspects (intra-protein and protein-ligand) with bilevel attention mechanisms. For sequence refinement, a structural adapter using cross-attention is integrated into a pLM to ensure structure-sequence consistency.

![[Pasted image 20240305124052.png]]
![[Pasted image 20240305124119.png]]
## EquiFold: Fast Antibody Structure Prediction for Drug Discovery and More by Jae Hyeon Lee

Paper link: https://www.biorxiv.org/content/10.1101/2022.10.07.511322v1

- Uses novel coarse-grained structure representation - which does **not** require MSA **or** language modeling. Makes it WAY faster than even ESMFold
	- AlphaFold2: represented by backbone frame and torsion angles
	- EquiFold: uses coarse-grained (CG) nodes that encompass a subset of atoms in each amino acid. The CG nodes are chosen such that 1) their union represents all atoms of the amino acid; 2) each member atom of a node shares at least one covalent bond with another member of the same node; and 3) each node consists of at least three atoms that collectively form a rigid body whose orientation in 3D is uniquely determined.
- Generation of realistic ensembles is important for various applications by expensive
	- Feedforward model trained with regression objective is optimized to produce a single conformation 
		- For example, if there are two conformations in the training set, the predicted conformation that minimizes the loss would be an average of the two conformations. But the "average" conformation **doesn't actually exist**
	- EquiFold proposes diffusion modeling as a solution (model called EquiFold-Diffusion)