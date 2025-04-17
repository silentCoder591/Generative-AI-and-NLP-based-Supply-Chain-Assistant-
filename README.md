# 🏭 Generative AI and NLP based Supply‑Chain Chatbot (Sage)

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

### 🏗️ Architecture

```mermaid
flowchart LR
    %% ---------- Front end ----------
    subgraph FE["Front End"]
        U(["User"])
        UI(["Chat UI"])
        U -- "HTTP / chat" --> UI
    end

    %% ---------- Back end ----------
    subgraph BE["Flask API"]
        NLP["NLP<br/>Intent + Slots"]
        TAPAS["TAPAS<br/>Table QA"]
        LLM["Gemini Flash 2.0"]
    end

    UI -- "POST / chat" --> BE
    BE --> NLP
    NLP --> TAPAS
    TAPAS -->|DataFrame| LLM
    LLM --> BE
    BE --> UI

    %% ---------- Data ----------
    subgraph DATA["Excel Tables"]
        XL["PO / Stock / Shipments"]
    end
    TAPAS -- pandas --> XL
```

## 🔄 Workflow

1. 💬 **User submits a question via the web-based chatbot.**  
   
   <img src="./Screesnshots/User%20sends%20question.png" width="300">

2. 🔄 **Flask forwards the question to the backend using GET/POST requests.**  
   
   <img src="./Screesnshots/Frontend_UI_message.png" width="1000">

3. 🧠 **The NLP component extracts the intent and details from the question.**  
   
   <img src="./Screesnshots/NLP_extraction.png" width="800">

4. 📊 **The Tapas component constructs a query and extracts relevant data from Excel files into a DataFrame.**  
   
   <img src="./Screesnshots/Tapas_query.png" width="800">

5. 🤖 **The GenAI component processes the DataFrame and question to generate a response.**  
   
   <img src="./Screesnshots/genai_input.png" width="800">

6. 📤 **The response is sent back to the frontend and displayed to the user.**  
   
   <img src="./Screesnshots/genai_output.png" width="300">


### 🧰 Tech Stack

| Layer           | Technologies                                                                                                                           |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| 🖥️ **Frontend**  | ![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white) ![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white) ![JS](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black) |
| ⚙️ **Backend**   | ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![Flask](https://img.shields.io/badge/Flask-000000?logo=flask&logoColor=white) |
| 🧠 **NLP**       | ![spaCy](https://img.shields.io/badge/spaCy-FF9900?logo=spacy&logoColor=white) + custom rule-based classifier                            |
| 📋 **Tabular QA**| ![TAPAS](https://img.shields.io/badge/Google_TAPAS-4285F4?logo=google&logoColor=white) (via 🤗 Transformers)                             |
| 🤖 **Gen AI**    | ![Gemini](https://img.shields.io/badge/Gemini-00ACC1?logo=google&logoColor=white) Flash 2.0 API                                            |
| 💾 **Data**      | Excel (openpyxl) + Pandas                                                                                                              |
| 🛠️ **DevOps**    | pip-tools, .env files, pre-commit hooks   
