# Local RAG Agent & Vector Pipeline (n8n + Qdrant + Gemini)

A RAG (Retrieval-Augmented Generation) project I built with n8n, Qdrant, and Google Gemini. One workflow loads documents into a local vector database, and a second one is a chat agent that answers questions using only that data. I made it to learn how RAG works end to end.

![n8n](https://img.shields.io/badge/n8n-Workflow%20Automation-FF6D5A?style=flat&logo=n8n)
![Qdrant](https://img.shields.io/badge/Qdrant-Vector%20Database-DC2626?style=flat&logo=qdrant)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-LLM%20%26%20Embeddings-8E75B2?style=flat&logo=googlegemini)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=flat&logo=docker)
![License](https://img.shields.io/badge/License-MIT-green.svg)

<img width="1568" height="821" alt="System architecture diagram" src="https://github.com/user-attachments/assets/9f2efc1a-1565-439c-83dc-4f7553ca28ea" />

## How It Works

The project has two separate workflows, so loading documents and chatting don't interfere with each other.

**1. Ingestion pipeline**
- Fetches a document and reads its binary data
- Splits the text into chunks (1,000 characters with 200 overlap) so context isn't lost at chunk edges
- Creates embeddings with Google's `embedding-001` model
- Stores them in a self-hosted Qdrant database

**2. Chat agent**
- An n8n AI Agent decides when to search Qdrant for an answer
- Window buffer memory keeps track of the conversation
- The prompt tells the model to answer only from retrieved context, which helps reduce made-up answers

## Built With
n8n, Qdrant, Google Gemini (chat + embeddings), Docker

## What I Learned
- How embeddings and vector search work
- Why chunk size and overlap affect answer quality
- Building an AI agent that uses a database as a tool
- Running services locally with Docker

## Setup

You'll need Docker, a self-hosted n8n instance, and a Google Gemini API key.

**1. Start Qdrant**

```bash
docker run -d -p 6333:6333 -p 6334:6334 \
  -v qdrant_storage:/qdrant/storage \
  qdrant/qdrant
```

**2. Import the workflows**
Import the ingestion and chat workflow files into n8n (**Workflows > Import from File**).

**3. Add credentials**
Add your Gemini API key and your Qdrant URL (`http://localhost:6333`) to the matching nodes.

**4. Run it**
Run the ingestion workflow first to load a document, then open the chat agent and ask questions about it.
