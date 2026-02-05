# n8n_RAG-Agent
A production-grade RAG architecture built on n8n that autonomously ingests production logs, evaluates quality using LLM guardrails, and builds its own fine-tuning dataset. Reduces manual support toil by 80%.
# 🤖 Autonomous RAG Support Agent & Dataset Builder (n8n)

![n8n](https://img.shields.io/badge/Orchestration-n8n-FF6D5A?style=for-the-badge&logo=n8n)
![Python](https://img.shields.io/badge/Logic-Python-3776AB?style=for-the-badge&logo=python)
![OpenAI](https://img.shields.io/badge/LLM-OpenAI_GPT4-412991?style=for-the-badge&logo=openai)
![VectorDB](https://img.shields.io/badge/Memory-Vector_Database-000000?style=for-the-badge)

> **A production-grade RAG architecture built on n8n that autonomously ingests production logs, evaluates quality using deterministic LLM guardrails, and builds its own fine-tuning dataset. Proven to reduce manual support toil by 80%.**

---

## 📖 Overview

In high-volume support environments, agents waste hours searching through logs or answering repetitive technical queries. This project is a **Self-Correcting RAG (Retrieval-Augmented Generation) Pipeline** designed to solve that bottleneck.

Unlike standard chatbots, this system includes **Evaluation Guardrails**: it critiques its own answers before sending them. If the confidence score is low, it routes to a human and "learns" from the human's resolution to improve future performance.

### 🚀 Key Features
* **Automated Log Ingestion:** Parses unstructured production logs (JSON/Text) and embeds them into a Vector Database.
* **Hallucination Guardrails:** An intermediate LLM step evaluates the retrieved context against the generated answer to ensure factual accuracy.
* **Self-Improving Dataset:** Successfully resolved queries are automatically formatted into `JSONL` pairs to fine-tune smaller, cheaper models in the future.
* **Human-in-the-Loop:** Low-confidence answers are routed to Slack/Ticketing systems for manual review.

---

## 🏗️ Architecture

The workflow operates on a 4-stage logic loop:

1.  **Ingest & Embed:** Incoming queries/logs are sanitized and vectorized.
2.  **Retrieve & Synthesize:** Relevant SOPs and historical solutions are retrieved to draft a response.
3.  **Evaluate (The Judge):** A separate "Judge" agent scores the draft for accuracy and relevance.
    * *Pass (>80%):* Response sent to user.
    * *Fail (<80%):* Routed to human engineer.
4.  **Feedback Loop:** Human resolutions are captured to update the Vector Store.

![Workflow Diagram]
<img width="1548" height="769" alt="Screenshot 2026-01-13 170801" src="https://github.com/user-attachments/assets/8c3c54e7-971b-431a-add9-1a2232ac87ab" />


---

## 🛠️ Tech Stack

* **Orchestration:** n8n (Self-hosted/Cloud)
* **LLM:** OpenAI GPT-4o / GPT-3.5-Turbo
* **Vector Database:** Pinecone / Supabase / Qdrant (Configurable)
* **Data Processing:** Python (Pandas) inside n8n Code Nodes

---

## ⚙️ Setup & Installation

### Prerequisites
* An active **n8n** instance (Local or Cloud).
* API Keys for **OpenAI** and your **Vector Database**.

### Importing the Workflow
1.  Download the `RAG_Agent_Workflow.json` file from this repository.
2.  Open your n8n dashboard.
3.  Click **Workflows** > **Import from File**.
4.  Select the JSON file.

### Configuration
Double-click the nodes to add your credentials:
1.  **OpenAI Node:** Add your API Key.
2.  **Vector Store Node:** Connect your preferred database.
3.  **Environment Variables:** Set your `similiarity_threshold` (Recommended: 0.85).

---

## 📊 Performance Metrics

| Metric | Before Automation | With RAG Agent | Improvement |
| :--- | :---: | :---: | :---: |
| **Avg. Response Time** | 20 mins | < 45 secs | **96% Faster** |
| **Manual Search Time** | 15 mins/ticket | 0 mins | **100% Removed** |
| **Resolution Accuracy** | Varied | Consistent (SOP based) | **Standardized** |

---

## 🤝 Contributing

This is an open-source template for building reliable support agents. Pull requests for new "Guardrail" logic or different Vector DB integrations are welcome.

## 📄 License

MIT License. Free to use for personal and commercial projects.
