🧠 LangChain Conversational Chatbot with OpenAI & Chroma
```
This project demonstrates a simple conversational chatbot with document retrieval using LangChain, OpenAI LLMs, and Chroma vector database.
The chatbot answers user questions only using the provided document context (Retrieval-Augmented Generation – RAG).
```
🚀 Features
```
Uses OpenAI Chat Models via LangChain

Creates embeddings using OpenAI Embedding models

Stores and searches documents with Chroma Vector Store

Retrieves the most relevant documents using similarity search

Answers questions strictly based on retrieved context

Easy to run on Google Colab
```
🛠️ Tech Stack
```
Python

LangChain

OpenAI (Chat & Embeddings)

ChromaDB

Google Colab
```
📦 Installation
```
Install the required libraries:

pip install langchain -qU
pip install langchain-openai -qU
pip install langchain-chroma -qU
```
🔑 Set Up OpenAI API Key (Colab)
```
Store your OpenAI API key securely in Google Colab:

from google.colab import userdata
import os

os.environ["OPENAI_API_KEY"] = userdata.get("OpenAi_Key")
```
🤖 Initialize the LLM
```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0
)
```
📐 Create Embeddings
```
from langchain_openai import OpenAIEmbeddings

embedding_model = OpenAIEmbeddings(
    model="text-embedding-3-small"
)
```
📄 Load Documents
```
from langchain_core.documents import Document

documents = [
    Document(
        page_content="The T20 World Cup 2024 ... Rohit Sharma ...",
        metadata={"source": "cricket news"},
    ),
    Document(
        page_content="The world of football is buzzing ...",
        metadata={"source": "football news"},
    ),
    Document(
        page_content="As election season heats up ...",
        metadata={"source": "election news"},
    ),
    Document(
        page_content="The AI revolution continues ...",
        metadata={"source": "ai revolution news"},
    ),
]
```
🧠 Create Vector Store (Chroma)
```
from langchain_chroma import Chroma

vectorstore = Chroma.from_documents(
    documents,
    embedding=embedding_model
)
```
🔍 Similarity Search
```
results = vectorstore.similarity_search("nlp")

for result in results:
    print(result.page_content)
    print(result.metadata)

🔄 Create a Retriever
retriever = vectorstore.as_retriever(
    search_type="similarity",
    search_kwargs={"k": 1},
)
```
🧾 Prompt Template
```
from langchain_core.prompts import ChatPromptTemplate

message = """
Answer this question using the provided context only.

{question}

Context:
{context}
"""

prompt = ChatPromptTemplate.from_messages([
    ("human", message)
])
```
🔗 Build the LangChain Pipeline
```
from langchain_core.runnables import RunnablePassthrough

chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
)
```
❓ Ask a Question
```
response = chain.invoke(
    "Who is the captain of India's cricket team?"
)

print(response.content)
```
✅ Output
```
The captain of India's cricket team is Rohit Sharma.
```
📌 Key Concept Used
```
Retrieval-Augmented Generation (RAG)

Vector Similarity Search

Prompt Engineering

LangChain Runnables
```
👤 Author
```
Rusira Dinujaya
Software Engineering Intern Zebra technologies
Interested in AI, NLP & Backend Development
```
