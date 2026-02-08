[README.md](https://github.com/user-attachments/files/25159308/README.md)
<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a RAG API with FastAPI

**Project Link:** [View Project](http://learn.nextwork.org/projects/ai-devops-api)

**Author:** Vince Gayatgay  
**Email:** vincegayatgay132@gmail.com

---

![Image](http://learn.nextwork.org/curious_teal_bold_lulo/uploads/ai-devops-api_g3h4i5j6)

---

## Introducing Today's Project!

In this project, I will demonstrate RAG (Retrieval-Augmented Generation) API using FastAPI + Chroma + Ollama (a local LLM) I'm doing this project to learn more about devops.

### Key services and concepts

A modern, fast web framework for building APIs with Python.

RAG (Retrieval-Augmented Generation): The architectural pattern that combines document search with AI generation.

Chroma: A vector database used to store and retrieve embeddings from your knowledge base.

Ollama: For running local large language models like tinyllama.

Swagger UI: For interactive API documentation and testing.

HTTP methods: Specifically the POST method for sending data to your API.

### Challenges and wins



### Why I did this project

I did this project because I wanted to deepen my understanding of modern AI architectures, particularly how RAG systems combine information retrieval with language models through a FastAPI backend.

---

## Setting Up Python and Ollama

In this step, I'm setting up Python. Python is the programming language we'll use to build API. It is the foundation for the project, as FastAPI, Chroma, add all the other libraries we'll use are Python packages. Without it, the API code wouldn't be able to run.

Ollama is a tool that runs AI models locally on the machine. It acts as a bridge or gateway to the AI language models (LLMs) we be using. When API receives a question, it will send it to Ollama, which then connects to the AI model to generate a human-like response. This is crucial for the "Generation" part of RAG.  

I need both of these tools because RAG API relies on Python for its core logic and Ollama to power its AI capabilities.

### Python and Ollama setup

![Image](http://learn.nextwork.org/curious_teal_bold_lulo/uploads/ai-devops-api_i9j0k1l2)

### Verifying Python is working

### Ollama and tinyllama ready

Ollama is a tool that allows you to run large language models, like tinyllama, directly on your own computer. It acts as a bridge or gateway to these AI models. Instead of sending requests to cloud services, Ollama runs the AI model locally, offering privacy, no API costs, and the ability to work offline. 

I downloaded the tinyllama model because it's a smaller, faster AI model that's perfect for learning and local development in this project. Tinyllama will serve as the "brain" behind RAG API's responses.

The model will help my RAG API by powering the "Generation" part of Retrieval-Augmented Generation. When the API receives a question, it will first find relevant information from my own documents (the "Retrieval" part). 

---

## Setting Up a Python Workspace

In this step, I'm setting up Python workspace. I need it because I will create a virtual environment, Python sets up a folder (usually named venv) that contains its own copy of Python and a place to install packages. When you "activate" this virtual environment, terminal then uses the Python and packages from that specific folder instead of the system-wide Python.

### Python workspace setup

### Virtual environment

A virtual environment is an isolated Python environment that keeps the project's dependencies separate from other Python projects on my computer. I created one for this project to  I created one for this project to ensure that all the specific versions of libraries like FastAPI, Uvicorn, ChromaDB, and Ollama that this RAG API needs don't conflict with any other Python projects.

Once I activate it, my terminal uses the Python and packages from this specific environment instead of the system-wide Python. This is indicated by the (venv) prefix in terminal prompt. To create a virtual environment, I use a command like py-3 -m venv venv (on Windows) within the project folder.

### Dependencies

The packages I installed are fastapi, uvicorn, chromadb, and ollama. FastAPI is used for web framework that helps you build APIs quickly. It handles all the networking and routing logic, allowing you to focus on writing the actual RAG functionality. ChromaDB Chroma is used for a vector database that stores document embeddings (numerical representations of text). When a user asks a question, ChromaDB searches through these embeddings to find the most relevant documents, which is the "Retrieval" part of RAG. Uvicorn is used for listening for incoming requests and routes them to the right functions in your code. Ollama is a Python client library that lets your code talk to the Ollama service. This package gives your Python API a way to send questions to tinyllama and get responses programmatically.

![Image](http://learn.nextwork.org/curious_teal_bold_lulo/uploads/ai-devops-api_u1v2w3x4)

---

## Setting Up a Knowledge Base

In this step, I'm creating knowledge base. A knowledge base is essentially a collection of your own documents or data that your AI model can refer to. Think of it as a specialized library for your AI. I need it because  AI models like tinyllama have limited knowledge from their training data. By providing your own knowledge base, you can give the AI accurate, up-to-date information about specific topics. This is the "Retrieval" part of RAG we retrieve relevant information from your documents first, before generating an answer.

### Knowledge base setup

![Image](http://learn.nextwork.org/curious_teal_bold_lulo/uploads/ai-devops-api_t1u2v3w4)

### Embeddings created

Embeddings are numerical representations of text. Think of them like a smart index in a library. I created these embeddings by running the embed.py script. This script takes the text documents, like k8s.txt, and converts them into these numerical representations, then stores them in ChromaDB. The db/ folder was created by ChromaDB when I ran the embedding script. It contains knowledge base's embeddings so ChromaDB can quickly search through them when the  API is running. Inside, you'll find a subfolder with a unique name that holds the raw embedding vectors and associated data, which are the actual numerical representations of the text. 

This is important for RAG because when a user asks a question, ChromaDB searches through these embeddings to find the most relevant documents. This is the "Retrieval" part of RAG – it retrieves relevant information before generating an answer, ensuring the AI has accurate and up-to-date context.

---

## Building the RAG API

In this step, I'm building a RAG API (Retrieval Augmented Generation Application Programming Interface). An API (Application Programming Interface) is software retrieve and share data with other apps. For example, when you use a travel app to see flight prices, the app uses airline APIs to request up-to-date prices and display them for you. 

FastAPI is  is a modern, fast (high-performance) web framework for building APIs with Python. It handles all the complex networking and routing logic (like managing HTTP requests and responses), so you can focus on writing the specific functionality for RAG system, rather than getting bogged down in the underlying web server details. I'm creating this because it will serve as the interface for RAG system. It will receive user questions, use your knowledge base to find relevant information, and then send that information to an AI model (like tinyllama via Ollama) to generate an informed answer.

### FastAPI setup

### How the RAG API works

My RAG API works by:

Receive Question: When a user sends a question to the API (for example, through the/query endpoint), FastAPI application receives it.

Search Knowledge Base with ChromaDB: The API then takes that question and uses ChromaDB to search through the embeddings (numerical representations) of my documents. ChromaDB efficiently finds the most relevant pieces of information from knowledge base that are semantically similar to the user's question.

Get Relevant Context: ChromaDB returns the relevant document snippets or "context" that are most likely to contain the answer to the user's question.

Combine Context + Question: FastAPI application then takes this retrieved context and the original user question and combines them into a single, well-structured prompt.

Send to Ollama's tinyllama: This combined prompt is then sent to the Ollama service, which uses the tinyllama AI model to generate an answer. The ollama Python client library facilitates

![Image](http://learn.nextwork.org/curious_teal_bold_lulo/uploads/ai-devops-api_f3g4h5i6)

---

## Testing the RAG API

In this step, I'm testing my RAG API. I'll test it using two methods: first, by calling the endpoint directly from the command line, and then by exploring it visually with FastAPI's Swagger UI. Swagger UI is an automatically generated, interactive documentation page for FastAPI server. It allows you to visually explore your API's endpoints, see what parameters they accept, and even try them out directly from your browser.

### Testing the API

### API query breakdown

I queried my API by running the command Invoke-RestMethod - PowerShell's command for making HTTP requests Uri - The URL of our API endpoint with the question as a parameter -Method Post - Specifies we want to use the POST method The command uses the POST method, which means the POST method is an HTTP method used to send data to a server. In the context of RAG API, I used  POST to send question (which is data) to the API. 

![Image](http://learn.nextwork.org/curious_teal_bold_lulo/uploads/ai-devops-api_g3h4i5j6)

### Swagger UI exploration

Swagger UI is an automatically generated, interactive documentation page for your FastAPI server. It allows you to visually explore your API's endpoints, see what parameters they accept, and even test them out directly from your browser. The best part about using Swagger UI was It's a powerful tool because it eliminates the need to manually craft curl commands or write test scripts for every endpoint, making API testing and exploration much more user-friendly.

---

## Adding Dynamic Content

### Adding the /add endpoint

### Dynamic content endpoint working

---

---
