Link: https://www.youtube.com/watch?v=0nA5QG3087g
Speaker: Ben Clavié
## Summary
Clavié asserts that RAG should not be thought of as a new paradigm. Instead, we should consider it a stitching together of information retrieval (very old) with LLM generation (new). He then focuses on the information retrieval portion of RAG pipelines, asserting that the baseline RAG method of computing cosine similarities is highly flawed and frequently produces sub-par results. In order to better bring in better results, he says that several things must be added to the standard RAG pipeline:
1. Leveraging metadata in your searches (e.g. extracting entities, dates, & categories from documents and tagging them to those documents so that they can be filtered at query time). We then filter documents using the metadata before doing any other types of similarity searches
2. Keyword search methods such as BM25 powered by tf-idf. These are extremely performant and cheap to compute.
3. Combining ranking of the keyword search method & the cosine similarity method (can take a weighted sum of the rankings from the two approaches)
4. Take top $k$ most similar documents from previous step and re-rank them using a larger model

	![[Pasted image 20240619142822.png]]
## Thoughts & Ideas
- Metadata tagging is very interesting. Slots in fairly nicely with the approach of tagging document nodes to their extracted entities & relationships in a graph database
- How can we apply these methods to non-text RAG (e.g. proteins and chemicals)? MSA methods appear to be too slow, so is there anything like keyword search that can be done?
## Notes
- RAG is *not* a new paradigm. It is the stitching together of information retrieval (which has been studied extensively since the 1970s) and LLM-powered generation. We can improve our performance by looking at different methods for retrieval (and these retrieval methods do not necessarily need to be vector-based!)
- Good RAG consists of three main components:
	- Good retrieval pipeline
	- Good generation pipeline
	- Good way of linking them together
- Basic MVP RAG:
	- Select embedding model (e.g. BERT)
	- Embed documents into vectors using model
	- Store vectors in numpy array in memory
	- Embed user query using same embedding model
	- Calculate cosine similarity between embedded user query and all documents in the numpy array
	- Return top $k$ most similar documents
- Clavié refers to standard RAG as "bi-encoders" because we first encode the document in chunks and then pool the chunked embeddings to generate a document embedding. 
- Basic MVP RAG makes things very computationally efficient (i.e. we compute the embeddings for the full set of documents once and don't need to re-compute them for each query), but trades off some retrieval performance
![[Pasted image 20240619141430.png]]
- Cross-encoders predict a similarity score taking in both the document & query together. That is, they compute query-aware document representations. This is highly-computationally demanding (need to re-compute the vectors for every new query) but much more accurate
![[Pasted image 20240619141458.png]]
- Re-ranking: retrieve a larger number of documents using a fast approach like vector search. Then provide those documents to a large, powerful model that ranks the retrieved documents
![[Pasted image 20240619141338.png]]
- What are the limitations of using embeddings for retrieval?
	- Compressing the representation of each token in a sequence down into a single vector. E.g. for a chunk with 1000 tokens, the vector for that chunk will be some averaging over 1000 distinct tokens
	- The tokens in the dataset inform how the embeddings distribute themselves in space. If your users' questions are very different than the model's training distribution (and/or question-answer pairs are never seen in the training distribution), you will not get good results
		- Embeddings contain information **that is useful for the model to reproduce the training data!** Nothing more.
- A good mitigation of the above limitations is to **include keyword search in RAG pipelines**:
	- BM25 powered by tf-idf is the baseline for keyword search
	- Extremely powerful and very low compute requirements so it's basically a free lunch in RAG pipelines
	![[Pasted image 20240619142318.png]]
- Also important to **use metadata in RAG pipelines**:
	- E.g. If we ask: "Summarize financial reports for the cruise division in Q4 2024", the vector representation for this query must accurately contain information regarding "financial reports", "cruise division", and "Q4 2024". In reality, we will likely find documents with cosine similarity that do not match one or more of these criteria.
	- Can use entity recognition models to **tag documents with the entities they contain**. At query time, this metadata can be used to filter out any irrelevant documents
	![[Pasted image 20240619142822.png]]
	![[Pasted image 20240619142944.png]]
