# Telegram RAG Support Bot

## Overview

Telegram RAG Support Bot is an AI-powered Telegram assistant built with n8n, OpenAI, Google Gemini, and Pinecone.

The workflow supports:

* Text messages from Telegram
* Voice messages from Telegram
* Automatic voice transcription
* PDF ingestion from Google Drive
* Vector storage using Pinecone
* Retrieval-Augmented Generation (RAG)
* AI-powered answers based on uploaded documents

---

## Architecture

### User Query Flow

```text
Telegram
   │
   ▼
Telegram Trigger
   │
   ▼
Check Message Type
   ├── Text Message
   │      ▼
   │  Transform Text Message
   │
   └── Voice Message
          ▼
      Get Telegram File
          ▼
      Gemini Transcription
          ▼
      Transform Voice Message
                 │
                 ▼
             AI Agent
                 │
                 ▼
         Pinecone Retriever
                 │
                 ▼
         OpenAI GPT-5 Mini
                 │
                 ▼
         Telegram Response
```

---

## Document Ingestion Flow

```text
Google Drive Folder
        │
        ▼
Google Drive Trigger
        │
        ▼
Download File
        │
        ▼
Default Data Loader
        │
        ▼
Token Splitter
        │
        ▼
OpenAI Embeddings
        │
        ▼
Pinecone Upsert
```

---

## Components

### Telegram Trigger

Receives incoming Telegram messages.

Supported:

* Text messages
* Voice messages

---

### Voice Processing

Voice messages are processed through:

1. Telegram File Download
2. Google Gemini Audio Transcription
3. Text Extraction
4. AI Agent Processing

Supported MIME type:

```text
audio/ogg
```

---

### AI Agent

The AI Agent acts as the orchestrator.

Responsibilities:

* Understand user intent
* Call Pinecone Retriever
* Use retrieved context
* Generate final response

System Prompt:

```text
You are a helpful assistant
```

---

### OpenAI Chat Model

Model:

```text
gpt-5-mini
```

Used for:

* Question answering
* Context-aware responses
* RAG completion

---

### Pinecone Vector Database

Functions:

* Store document embeddings
* Retrieve relevant chunks
* Provide context to AI Agent

---

## Embedding Configuration

Embedding Model:

```text
OpenAI Embeddings
```

Dimensions:

```text
1536
```

Used for:

* Document embeddings
* Query embeddings

---

## Chunking Strategy

Current configuration:

```text
Chunk Size: 800
Chunk Overlap: 180
```

Recommended for:

* Technical PDFs
* Documentation
* Books
* Knowledge Bases

---

## Pinecone Storage

Each uploaded file is stored in a separate namespace.

Benefits:

* Isolation between documents
* Easier document management
* Better retrieval accuracy

---

## Retrieval Configuration

Retriever Description:

```text
Retrieve the most relevant text chunks from Pinecone based on the user's question.
Perform vector similarity search over embedded PDF content.
Return top matches as context for answer generation.
```

---

## Google Drive Knowledge Base

The workflow monitors a Google Drive folder.

When a new file is added:

1. Trigger activates
2. File is downloaded
3. Content is extracted
4. Text is chunked
5. Embeddings are generated
6. Chunks are stored in Pinecone

No manual ingestion is required.

---

## Use Cases

### Customer Support

Upload:

* Product manuals
* Documentation
* FAQs

Ask:

```text
How do I reset the device?
```

---

### Book Q&A

Upload:

* PDFs
* Research papers
* Technical books

Ask:

```text
What is Gradient Descent?
```

---

## Technologies

* n8n
* OpenAI GPT-5 Mini
* OpenAI Embeddings
* Pinecone
* Google Gemini
* Telegram Bot API
* Google Drive

---

## Future Improvements

* Metadata filtering
* Source citation
* Multi-document search
* Conversation memory
* User authentication
* Hybrid search (Vector + Keyword)
* Re-ranking
* Document versioning

---

## Workflow Status

Current Features:

✅ Telegram Chat

✅ Voice Transcription

✅ PDF Ingestion

✅ Pinecone Vector Search

✅ RAG Responses

✅ Automated Knowledge Base Updates

Version: 1.0
