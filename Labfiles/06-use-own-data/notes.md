### **Retrieval Augmented Generation (RAG) with Azure OpenAI**
*RAG enables AI models to reference external data sources for more accurate and contextual responses.*

---

### **How RAG Works in Azure OpenAI**
1. **User sends a prompt** to the model.
2. **Model determines relevant content** and intent.
3. **AI Search queries an index** to find relevant data.
4. **Search results are added** to the model's prompt as context.
5. **Azure OpenAI generates a response** using both the retrieved data and its pretrained knowledge.
6. **Response is returned** to the user, along with relevant data references (if applicable).

---

### **Fine-Tuning vs. RAG**
| **Aspect**          | **Fine-Tuning** | **RAG** |
|---------------------|---------------|--------|
| **Custom Model Training** | Required | Not required |
| **Cost & Time** | High | Low |
| **Use Case** | When model needs to learn from a large dataset | When real-time data reference is needed |
| **Interaction** | Model learns from predefined examples | Model retrieves relevant information dynamically |

**RAG is ideal when real-time and up-to-date information is needed, avoiding costly fine-tuning.**

---

### **Connecting Your Own Data**
- **Data Sources**: Upload data in **Azure AI Studio**, **Blob Storage**, or **AI Search index**.
- **Supported File Formats**: `.md`, `.txt`, `.html`, `.pdf`, `.docx`, `.pptx`
- **Indexing**: Use **Azure AI Search** to chunk and store data for retrieval.
- **Vectorization**: AI Search can convert text into vectors for **semantic search**.

---

### **Improving Search Quality**
- **Enable Semantic Search**: Enhances document retrieval and improves response quality.
- **Use Vectorization**: Converts data into numerical representations for better similarity matching.
- **Optimize Data Preparation**: Use scripts to pre-process large text files for accuracy.

---

### **Token Considerations & Best Practices**
- **Token Usage in RAG**:
  - System messages
  - User prompt
  - Conversation history
  - Retrieved search documents
  - Model’s response
- **Token Limits**:
  - **System messages** are truncated if they exceed the model’s token limit.
  - **Model response is limited to 1500 tokens** when using your own data.
- **Recommendations**:
  - Keep **question length short**.
  - Limit **conversation history**.
  - Use **prompt engineering techniques** like **breaking down tasks** or **chain of thought prompting**.

---

### **Using the API with Your Data**
- **Request JSON Example**:
  ```json
  {
      "dataSources": [
          {
              "type": "AzureCognitiveSearch",
              "parameters": {
                  "endpoint": "<your_search_endpoint>",
                  "key": "<your_search_key>",
                  "indexName": "<your_search_index>"
              }
          }
      ],
      "messages": [
          {
              "role": "system", 
              "content": "You are a helpful assistant assisting users with travel recommendations."
          },
          {
              "role": "user", 
              "content": "I want to go to New York. Where should I stay?"
          }
      ]
  }
  ```
- **API Call Endpoint**:
  ```
  <your_azure_openai_resource>/openai/deployments/<deployment_name>/chat/completions?api-version=<version>
  ```
- **Headers Required**:
  - `Content-Type: application/json`
  - `api-key: <your_api_key>`

- **API Docs**: https://learn.microsoft.com/en-us/azure/ai-services/openai/reference
---

### **Vector Search example with user-assigned managed identity and role info**
```
POST https://{endpoint}/openai/deployments/{deployment-id}/chat/completions?api-version=2024-10-21

{
 "messages": [
  {
   "role": "user",
   "content": "can you tell me how to care for a cat?"
  },
  {
   "role": "assistant",
   "content": "Content of the completion [doc1].",
   "context": {
    "intent": "cat care"
   }
  },
  {
   "role": "user",
   "content": "how about dog?"
  }
 ],
 "data_sources": [
  {
   "type": "azure_search",
   "parameters": {
    "endpoint": "https://your-search-endpoint.search.windows.net/",
    "authentication": {
     "type": "user_assigned_managed_identity",
     "managed_identity_resource_id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{resource-name}"
    },
    "index_name": "{index name}",
    "query_type": "vector",
    "embedding_dependency": {
     "type": "deployment_name",
     "deployment_name": "{embedding deployment name}"
    },
    "in_scope": true,
    "top_n_documents": 5,
    "strictness": 3,
    "role_information": "You are an AI assistant that helps people find information.",
    "fields_mapping": {
     "content_fields_separator": "\\n",
     "content_fields": [
      "content"
     ],
     "filepath_field": "filepath",
     "title_field": "title",
     "url_field": "url",
     "vector_fields": [
      "contentvector"
     ]
    }
   }
  }
 ]
}
```