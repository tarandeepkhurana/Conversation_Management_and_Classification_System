# Conversational Data Management & Information Extraction with Groq API
This project demonstrates two core tasks using Groq’s OpenAI-compatible API:
- Conversation History Management with Summarization
- JSON Schema-based Information Extraction & Validation
---
## **Setup**
1. Get a Groq API Key and store it securely in Colab using:
```
from google.colab import userdata
groq_api_key = userdata.get("GROQ_API_KEY")
```
This avoids hardcoding sensitive credentials.

2. Initialize the client:
```
import openai
client = openai.OpenAI(
    base_url="https://api.groq.com/openai/v1",
    api_key=groq_api_key
)
```
---
## **🚀 Task 1: Conversation History Management & Summarization**
We implement a ConversationManager class that:
- Stores full message history (with role, content, timestamp).
- Provides truncation by:
  - Turns → keep last n turns.
  - Characters → keep within a maximum character budget.
- Periodically generates summaries using Groq LLMs:
  - Summarization is triggered every k runs (summarization_k).
  - Replaces the history with a concise system summary + most recent context.
- Ensures conversations remain scalable and lightweight without losing context.

**Example Features:**
- Add messages (add_message)
- Get conversation history (get_history)
- Truncate history (truncate_by_turns, truncate_by_chars)
- Summarize history (summarize_history, maybe_summarize)

This ensures long-running chats remain manageable.

---

## **🚀 Task 2: JSON Schema-based Information Extraction**
We use function calling tools + JSON Schema validation to extract structured fields from user chats:
- Schema defines required fields:
  ```
  {
    "name": "string or null",
    "email": "string or null",
    "phone": "string or null",
    "location": "string or null",
    "age": "integer or null (0–120)"
  }
  ```
- Validator (validate_extraction) checks model outputs against schema using jsonschema.
- Tool function (extract_info) ensures only valid structured data is returned.
- Pipeline:
   - User provides free-text input ("Hi, I’m Arjun Kapoor, reach me at arjun.k@example.com, call +91-9876543210, I’m 29, from Bengaluru").
   - Model calls extract_info with structured arguments.
   - Output is validated against JSON schema.
   - Final result is returned strictly in JSON.

**Example Output:**
```
{
  "name": "Arjun Kapoor",
  "email": "arjun.k@example.com",
  "phone": "+91-9876543210",
  "location": "Bengaluru",
  "age": 29
}
```
