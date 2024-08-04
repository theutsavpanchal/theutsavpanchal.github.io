---
title: What are Embedding Models in RAG ? 
layout: default
tags: [rag, transformers]
pinned: true
blog_post: true
---

## What are Embedding Models ?
Embedding models converts real world entities like word, sentence or paragraph into a vector space (basically numbers). A very interesting property of vector space is, similar words are closer to each other and dissimilar words are far from each other. Popular word embedding models like [Word2Vec and Glove](https://www.mygreatlearning.com/blog/word-embedding/) were the first models which recognized the semantic similarities and transform those words into a vector space. In those vector space, you can perform algebraic operations like addition and substraction.  A very popular example from the Word2Vec representation includes ["king - men + women = queen"](https://www.technologyreview.com/2015/09/17/166211/king-man-woman-queen-the-marvelous-mathematics-of-computational-linguistics/). In other words, adding the vectors associated with "king" and "women" and substracting the vector associated with "men", gives the similar vectors associated with "queen". 

<div style="text-align: center">
	<img style="max-width: 400px; text-align: center" src="/images/vec-space.png" />
</div>


## Embeddings vs Tokenizations 

### Tokenization
Tokenization is simply cutting down sentences into words or tokens. [Tokens can be thought of piece of words.](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them) Each token is represented by an integer. In some models, one token might represent one word while in some models it represents 3/4th of a word. [Open AI's](https://platform.openai.com/tokenizer) tokenizer platform shows how the input texts are converted into tokens. Some popular tokenization techniques include sub word tokenizers like Byte Pair Encoding (BPE), word-piece and sentence-piece. 

### Embeddings 
After a piece of text is tokenized, we then embed those tokens into vector representations. Embeddings are generally not static and they change during the training process. One token can be represented into 512 dimensions of embeddings.This is usually referred as dim_n or ndim. This can be explained by a simple figure below. 

<div style="text-align: center">
	<img style="max-width: 700px; text-align: center" src="/images/tokenization.png" />
</div>


Embedding vectors can be used in various domains. Some popular examples include. 
- Image Embeddings
- Video Embeddings
- Audio Embeddings 
- Graph Embeddings
- Shared Embeddings: For example CLIP by OpenAI which generates embeddings for both text and images.  

Some application of embedding models include. 
- LLMs: the input is tokenized and then converted into embeddings. 
- RAG: for efficient retrieval of similar documents. 
- Recommendations: Product or Content recommendation in a similar space. 


## Problems with Word2Vec and GloVE  
Word2Vec and GloVe laid the initial foundations to transform texts into semantic vector representation. These representations can be used to identify similarities in the texts. The problems with these models was they failed to identify the context/relevance of words in a sentence. Assume two sentences as described below. 
- sentence1: "The **bat** flew out of the cave at dusk."
- sentence2: "He swung the **bat** and hit a home run ." 

In both the above sentences, the word "bat" represents different meanings, however if you compute the similarity of the word "bat" in both of the sentences they turned out to be the same. 

<div style="text-align: center">
	<img style="max-width: 600px; text-align: center" src="/images/bat.png" />
</div>



## How Transfomers solved the Problem 
So the problem with Word2Vec and GloVE was contextualized word embeddings. In 2017, Transformers was introduced in the paper["Attention is all you need"](https://proceedings.neurips.cc/paper_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf). Transformers consists of two sections, an Encoder and a Decoder. The input to the encoder is words or tokens and its output is a sequence of continous represenatations, similarly the input to the decoder is sequence of representations and its output is words/tokens. 

<div style="text-align: center">
	<img style="max-width: 500px; text-align: center" src="/images/encdec.png" />
</div>

Even though transformers were introduced to solve language translation tasks, it solved the problem of contextualized word embeddings. Lets see how a translation tasks works.   

Encoder takes an input words/tokens and outputs a sequence of representations at a certain time stamp. In contrast, the decoder operates on one token at a time and considers the previous outputs and also the output of the encoder. In the below example, the decoder predicts the word "I" and this word is taken into the consideration in next time step with next encoder output token. Using both of them the decoder predicts the next token. Read on this amazing blogpost by [Jay Alammar](https://jalammar.github.io/illustrated-transformer/) on how Transformers works in depth.  

This results in the output tokens being the contextualized output tokens that we were looking for !

<div style="text-align: center">
	<img style="max-width: 600px; text-align: center" src="/images/transformer_decoding_2.gif" />
	 <figcaption style="margin-top: 10px; font-style: italic; color: gray;font-size: small;">
            How transformers works. 
        </figcaption>
</div>

These architectures are used in most of the technologies today. For eg. LLMs like GPT2, GPT3 uses a decoder only transformer-architecture while BERT uses Encoder only transformer-architecture is heavily used in sentence embedding models. 

## BERT
Bert stands for Bidirectional Encoder Representations from Transformers. It was built in two sizes. It was trained on 3.3B words and used in task specific fine tuning steps.  

| | Bert Base | Bert Large|
| Layers | 12 | 24|
|Hidden Units| 768 | 1024|
|Attention Heads| 12 | 16|
|Parameters| 110M | 340M|

Bert was fine tuned in two tasks.   

**1) Masked Language Modelling.**   
In this task, the input to the models was input words/tokens started with a special token [CLS] and ended with [SEP]. 15 % of those input words/tokens were masked and the model was trained to predict those tokens.
- Input: "[CLS] The man went [mask]  the store [SEP]"
- Output: "to"   

This tasks are very critical as the model learns to predict words based on the surrounding tokens. 

**2) Next Sentence Prediction.**  
In this task the model was trained to predict the next sentence and determine how likely it is. For eg. If one sentence is 
- Input : "[CLS] The man went to the store [SEP]"
- Output: "And he bought a gallon of milk [CLS]"  

The output in the above case is considered to be true. But if the model predicted like   

- Input : "[CLS] The man went to the store [SEP]"
- Output: "Penguins are flighless birds [CLS]"  

This would be a no.  
By training on these type of tasks, the model learns the relationship between two sentences. 

One special example of task specific BERT are Cross Encoders. In Cross Encoder models, two sentences are given as inputs with a seperator token, [SEP] and the model is asked to classify how similar this sentences are. The similarity score lies between 0 and 1.

<div style="text-align: center">
	<img style="max-width: 400px; text-align: center" src="/images/ce.png" />
	 <figcaption style="margin-top: 10px; font-style: italic; color: gray;font-size: small;">
            Cross Encoders 
        </figcaption>
</div>


## Token Embeddings in BERT

Lets see how token embeddings in BERT looks like. BERT has a vocabulary size of 30,000 words and an embedding dimension of 768. It means each token is represented into 768 integers. Each sentence is prepended with a special token **[CLS]**. The input text in tokenized and these tokens are passed into multiple encoder layers to create embeddings. With every encoder layer, these representations become better in integrating contexts of the whole sentence.  

<div style="text-align: center">
	<img style="max-width: 600px; text-align: center" src="/images/bertembed.png" />
	 <figcaption style="margin-top: 10px; font-style: italic; color: gray;font-size: small;">
            Cross Encoders 
        </figcaption>
</div>


## Conclusion 
So this is how basically sentence embedding model works. The main research of sentence embeddings was done by Google in USE (Universal Sentence Encoder) which utilizes original transformer architecture. SBERT was the first model which was trained on question-answer pairs which led to top performing models like "All-mini-LM-V6". The model was trained on more than 1B training pairs and has an embedding dimension of 384, which is very popularly used in RAG systems. 
 