# Generative-AI-and-NLP-based-Supply-Chain-Assistant

# 🏭 Supply‑Chain AI Chatbot (Sage)

A web‑based conversational assistant that connects to SAP‑style ERP data and answers procurement, inventory, and shipping questions in natural language.  
Built for an academic capstone at Drexel University (MS CS, 2025).

---

## ✨ Key Features
| Area | Example Questions the Bot Understands |
|------|---------------------------------------|
| **Procurement** | • “Which POs are pending approval?”<br>• “Which purchase orders for material `F000010234` have a delivery date of **20 Sep** and are still open?” |
| **Inventory** | • “How much consignment stock is available at plant **7000**?”<br>• “Which batches have quantity > 500?” |
| **Shipping** | • “Which shipments have destination **7000**?”<br>• “What is the shipping ID of materials with expected delivery in **September**?” |

* Works on **tabular SAP‑like data stored in Excel**  
* **Intent + slot extraction** via custom NLP model  
* **TAPAS** (Table Parser) automatically converts slots to SQL‑like queries over Excel tables  
* **Gemini Flash 2.0** turns raw table rows into fluent answers (JSON → prose)  
* Exposed through a **Flask REST** back‑end and a lightweight HTML/JS chat UI  

---

## 🏗️ Architecture

```mermaid
flowchart LR
    subgraph Front‑End
        U(User) --HTTP-->|/chat| FE(Chat UI)
    end
    FE --POST /chat--> API[Flask<br>API]
    API --> NLP[NLP<br>Intent & Slots]
    NLP --> TAPAS[Google TAPAS<br>Table QA]
    TAPAS -->|DataFrame| LLM[Gemini Flash 2.0]
    LLM --Answer JSON--> API
    API --Response--> FE
    subgraph Data
        X1[Excel Tables<br>(PO, Stock, Shipments)]
    end
    TAPAS --pandas--> X1
