# PDF AI Assistant

This project implements a **PDF-based AI assistant** using the `phi` framework. It loads PDF documents as a **knowledge base**, stores conversation history in a PostgreSQL database, and provides an interactive command-line interface (CLI) for user interaction.

---

## Features
- **PDF-based Knowledge Retrieval**: Loads a PDF from a URL and processes it using vector search.
- **Conversational Memory**: Stores chat history in PostgreSQL to continue previous sessions.
- **Vector Database Support**: Uses `pgvector` for semantic search.
- **Interactive CLI**: Enables interaction via a command-line chatbot.

---

## Installation

### 1. Clone the Repository
```bash
 git clone https://github.com/itzuditsharma/PDF-Assistant-AI-Agent.git
 cd PDF-Assistant-AI-Agent
```

### 2. Create a Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Set Up Environment Variables
Create a `.env` file in the project root and add:
```env
GROQ_API_KEY=your_groq_api_key
```

### 5. Start PostgreSQL and Configure Database
Ensure PostgreSQL is running and create the necessary database and table.
```bash
psql -U ai -d ai -h localhost -p 5532
```

---

## Usage

### Run the Assistant
```bash
python main.py
```

### Start a New Chat Session
```bash
python main.py --new
```

---

## Code Explanation

### 1. Load Environment Variables
```python
from dotenv import load_dotenv
import os
load_dotenv()
os.environ["GROQ_API_KEY"] = os.getenv("GROQ_API_KEY")
```
Loads API keys and credentials from the `.env` file.

### 2. Configure PostgreSQL Database
```python
db_url = "postgresql+psycopg://ai:ai@localhost:5532/ai"
```
Defines the database connection string.

### 3. Load PDF Knowledge Base
```python
knowledge_base = PDFUrlKnowledgeBase(
    urls=["https://phi-public.s3.amazonaws.com/recipes/ThaiRecipes.pdf"],
    vector_db=PgVector2(collection="recipes", db_url=db_url)
)
knowledge_base.load()
```
Fetches and processes the PDF for AI-assisted retrieval.

### 4. Set Up Conversation Storage
```python
storage = PgAssistantStorage(table_name="pdf_assistant", db_url=db_url)
```
Stores chat history in PostgreSQL.

### 5. Create and Run the Assistant
```python
assistant = Assistant(
    user_id=user,
    knowledge_base=knowledge_base,
    storage=storage,
    show_tool_calls=True,
    search_knowledge=True,
    read_chat_history=True
)
assistant.cli_app(markdown=True)
```
Initializes and runs the chatbot in a CLI environment.

---

## Outputs
![image](https://github.com/user-attachments/assets/45f182bf-1875-4d05-9db4-13c378fc4a38)
![image](https://github.com/user-attachments/assets/5945d014-2e80-43af-a802-8022eb0e368c)
![image](https://github.com/user-attachments/assets/63f2d153-5cff-49e4-a69c-c231328d01ef)



## Contributing
Feel free to submit issues or pull requests to improve this project!

---

## License
This project is licensed under the MIT License.

