# MYHAQ AI - AI-Powered Legal Assistant

### React | FastAPI | MongoDB | Groq API | FAISS | RAG

---

## 1. Project Overview

MYHAQ AI is a full-stack, AI-powered legal assistant web application that helps citizens understand their legal rights and generate formal complaint documents. The name **MYHAQ** reflects the concept of *my right* (Haq meaning right or entitlement), emphasizing the platform's mission to make legal knowledge accessible to everyone.

The platform allows users to describe their legal situation in plain language, and the AI responds with relevant Indian Penal Code (IPC) sections, explains their legal rights, and automatically generates a professional complaint document ready for submission to authorities.

---

## 2. Purpose and Problem It Solves

Navigating the Indian legal system is challenging for ordinary citizens. Most people do not know which IPC sections apply to their situation, how to formally phrase a complaint, or what punishment an offender may face. Legal consultations are expensive and often inaccessible.

**MYHAQ AI addresses this by:**

- Providing instant, AI-generated legal explanations in plain language that anyone can understand
- Automatically identifying the relevant IPC sections based on the user's described situation
- Generating a formally worded legal complaint document that users can take to a police station or court
- Making legal awareness available 24/7 without any cost or appointment requirement

**Example use cases:**

- A user reports that their wallet was stolen. MYHAQ AI identifies IPC Section 378 (Theft) and IPC Section 379 (Punishment for Theft), explains the law, and generates a complaint.
- A user reports online fraud. The system identifies IT Act Section 66C (Identity Theft) and Section 66D (Cheating by impersonation), explains punishments, and drafts the complaint.
- A user faces harassment at the workplace. The platform identifies relevant IPC sections and generates a formal written complaint.

---

## 3. Features and Functionality

### 3.1 AI Legal Question Answering

The Ask Question module is the primary interface:

- Users type their legal situation in natural language (e.g., *"My wallet was stolen and I want to file a police complaint"*)
- The AI processes the query and returns a detailed legal explanation covering what happened legally, which laws apply, and what the user can do next
- Responses are contextual - the AI uses Retrieval Augmented Generation (RAG) to search through a legal knowledge base and return the most relevant information rather than guessing

### 3.2 Relevant IPC Section Display

Alongside the legal explanation, the platform displays a structured panel of relevant IPC sections:

- Section number and title (e.g., IPC Section 378 - Theft)
- A plain-language definition of what the section covers
- The punishment prescribed under the section
- Multiple related sections are shown when the situation involves multiple offences (e.g., both theft and identity theft)

### 3.3 Automated Complaint Generator

The Complaint Generator module automates the creation of formal legal documents:

- Users provide the details of their complaint (incident date, location, parties involved, description of events)
- The AI drafts a professionally formatted complaint letter using appropriate legal language
- The generated complaint references the applicable IPC sections automatically
- The document is ready for the user to print or submit digitally

### 3.4 RAG-Powered Contextual Responses

The Retrieval Augmented Generation (RAG) system is what makes MYHAQ AI accurate and reliable:

- A FAISS (Facebook AI Similarity Search) vector index stores the legal knowledge base (IPC sections, legal definitions, case examples)
- When a user submits a query, the system first retrieves the most relevant chunks of legal text from the FAISS index
- The retrieved context is then passed to the Groq API LLM (Large Language Model) along with the user's question
- This ensures the AI response is grounded in actual legal text rather than hallucinated or generic answers

### 3.5 Responsive UI

- The frontend is built with React and styled for full responsiveness across desktop and mobile devices
- A clean sidebar navigation provides access to Home, Ask Question, and Complaint Generator sections
- The interface is designed to be non-intimidating and easy to use for people with no legal background

---

## 4. Technology Stack

| Layer | Technology | Role |
|-------|-----------|------|
| Frontend | React.js | UI components, routing, state management |
| Frontend | Tailwind CSS | Responsive styling and layout |
| Backend | FastAPI (Python) | High-performance API server for AI and data endpoints |
| AI - LLM | Groq API | Fast inference LLM for generating legal responses |
| AI - Retrieval | FAISS | Vector similarity search over the legal knowledge base |
| AI - Pattern | RAG | Retrieval Augmented Generation for contextual accuracy |
| Database | MongoDB | Stores user queries, generated complaints, and session data |
| Deployment | Uvicorn | ASGI server for running FastAPI |

---

## 5. System Architecture

### 5.1 High-Level Flow

The request lifecycle for a legal query works as follows:

1. **User Input** - The user types a legal question or complaint description in the React frontend
2. **API Request** - The frontend sends an HTTP POST request to the FastAPI backend with the user's query
3. **Embedding** - The backend converts the query into a vector embedding using a sentence transformer model
4. **FAISS Retrieval** - The query embedding is compared against the FAISS index and the top-K most relevant legal text chunks are retrieved
5. **Prompt Construction** - The retrieved legal context is combined with the user's query into a structured prompt
6. **Groq API Call** - The prompt is sent to the Groq API which runs the LLM and returns a legal explanation
7. **Response** - The backend parses the LLM response, identifies the relevant IPC sections, and returns structured JSON to the frontend
8. **Display** - The frontend renders the legal explanation and IPC sections panel side by side

### 5.2 RAG Architecture Detail

RAG (Retrieval Augmented Generation) is the core intelligence pattern of MYHAQ AI. It combines the power of vector search with the fluency of a large language model:

- **Knowledge Base** - A curated dataset of Indian Penal Code sections, IT Act provisions, and legal definitions is preprocessed and chunked into small passages
- **FAISS Index** - Each passage is converted to a dense vector using a sentence transformer. These vectors are stored in a FAISS index for millisecond-speed similarity search
- **Query Matching** - When a user submits a query, it is also converted to a vector. FAISS finds the passages most similar to the query
- **Augmented Prompt** - The matched passages are injected into the LLM prompt as context, grounding the model's response in factual legal text
- **Generation** - The Groq API LLM generates a coherent, contextually accurate legal explanation using the provided context

---

## 6. Application Pages and Modules

### 6.1 Home

The landing page introduces MYHAQ AI, explains its purpose, and guides users to either the Ask Question or Complaint Generator modules. It sets the tone of trust and accessibility.

### 6.2 Ask Question

This is the primary legal query interface:

- A text area where users describe their legal situation in their own words
- A Submit button that triggers the AI pipeline
- A Legal Explanation section that renders the AI's detailed response
- A Relevant IPC Sections panel on the right that lists all matching legal sections with definitions and punishments

### 6.3 Complaint Generator

This module walks users through generating a formal complaint:

- Input fields for incident details: date, location, description, parties involved
- AI generates a professionally formatted complaint letter based on the inputs
- The complaint references applicable IPC sections automatically
- Users can copy or download the generated complaint

---

## 7. Project Structure

```
myhaq-ai/
├── frontend/
│   ├── src/
│   │   ├── components/        # Navbar, Sidebar, IPCPanel, ExplanationBox
│   │   ├── pages/             # Home.jsx, AskQuestion.jsx, ComplaintGenerator.jsx
│   │   └── App.jsx            # Routing and layout
│   ├── public/
│   └── package.json
├── backend/
│   ├── main.py                # FastAPI app entry point
│   ├── routes/
│   │   ├── query.py           # /api/query - handles legal Q&A
│   │   └── complaint.py       # /api/complaint - generates complaint docs
│   ├── rag/
│   │   ├── embedder.py        # Converts text to vector embeddings
│   │   ├── faiss_index.py     # FAISS index build and query logic
│   │   └── knowledge_base/    # IPC sections and legal text files
│   ├── groq_client.py         # Groq API integration
│   ├── models.py              # MongoDB schemas (Pydantic models)
│   ├── database.py            # MongoDB connection
│   └── requirements.txt       # Python dependencies
└── README.md
```

---

## 8. API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/query` | Submit a legal question; returns explanation + IPC sections |
| POST | `/api/complaint` | Submit complaint details; returns generated complaint text |
| GET | `/api/history` | Retrieve past queries and complaints for a user |
| GET | `/api/ipc/:section` | Fetch details for a specific IPC section |
| GET | `/health` | Health check endpoint for the backend server |

---

## 9. Installation and Setup

### 9.1 Prerequisites

- Python 3.10 or higher
- Node.js v18 or higher
- MongoDB (local or Atlas)
- Groq API key (free tier available at [console.groq.com](https://console.groq.com))

### 9.2 Clone the Repository

```bash
git clone https://github.com/<your-repo>/myhaq-ai.git
cd myhaq-ai
```

### 9.3 Backend Setup

```bash
cd backend
pip install -r requirements.txt
```

Create a `.env` file in the backend directory:

```
GROQ_API_KEY=your_groq_api_key_here
MONGO_URI=mongodb://localhost:27017/myhaq
PORT=8000
```

Build the FAISS index from the knowledge base:

```bash
python rag/faiss_index.py --build
```

Start the FastAPI server:

```bash
uvicorn main:app --reload --port 8000
```

### 9.4 Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend will be available at `http://localhost:5173` and the backend API at `http://localhost:8000`.

---

## 10. Future Enhancements

- Voice input support so users can speak their complaint rather than type it
- Multi-language support (Hindi, Telugu, Tamil) for broader accessibility
- PDF export of generated complaint documents with official formatting
- Integration with e-filing portals for direct digital submission of complaints
- Expansion beyond IPC to cover consumer protection laws, RTI, and labour laws
- Lawyer directory integration for users who need professional legal help
- Case status tracking for filed complaints

---

## 11. Team

Developed by **Bhavana** as part of the Software Development Project.

---

## 12. License

This project is developed for educational purposes under the SDP-11 academic project guidelines.

> Legal information provided by MYHAQ AI is for awareness purposes only and does not constitute professional legal advice.
