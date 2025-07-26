# Multilingual-RAG-System
This project is a multilingual RAG system using Google Gemini and FAISS to answer Bangla and English queries from educational PDFs. It retrieves relevant content, generates context-aware answers, supports short-term memory, and includes evaluation metrics for grounded ness and relevance.
## 🔧 Setup Guide

1. **Install Required Libraries**
```bash
pip install fastapi uvicorn sentence-transformers faiss-cpu google-generativeai
pip install pytesseract pdf2image pillow
export GOOGLE_API_KEY="your-gemini-api-key" (Didn't give my gemini-api-key here for security purposes as this would be a public repository)
uvicorn app:app --reload       # For REST API

```
## 2. Tools, Libraries & Packages Used

Google Gemini API (google-generativeai)

SentenceTransformers (distiluse-base-multilingual-cased-v1)

FAISS – for semantic vector search

FastAPI – RESTful API interface

PyMuPDF / PyTesseract – for PDF/OCR text extraction

NumPy, Pickle – data handling

Colab / Replit – development and hosting

3. Sample Queries and Outputs
English
Query: Who is the author of the story "Aparichita"?
Answer: Rabindranath Tagore is the author of "Aparichita".

Bangla
প্রশ্ন: অনুপমের চরিত্র কেমন ছিল?
উত্তর: অনুপম একজন ভদ্র, আত্মমর্যাদাসম্পন্ন এবং সংবেদনশীল চরিত্র।


4. API Documentation

POST /ask

Request Body:

{
  "query": "Who is Anupam?"
}

Response:

{
  "query": "Who is Anupam?",
  "answer": "Anupam is a sensitive and respectful character...",
  "groundedness": 0.82,
  "relevance": 0.87
}

5.Evaluation Matrix:


| Metric           | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| Groundedness | Measures cosine similarity between answer and context         |
| Relevance  | Measures cosine similarity between query and retrieved chunks |


Question Answers-


What method or library did you use to extract the text, and why? Did you face any formatting challenges with the PDF content?
I used PyMuPDF and PyTesseract (OCR) because the PDF had mixed fonts and layout. Yes, some sentences appeared unreadable or broken in Bangla due to improper encoding, prompting us to fall back on OCR for cleaner Unicode output.

What chunking strategy did you choose?
I used character-based chunking with overlap (chunk_size=500, overlap=50) via LangChain’s RecursiveCharacterTextSplitter. This ensures that semantically connected phrases don’t get separated and helps Gemini maintain context while generating answers.


What embedding model did you use?
I used distiluse-base-multilingual-cased-v1 from sentence-transformers for its strong performance on multilingual tasks. It effectively captures semantic similarity across both English and Bangla, ensuring coherent retrieval for diverse queries.


How are you comparing the query with your stored chunks?
I used cosine similarity between query embeddings and stored chunk embeddings via FAISS. FAISS is fast and optimized for vector search. Cosine similarity helps measure semantic closeness rather than exact keyword match.

How do you ensure meaningful comparison between question and chunks?
I used multilingual embeddings so that both query and documents live in the same semantic space. If the query is vague, irrelevant chunks may be retrieved. In such cases, performance depends on the quality of chunking and vector representation.


Do the results seem relevant? If not, what might improve them?
Most results are relevant, especially for fact-based or character analysis questions. Improvements could include:
-Using paragraph-based chunking with better section alignment
-Fine-tuning embeddings for textbook-style content
-Expanding the corpus with more documents


