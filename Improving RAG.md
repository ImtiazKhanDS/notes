Rag companies Jason liu consulted
1. Timescale
2. Limitless AI
3. Sandbar
4. Raycast
5. Tensorlake
6. Dunbar
7. Bytebot
8. Naro
9. Trunk Tools
10. New Computer
11. Weights and Biases
12. Modal Labs
13. Pydantic

**Feedback design**
1. Thumbs up or thumbs down 
2. Is your query responded correctly 
3. Satisfaction
**Monitor cosine and rerankers (relevancy)**
(query , cosine , feedback)

**Cluster queries to understand** 
topic , count, mean cosine, mean feedback
- rank importance of question types against relevance and satisfaction 
- Look at data to propose experiments to improve individual clusters 
- Build classifiers to improve observations in real time.

Once you know the topic clusters , create more questions for certain topics from an llm and improve rag

Tech Stack
1. LanceDB
2. Pydantic

Fine tune your custom embedding models

Chunking strategy by both OpenAI and Claude 
800 tokens with 50% overlap

