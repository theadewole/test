# API Interaction with Custom GPT

## Question 1
**Can one use API to interact with a custom GPT?**

### Response:
Yes, you can use an API to interact with a custom GPT model, including those fine-tuned or configured through OpenAI's **Assistants API** or **Custom GPTs** on OpenAI's platform. Here’s how:

### **1. Using OpenAI's Assistants API**
   - You can create a **Custom Assistant** with specific instructions and tools.
   - The API allows you to send prompts and receive responses programmatically.

   **Example (Python)**:
   ```python
   import openai

   openai.api_key = "your-api-key"

   response = openai.ChatCompletion.create(
       model="gpt-4-turbo",
       messages=[
           {"role": "system", "content": "You are a data analytics assistant."},
           {"role": "user", "content": "How do I clean data using Power Query?"}
       ]
   )

   print(response["choices"][0]["message"]["content"])
   ```

### **2. Using OpenAI's Custom GPTs via API**
   - If you have configured a **Custom GPT** in OpenAI’s ChatGPT interface, it can also be accessed through the API.
   - The **Assistants API** allows more structured interactions, such as function calling and memory.

   **Example for Custom GPT Interaction:**
   - After creating a Custom GPT, use its API endpoint.
   - Call it via `openai.ChatCompletion.create()` or `openai.Assistants.create()`.

### **3. Self-Hosted or Fine-Tuned GPT Models**
   - If you train a custom model (e.g., fine-tuned GPT), you can deploy it on your cloud service.
   - Use **FastAPI, Flask**, or other frameworks to expose an API.

   **Example using FastAPI:**
   ```python
   from fastapi import FastAPI
   from openai import OpenAI

   app = FastAPI()
   openai.api_key = "your-api-key"

   @app.post("/chat/")
   async def chat(message: str):
       response = openai.ChatCompletion.create(
           model="your-fine-tuned-model",
           messages=[{"role": "user", "content": message}]
       )
       return {"response": response["choices"][0]["message"]["content"]}
   ```

### **4. Using LangChain for Custom GPT Automation**
   - If your goal is to integrate a Custom GPT with workflows, **LangChain** helps.
   - It enables **memory, external tools, and structured responses**.

   **Example:**
   ```python
   from langchain.chat_models import ChatOpenAI
   from langchain.memory import ConversationBufferMemory
   from langchain.chains import ConversationChain

   chat_model = ChatOpenAI(model="gpt-4-turbo", openai_api_key="your-api-key")
   memory = ConversationBufferMemory()

   conversation = ConversationChain(llm=chat_model, memory=memory)
   response = conversation.predict(input="What are the best practices in financial reporting?")
   print(response)
   ```

---

## Question 2
**I have a custom GPT with this default instruction below. How can I interact with it?**  
[![View Data Dictionary](https://img.shields.io/badge/View%20Action%20Menu-Click%20Here-blue?style=for-the-badge)](https://github.com/theadewole/Prompt_Enginnering/blob/main/wed_29.md)  

### Response:
Since you have a **Custom GPT** with this workflow, you can interact with it via the **OpenAI Assistants API** or the **Chat Completions API**. Below are the different ways to interact with your custom GPT programmatically.

### **1. Using OpenAI's Chat Completions API**
```python
import openai

openai.api_key = "your-api-key"

response = openai.ChatCompletion.create(
    model="gpt-4-turbo",
    messages=[
        {"role": "system", "content": "You are a CDISC mapping assistant following a structured workflow."},
        {"role": "user", "content": "Step 0: I have uploaded the source data for the AE domain. Proceed to Step 1."}
    ]
)

print(response["choices"][0]["message"]["content"])
```

### **2. Using OpenAI Assistants API**
```python
import openai
import time

openai.api_key = "your-api-key"

thread = openai.beta.threads.create()

message = openai.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content="Step 0: Uploading source data for the DM domain. Proceed to Step 1."
)

run = openai.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id="your-assistant-id"
)

while run.status != "completed":
    time.sleep(2)
    run = openai.beta.threads.runs.retrieve(run.id)

messages = openai.beta.threads.messages.list(thread_id=thread.id)
print(messages.data[0].content)
```

---

## Question 3
**With the above approach, can the temperature of the response be adjusted to the desired range?**

### Response:
Yes, you can **adjust the temperature** of the response in all the approaches mentioned above.

### **1. Adjusting Temperature in Chat Completions API**
```python
response = openai.ChatCompletion.create(
    model="gpt-4-turbo",
    temperature=0.2,
    messages=[{"role": "system", "content": "You are a CDISC mapping assistant following a structured workflow."}]
)
```

### **2. Adjusting Temperature in Assistants API**
```python
run = openai.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id="your-assistant-id",
    temperature=0.1  
)
```

### **3. Adjusting Temperature in LangChain**
```python
chat_model = ChatOpenAI(model="gpt-4-turbo", openai_api_key="your-api-key", temperature=0)
```

### **🔥 Best Temperature Settings for Your CDISC Workflow**
| **Use Case** | **Recommended Temperature** |
|-------------|------------------------|
| **CDISC Mapping Workflow** | `0` to `0.2` |
| **General SDTM Assistance** | `0.3` to `0.5` |
| **Exploratory Clinical Data Analysis** | `0.7` to `1.0` |

