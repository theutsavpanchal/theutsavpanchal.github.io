---
title: LLMs to edit files - Part 1
layout: default
tags: [rag]
pinned: true
blog_post: false
---

# LLMs to Edit files : Groq, Langchain & streamlit UI

### Overview
In this blogpost, we will build a custom application which can help us to edit JSON files with a simple prompt.   

### Requirements/Expectations
This is one of the important steps, you should first define what kind of operations/properties you expect your applications should do/have.
1) The files will be in JSON format. 
2) User can ask any query related to the files. 
3) The query can be about extracting particular value, modifying/deleting/adding a single value, modifying/deleting/adding values in batch.
4) The application should also be able to remember atleast 1 previous conversation. 

Lets start building the application. 

