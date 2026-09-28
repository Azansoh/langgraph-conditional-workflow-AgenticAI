# 🎓 UMT University Assistant

An intelligent **University Assistant** built with **LangGraph, RAG (Retrieval-Augmented Generation), FAISS, Hugging Face Embeddings, Groq LLM, and Streamlit**.

The application allows students to ask questions about **academic policies, fee structures, and general university-related topics**. It intelligently classifies each question and routes it to the appropriate knowledge source.

---

## 🚀 Project Overview

The **UMT University Assistant** is designed as an AI-powered student support system.

Instead of sending every question directly to an LLM, the application uses a **LangGraph workflow** to determine what type of question the student is asking.

### Query Routing

```text
                         ┌──────────────────┐
                         │   Student Query  │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │    Classifier    │
                         │    LangGraph     │
                         └────────┬─────────┘
                                  │
                ┌─────────────────┼─────────────────┐
                │                 │                 │
                ▼                 ▼                 ▼
          ┌──────────┐      ┌──────────┐      ┌──────────┐
          │ Academic │      │   Fees   │      │ General  │
          │   RAG    │      │   RAG    │      │ Knowledge│
          └────┬─────┘      └────┬─────┘      └────┬─────┘
               │                 │                 │
               ▼                 ▼                 │
          Academic PDF      Fee Structure PDF      │
               │                 │                 │
               └─────────────────┼─────────────────┘
                                 ▼
                         ┌──────────────────┐
                         │  Response Node   │
                         │  Groq LLM        │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │  Final Answer    │
                         └──────────────────┘
```

---

## ✨ Features

### 🎓 Programme Personalization

Students can select their programme from the sidebar:

* BSCS
* BSSE
* BSIT
* BBA
* BSAI
* BSDS

The selected programme is passed through the LangGraph state and used to personalize responses.

---

### 🧠 Intelligent Query Classification

Every student question is classified into one of three categories:

| Category    | Purpose                                                                            |
| ----------- | ---------------------------------------------------------------------------------- |
| 📘 Academic | Attendance, exams, grading, credits, promotion, courses, degree requirements, etc. |
| 💰 Fee      | Tuition fees, payments, refunds, scholarships, late charges, etc.                  |
| 💬 General  | Greetings, casual questions, and topics outside academic/fee documents             |

The classifier uses the **Groq LLM** to determine the appropriate route.

---

### 📚 Retrieval-Augmented Generation (RAG)

The project uses two separate PDF knowledge sources:

```text
academics_handbook.pdf
fee_structure.pdf
```

Each document is processed independently.

The processing pipeline is:

```text
PDF
 ↓
PyPDFLoader
 ↓
Text Splitting
 ↓
RecursiveCharacterTextSplitter
 ↓
Hugging Face Embeddings
 ↓
FAISS Vector Store
 ↓
Retriever
 ↓
Relevant Context
 ↓
Groq LLM
```

---

### 🔎 FAISS Semantic Search

The application uses **FAISS** to perform similarity-based retrieval.

For each question, the relevant PDF chunks are retrieved before generating the answer.

Configuration:

```python
chunk_size = 800
chunk_overlap = 100
k = 4
```

This means the retriever returns the **4 most relevant chunks** for the student's query.

---

### 🤖 Groq LLM

The application uses:

```text
openai/gpt-oss-120b
```

through the LangChain Groq integration.

The LLM is used for:

* Query classification
* Final answer generation
* Programme-aware responses
* General knowledge questions

---

### 🔀 LangGraph Workflow

The application is built using **LangGraph**.

The graph contains the following nodes:

```text
START
  ↓
classifier
  ↓
 ┌───────────────┬──────────────┐
 ↓               ↓              ↓
academic_rag   fee_rag       general
 ↓               ↓              ↓
 └───────────────┼──────────────┘
                 ↓
             response
                 ↓
                END
```

This provides a structured workflow instead of manually managing multiple conditional operations.

---

### 💬 Streamlit Chat Interface

The project provides a clean interactive chat interface using Streamlit.

Features include:

* 🎓 University-themed interface
* 💬 Chat-based interaction
* 🧑‍🎓 Student and assistant avatars
* 📘 Query classification badges
* ⚙️ Programme selection
* 🗑️ Clear chat button
* 🔄 Persistent chat history
* ⏳ Loading indicator

Example classification badges:

```text
ACADEMIC
FEE
GENERAL
```

---

## 🛠️ Tech Stack

### Programming Language

* **Python**

### AI / LLM

* **Groq**
* **LangChain**
* **LangGraph**

### Retrieval-Augmented Generation

* **FAISS**
* **Hugging Face Embeddings**
* **Sentence Transformers**
* **PyPDFLoader**
* **RecursiveCharacterTextSplitter**

### Frontend

* **Streamlit**
* Custom CSS

### Environment Management

* **python-dotenv**

---

## 📦 Project Structure

```text
UMT-University-Assistant/
│
├── app.py
│
├── academics_handbook.pdf
├── fee_structure.pdf
│
├── .env
├── .gitignore
│
└── README.md
```

---

## ⚙️ Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/umt-university-assistant.git
```

Move into the project directory:

```bash
cd umt-university-assistant
```

---

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

For macOS/Linux:

```bash
source venv/bin/activate
```

---

### 3. Install Dependencies

Install the required packages:

```bash
pip install streamlit langgraph langchain langchain-groq langchain-community langchain-text-splitters langchain-huggingface faiss-cpu pypdf sentence-transformers python-dotenv
```

---

## 🔑 Environment Variables

Create a `.env` file in the root directory:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Replace:

```text
your_groq_api_key_here
```

with your actual Groq API key.

### Important

Never upload your `.env` file or API key to GitHub.

Add this to `.gitignore`:

```text
.env
venv/
__pycache__/
```

---

## 📄 Required PDF Files

Place the following files in the same directory as `app.py`:

```text
academics_handbook.pdf
fee_structure.pdf
```

### `academics_handbook.pdf`

Used for questions related to:

* Academic policies
* Attendance
* Exams
* Grading
* Credits
* Promotion
* Courses
* Degree requirements
* Summer training
* Academic structure

### `fee_structure.pdf`

Used for questions related to:

* Tuition fees
* Payment
* Refunds
* Scholarships
* Late charges
* Programme-specific fees

---

## ▶️ Running the Application

After activating your virtual environment, run:

```bash
streamlit run app.py
```

Streamlit will start the application locally.

You can then open the URL shown in the terminal, usually:

```text
http://localhost:8501
```

---

## 🧪 Example Questions

### 📘 Academic Questions

```text
How many credits are required for my degree?
```

```text
What are the attendance requirements?
```

```text
What is the grading policy?
```

```text
How many courses do I need to complete?
```

---

### 💰 Fee Questions

```text
What is the tuition fee for BSCS?
```

```text
What is the fee structure for BBA?
```

```text
Are there any late payment charges?
```

```text
What is the refund policy?
```

---

### 💬 General Questions

```text
Hello
```

```text
What can you help me with?
```

```text
Tell me something interesting about university life.
```

---

## 🧩 Core Components

### 1. Resource Loading

The application loads the embedding model, PDF retrievers, and LLM using Streamlit's resource caching.

```python
@st.cache_resource
def load_resources():
```

This prevents expensive resources from being recreated unnecessarily during Streamlit reruns.

---

### 2. Academic Retriever

The academic PDF is converted into a FAISS retriever:

```python
academic_retriever = build_retriver("academics_handbook.pdf")
```

---

### 3. Fee Retriever

The fee PDF gets its own independent retriever:

```python
fee_retriever = build_retriver("fee_structure.pdf")
```

This separation allows the LangGraph router to select the appropriate knowledge base.

---

### 4. Query Classifier

The classifier determines whether the question is:

```text
academic
fee
general
```

The result is stored in:

```python
query_type
```

---

### 5. Conditional Routing

The router determines which node should execute:

```python
def route_query(state: State):
    if state["query_type"] == "academic":
        return "academic_rag"
    elif state["query_type"] == "fee":
        return "fee_rag"
    else:
        return "general"
```

---

### 6. Context Retrieval

For academic questions:

```python
docs = academic_retriever.invoke(query)
```

For fee questions:

```python
docs = fee_retriever.invoke(query)
```

The retrieved documents are combined into a context string and passed to the response node.

---

### 7. Programme-Aware Response

The selected programme is included in the response generation process.

For example:

```text
Student Programme: BSCS
```

The assistant can therefore focus on information relevant to the selected programme when the retrieved document contains multiple programme-specific details.

---

## 🧠 State Management

The LangGraph state contains:

```python
class State(TypedDict):
    programme: str
    messages: Annotated[list, add_messages]
    query_type: str
    retrieved_context: str
```

### State Fields

| Field               | Purpose                                 |
| ------------------- | --------------------------------------- |
| `programme`         | Stores the student's selected programme |
| `messages`          | Maintains conversation history          |
| `query_type`        | Stores the classifier result            |
| `retrieved_context` | Stores retrieved PDF information        |

---

## ⚡ Performance Optimization

The project uses:

```python
@st.cache_resource
```

for expensive resources.

This is applied to:

* Hugging Face embeddings
* PDF processing
* FAISS vector stores
* Groq LLM
* LangGraph application

This helps prevent rebuilding the entire RAG pipeline every time Streamlit reruns the application.

---

## 🔐 Security

The project uses environment variables for API credentials.

Example:

```env
GROQ_API_KEY=your_api_key
```

Do not hard-code API keys inside Python files.

Also make sure `.env` is included in `.gitignore`.

---

## 🔮 Future Improvements

Possible future enhancements include:

* 🔐 Student authentication
* 👤 Individual student profiles
* 📊 Student dashboard
* 🗂️ More university documents
* 📚 Course-specific RAG
* 📝 Exam schedule retrieval
* 🚌 University transport information
* 📅 Academic calendar integration
* 🧾 Online fee challan information
* 🌐 Deployment on Streamlit Cloud
* 💾 Persistent conversation history
* 📑 Source/page citations in answers
* 🧠 Conversation memory
* 🔍 Hybrid search using keyword + semantic retrieval
* ⚡ Streaming LLM responses

---

## 🎯 Learning Outcomes

This project demonstrates practical implementation of:

* Retrieval-Augmented Generation
* Vector databases
* Semantic search
* Document embeddings
* LangChain
* LangGraph
* Conditional AI workflows
* LLM-based classification
* PDF document processing
* Streamlit application development
* State management
* Prompt engineering
* Environment variable management

---

## 📌 Project Highlights

```text
📄 PDF Documents
       ↓
🔪 Text Chunking
       ↓
🧠 Hugging Face Embeddings
       ↓
🔎 FAISS Vector Search
       ↓
🔀 LangGraph Routing
       ↓
🤖 Groq LLM
       ↓
💬 Streamlit Chat Interface
```

---

## 👨‍💻 Author

**Muhammad Azan**

Computer Science Student | AI & Machine Learning Enthusiast | Full-Stack Developer

### Areas of Interest

* Artificial Intelligence
* Generative AI
* Machine Learning
* Deep Learning
* NLP
* RAG Systems
* Agentic AI
* Full-Stack Development
* Python Development

---

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.

---

## 📜 License

This project is intended for educational and demonstration purposes.
