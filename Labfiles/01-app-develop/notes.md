### **Azure OpenAI Endpoints & Features**
*Azure OpenAI provides REST API and SDK access for deploying and interacting with AI models.*

---

### **Available Endpoints**
- **ChatCompletion**: Generates chat responses based on input conversation with role-specified messages.
- **Embeddings**: Converts input text into vector representations for AI-powered search and recommendations.

---

### **ChatCompletion Endpoint**
- Input: A structured chat conversation with clearly defined roles.
- Used for:
  - Realistic AI conversations with context.
  - Summarization, entity extraction, and other text-processing tasks.
- **Example API Call (ChatCompletion)**:
  ```bash
  curl https://YOUR_ENDPOINT_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/chat/completions?api-version=2023-03-15-preview \
    -H "Content-Type: application/json" \
    -H "api-key: YOUR_API_KEY" \
    -d '{
      "messages": [
        {"role": "system", "content": "You are a helpful assistant, teaching people about AI."},
        {"role": "user", "content": "Does Azure OpenAI support multiple languages?"}
      ]}'
  ```
- **Example Response**:
  ```json
  {
      "id": "chatcmpl-6v7mkQj980V1yBec6ETrKPRqFjNw9",
      "model": "gpt-35-turbo",
      "choices": [
          {
              "message": {
                  "role": "assistant",
                  "content": "Yes, Azure OpenAI supports several languages."
              },
              "finish_reason": "stop"
          }
      ]
  }
  ```
- Supports additional parameters like **temperature**, **max_tokens**, etc.

---

### **Embeddings Endpoint**
- **Purpose**: Generates numerical representations (vectors) from text for AI search and machine learning applications.
- **Example API Call (Embeddings)**:
  ```bash
  curl https://YOUR_ENDPOINT_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/embeddings?api-version=2022-12-01 \
    -H "Content-Type: application/json" \
    -H "api-key: YOUR_API_KEY" \
    -d '{"input": "The food was delicious and the waiter..."}'
  ```
- **Example Response**:
  ```json
  {
    "object": "list",
    "data": [
      {
        "object": "embedding",
        "embedding": [0.0173, -0.0291, ..., 0.0134],
        "index": 0
      }
    ],
    "model": "text-embedding-ada:002"
  }
  ```
- **Models**: Choose models like `text-embedding-ada-002` for embeddings or `text-similarity` models for similarity searches.

---

### **Using Azure SDK**
- **Required Parameters**: `endpoint`, `api_key`, `deployment_name`
- **Python Example**:
  ```python
  from openai import AzureOpenAI

  client = AzureOpenAI(
      azure_endpoint='<YOUR_ENDPOINT_NAME>', 
      api_key='<YOUR_API_KEY>',  
      api_version="2024-02-15-preview"
  )

  response = client.chat.completions.create(
      model='<YOUR_DEPLOYMENT_NAME>',
      messages=[
          {"role": "system", "content": "You are a helpful assistant."},
          {"role": "user", "content": "What is Azure OpenAI?"}
      ]
  )

  print("Response: " + response.choices[0].message.content)
  ```
  
---

### **Enhancing Model Responses**
#### **1. Primary, Supporting & Grounding Content**
- **Primary Content**: Subject of the query (e.g., a sentence for translation).
- **Supporting Content**: Additional details influencing the response (e.g., names, dates).
- **Grounding Content**: Reliable reference material (e.g., company FAQ, unpublished documents).

#### **2. System Messages**
- Guides the model's behavior, tone, or response format.
- **Examples**:
  - `"You are a command-line terminal. Respond as cmd.exe would."`
  - `"Translate only from English to Spanish. Do not reply in any other language."`
  - `"Act as a motivational speaker, offering encouragement."`

#### **3. Conversation History**
- Retains context across multiple interactions.
- Can be set in Parameters section of chat to include previous messages.
- Can utilize summarization of past conversation to reduce input tokens for subsequent query.

#### **4. Few-Shot Learning**
- Provides example conversations to teach the model a response pattern.
- **Example**:
  ```text
  User: That was an awesome experience
  Assistant: positive
  User: I won't do that again
  Assistant: negative
  User: You can't miss this
  Assistant: ???
  ```
- Helps AI learn how to classify or continue patterns.

---

### **Code Generation & Editing**
Azure OpenAI models can:
- **Write new code** from natural language prompts.
- **Translate code** between languages.
- **Understand & document unknown code**.
- **Fix bugs and improve performance**.
- **Refactor code** for efficiency.
