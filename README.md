# E-Commerce Product Recommender & Chatbot

## 🚀 Overview
This repository contains an e-commerce product recommendation and Q&A chatbot. It utilizes a Retrieval-Augmented Generation (RAG) architecture to accurately answer user queries based on a database of Flipkart product reviews and titles. The application is built using Flask and uses LangChain to orchestrate the retrieval and generation processes.

## ✨ Key Features
* **Conversational AI**: Uses the `ChatGroq` LLM to provide concise and helpful context-aware answers to product queries.
* **Chat History / Memory**: Maintains user session history to seamlessly answer follow-up questions and handle multi-turn conversations.
* **Vector Search Database**: Uses DataStax Astra DB to store and search through product review embeddings.
* **HuggingFace Embeddings**: Leverages HuggingFace endpoint models to compute highly accurate text embeddings for fast retrieval.
* **App Observability**: Integrated with `prometheus_client` to expose an endpoint (`/metrics`) for tracking HTTP request counts and application metrics.

## 🛠️ Technology Stack
* **Web Framework**: Flask
* **LLM Orchestration**: LangChain (`langchain`, `langchain-groq`, `langchain-huggingface`)
* **Vector Store**: Astra DB (`langchain-astradb`)
* **Monitoring**: Prometheus & Grafana (Kubernetes-ready configurations included)

## 📂 Project Structure
* `app.py`: The main Flask application entry point. Handles the user interface routing (`/`), chatbot API requests (`/get`), and Prometheus metrics (`/metrics`).
* `components/data_ingestion.py`: Manages connection to AstraDB and handles the ingestion of CSV data (`flipkart_product_review.csv`) into the vector database using HuggingFace embeddings.
* `components/rag_chain.py`: Configures the LangChain RAG pipeline, incorporating history-aware retrievers, contextual chat prompts, and the Groq LLM model.
* `requirements.txt`: Project dependencies.

## ⚙️ Setup & Installation

### 1. Prerequisites
Ensure you have Python 3.8+ installed.

### 2. Install Dependencies
Clone the repository and install the required Python packages:
```bash
pip install -r requirements.txt
```

### 3. Environment Variables
The application relies on `python-dotenv` to load configurations. Create a `.env` file in the root directory and ensure the variables mapped in `components/config.py` are defined. Common variables required include:
```ini
ASTRA_DB_API_ENDPOINT=your_astra_db_endpoint
ASTRA_DB_APPLICATION_TOKEN=your_astra_db_token
ASTRA_DB_KEYSPACE=your_keyspace
GROQ_API_KEY=your_groq_api_key
HUGGINGFACEHUB_API_TOKEN=your_huggingface_token
PORT=5001
```

### 4. Running the Application
Start the Flask development server:
```bash
python app.py
```
The application will be accessible at `http://localhost:5001`. You can test the chatbot directly via the web interface or interact with the `/get` API endpoint.

### 5. Data Ingestion Note
By default, the application is set to `load_existing=True` to fetch the pre-loaded database from Astra DB. If you need to populate the database with `data/flipkart_product_review.csv` for the first time, you must trigger `DataIngestor().ingest(load_existing=False)`.
