# Project Over View
This project is an agricultural document retrieval system that matches small hold farmer questions with relevant agricultural extension documents.
The project explores several retrieval methods like:
* BM25
* Dense Retrieval
* Hybrid retrieval
* Cross-encoder reranking
  
**nDCG@5** was the metric used to measure the system's efficiency to rank most relevant documents within the top 5 results.

# Dataset
This project was developed using four files, each serving a different purpose.
1. documents.csv --- Contains the agricultural knowledge base that the retrieval model searches.
2. train_queries.csv --- Contains the training questions that simulate the types of questions a farmer may ask.
3. qrels_train.csv --- Contains the relevance judgments used to evaluate how well a retrieval model ranks documents for each training query.
4. test_queries.csv --- Contains the unseen farmer questions used to evaluate the retrieval system.
   
The knowledge base contains 695 agricultural extension factsheets covering crop production, climate adaptation, pests, diseases, soil management, nutrient deficiencies, and fertilizer advice.
The dataset covers a range of crops relevant to agricultural production in Africa, including: Maize, Tomato, Rice, Cassava, Common beans, Cowpea etc.

The agricultural materials are primarily focused on African contexts, with sources and examples spanning countries including Nigeria, Ghana, Togo, Mali, Ethiopia, Kenya, and other African countries.

**Data availability:** The competition dataset is not included in this repository due to the competition's restrictions on redistribution of competition data. It can be obtained through the official Kaggle competition page.
Link to dataset: Dataset (https://www.kaggle.com/competitions/agricultural-extension-rag-smart-retrieval-for-farmers/data)


**Training Pipeline:** 
Four retrieval approaches were implemented and evaluated:
1. BM25: BM25 was used as a lexical retrieval baseline, ranking documents based on the occurrence and importance of query terms.
 
2. Dense Retrieval: Dense retrieval was implemented using sentence-transformer embeddings and cosine similarity to capture semantic relationships between farmer questions and agricultural documents.
Several document representations were explored, including title-only, text-only, title + text, and a title-weighted representation. The final dense retrieval system used intfloat/e5-base-v2 with the document representation: 'title + title + text'

3. Hybrid Retrieval: BM25 and dense retrieval scores were combined using min-max normalization and a weighted hybrid scoring approach. Different BM25/dense weighting values were evaluated to determine whether combining lexical and semantic retrieval improved performance.
4. Cross-Encoder Reranking: Dense retrieval was first used to retrieve the top 50 candidate documents for each query. These candidates were then passed through a pretrained Cross-Encoder to produce more refined relevance scores and rerank the documents.


# Evaluation:

The four retrieval approaches were evaluated using nDCG@5, where higher scores indicate better ranking of relevant documents within the top five results.
** Retrieval Approach |    nDCG@5
*  BM25               |    0.419
* Dense Retrieval     |    0.791
* Hybrid Retrieval    |    0.771
* Cross-Encoder Reranking  | 0.720
  
Dense retrieval achieved the strongest performance among the approaches evaluated and was therefore selected as the final retrieval approach and used to generate the test-set predictions.


# Reproduction:
To reproduce the project:
1. Clone this repository.
2. Install the required Python dependencies:
pip install -r requirements.txt

3. Obtain the competition dataset from Kaggle and place the required files in the appropriate location.
4. Open agricultural_rag.ipynb and run the notebook from top to bottom.

#Appendix:
**Contributor:** Blessing Nelson, Alfred Likita, Amisi Jospin Hassan, Obanijesumi Alawode

**References**
Sawant, S., Nair, R., & Hariharan, S. (2026). Empowering farmers with artificial intelligence: 
A retrieval-augmented generation based large language model advisory framework. Journal 
of Agricultural Engineering, 57, Article 1908. https://doi.org/10.4081/jae.2026.1908 

 Kutoma Wakunuma and Tilimbe Jiya. 2019. Stakeholder Engagement and Responsible Research & 
Innovation in promoting Sustainable Development and Empowerment through ICT. European Journal of 
Sustainable Development, 8, 3, 275–275. https://doi.org/10.14207/ejsd.2019.v8n3p275  

Gendered Chatbots in Nigeria: Critical Perspectives. In Responsible AI in Africa: Challenges and 
Opportunities. Damian Okaibedi Eke, Kutoma Wakunuma, and Simisola Akintoye (Eds.). Springer 
International Publishing, 119–139. https://doi.org/10.1007/978-3-031-08215-3_6  

The Engage2020 Action Catalogue. 2015. Retrieved from https://actioncatalogue.eu/  

Problem Statement: Optimizing RAG Document Retrieval for Agronomic Advice (project brief, 
Agricultural Extension RAG: Smart Retrieval for Farmers, Kaggle).
