---
title: Guide to a build a completely local RAG pipeline
layout: default
tags: [rag]
pinned: true
blog_post: true
---

# Guide to build a completely local RAG pipeline

I have been working with RAG for a few time now and there are lots of discussions about if it is possible to have a completely offline/local RAG pipeline. Is it possible to have your "personal google service". And I believe YES, it is possible. 

### But how do I check if it is offline?
Its very simple, just look for any model which doesn't demand any API key, or the model which are publicly available to clone it as a git repository. 

Basically to build a complete local RAG pipeline, there are two most important things that you need to take care of.   

1) download the best possible embedding model out there.   

2) download the best possible llm model out there.  

Now the point number 2 depends on what kind of methods are you using for your RAG approach, if you are using the retreival method then you will need a local llm server for your inference. And if you are using direct generation from RAG then you still need the llm server, but the latter will be computationally less expensive.    

Always keep an eye out on HuggingFace [Mteb Leaderboard](https://huggingface.co/spaces/mteb/leaderboard) to see what embedding models are outperforming right now.    

Also keep an eye on [Open Source LLM leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) and you can see the current best local models depending on your requirement.  

### Local Embedding Models  
There are publicly available open source embedding models that you can clone it for eg. the one which I worked with is [
all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2/tree/main) from hugging face. you can simply clone the repository from hugging face.   
 ```bash
 git clone https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2/tree/main

 ```

 And then you can use it in your application. 

 ```python
 from langchain_huggingface import HuggingFaceEmbeddings
 embed_model_path = "path/all-MiniLM-L6-v2"
 embed_model = HuggingFaceEmbeddings(model_name=embed_model_path)

 ```

### Vector Database  
Vector database are the places where your embeddings are saved. And thus when user asks a query, the query is converted into embeddings and they are semantically searched into these database. It is very important to select the right kind of Vector database as your search algorithm depends on that. However there are various methods by which you can increase your semantic relevance of query and embeddings.   

[ChromaDB](https://docs.trychroma.com/) is one of VectorDatabase out there which I am using currently. If you are into hybrid search, you can set up your own retreivers. Or you can use [Weviate](https://weaviate.io/) which by default uses hybrid search retreival strategies.  


### Retreival strategies  
Now you have downloaded your embedding model and set up your vector database. It is now time to figure out which strategies you want to use improve your search results. There are various Advanced-RAG techniques which you can use to improve search results.   

1) **Hybrid Search**  
There are two kinds of searching strategies, semantic search and keyword search. Keyword search as the name suggests searches by a particular keyword. Semantic search take into account the [cosine similarity](https://cohere.com/blog/what-is-similarity-between-sentences) between the query and retreival results.

The first strategy I would recommend it to use Hybrid searching methods, simply use a [BM25 retreiver](https://python.langchain.com/v0.2/docs/integrations/retrievers/bm25/) as an [ensemble retreiver](https://python.langchain.com/v0.1/docs/modules/data_connection/retrievers/ensemble/), this will improve your search results atleast by 50%.   

2) **Maximum Marginal Relevance (MMR)**  

Another retreival strategy I would suggest is to use **maximul marginal relevance (mmr)**. In the above paragraph we talked about getting relevant documents, now these relevant documents are calculated on the basis of query and the data. mmr is the method that takes into account the similarity between the retreived results. If you are using langchain, then you can use the similarity_type to "mmr" and set the fetch_k parameter.

```python
retriever = db.as_retriever(search_type="mmr", search_kwargs={"k": 5, "fetch_k":20})
```

fetch_k parameter will look up into 20 different texts and compare the relevance between the texts. Hence by using MMR method you will get a wide variety of diverse documents. If your query topic is mentioned at multiple places in your dataset then MMR method can help you to look up and return most relevant and sparse datasets. This will help you to avoid getting duplicate/same relevant texts again and again.  


3) **Split method**  
This is one of the most underrated but important method that I found out while building my own RAG pipeline. Before you convert your documents into embeddings, you basically split them in tokens, characters or headers. There are various kind of split methods available online.  

The two I found out most useful were [**RecursiveCharacterTextSplitter**](https://python.langchain.com/v0.1/docs/modules/data_connection/document_transformers/) and [**MarkdownHeaderTextSplitter**](https://python.langchain.com/v0.1/docs/modules/data_connection/document_transformers/). I was mostly working with markdown files so it was very important for me that the splitted documents end up in their parent context and not anywhere else.    

MarkdownHeaderTextSplitter splits each documents from headers. Hence you can imagine as the contents of header 3 (described by 3#) will always stay inside the context of header 2(described by 2#). This kind of contextual binding led my splits to stay in a formatted contextual manner.

