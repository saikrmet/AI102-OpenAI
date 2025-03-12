### **Using Azure OpenAI REST API for DALL-E Image Generation**
*Azure OpenAI enables applications to generate images using DALL·E models via a REST API.*

---

### **Making a REST API Call to DALL·E**
To generate an image, you need:
1. **Azure OpenAI Service Endpoint**
2. **Authorization Key**

You send a **POST request** with these required parameters in a JSON body.

---

### **Required Request Parameters**
| Parameter  | Description |
|------------|-------------|
| **prompt**  | Text description of the image to be generated |
| **n** | Number of images to generate (DALL·E 3 supports only `n=1`) |
| **size** | Image resolution: <br> - **DALL·E 3**: `1024x1024`, `1792x1024`, `1024x1792` <br> - **DALL·E 2**: `256x256`, `512x512`, `1024x1024` |
| **quality** _(optional)_ | Image quality: `standard` (default) or `hd` |
| **style** _(optional)_ | Visual style: `natural` or `vivid` (default: `vivid`) |

---

### **Example API Request**
```bash
curl -X POST "https://YOUR_AZURE_OPENAI_RESOURCE/openai/deployments/YOUR_DEPLOYMENT_NAME/images/generations?api-version=2023-06-01-preview" \
  -H "Content-Type: application/json" \
  -H "api-key: YOUR_API_KEY" \
  -d '{
    "prompt": "A futuristic city skyline at sunset",
    "n": 1,
    "size": "1024x1024",
    "quality": "hd",
    "style": "vivid"
  }'
```

---

### **Example API Response**
```json
{
    "created": 1686780744,
    "data": [
        {
            "url": "<URL of generated image>",
            "revised_prompt": "<prompt that was used>"
        }
    ]
}
```

- **`url`**: Link to the generated PNG image.
- **`revised_prompt`**: Modified prompt used by the system to optimize results.
