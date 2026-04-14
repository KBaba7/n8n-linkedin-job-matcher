# Groq API Setup Guide

## Why Groq?

- **FREE Tier**: 30 requests/minute, 14,400 requests/day
- **Ultra Fast**: 10x faster than OpenAI (300+ tokens/sec)
- **Great Models**: Llama 3.3 70B, Mixtral 8x7B, Gemma 2 9B
- **OpenAI Compatible**: Drop-in replacement for OpenAI API

## Getting Your Groq API Key

### Step 1: Sign Up

1. Go to [console.groq.com](https://console.groq.com)
2. Click "Sign Up" (free, no credit card required)
3. Verify your email

### Step 2: Create API Key

1. Navigate to [API Keys](https://console.groq.com/keys)
2. Click "Create API Key"
3. Name it (e.g., "N8N Job Matcher")
4. Copy the key (starts with `gsk_...`)

### Step 3: Add to N8N

1. Open N8N at http://localhost:5678
2. Go to **Credentials** → **Add Credential**
3. Select **HTTP Header Auth**
4. Configure:
   - **Name**: `Groq API`
   - **Header Name**: `Authorization`
   - **Header Value**: `Bearer gsk_your_api_key_here`
5. Save

## Available Models

### Recommended: Llama 3.3 70B Versatile
```json
{
  "model": "llama-3.3-70b-versatile",
  "temperature": 0.1,
  "max_tokens": 2000
}
```
- Best for structured output (JSON)
- Great reasoning capabilities
- Fast inference

### Alternative: Mixtral 8x7B
```json
{
  "model": "mixtral-8x7b-32768",
  "temperature": 0.1,
  "max_tokens": 2000
}
```
- Larger context window (32k tokens)
- Good for long resumes

### Alternative: Llama 3.1 8B
```json
{
  "model": "llama-3.1-8b-instant",
  "temperature": 0.1,
  "max_tokens": 2000
}
```
- Fastest inference
- Good for simple parsing

## Rate Limits

### Free Tier
- **Requests**: 30/minute, 14,400/day
- **Tokens**: 6,000/minute
- **Context**: Up to 32k tokens (model dependent)

### Paid Tier
- **Requests**: 30,000/minute
- **Tokens**: 1,000,000/minute
- **Priority**: Faster queue times

## API Endpoint

```
https://api.groq.com/openai/v1/chat/completions
```

## Example Request

```bash
curl https://api.groq.com/openai/v1/chat/completions \
  -H "Authorization: Bearer $GROQ_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama-3.3-70b-versatile",
    "messages": [
      {
        "role": "system",
        "content": "You are a helpful assistant."
      },
      {
        "role": "user",
        "content": "Extract skills from this resume: ..."
      }
    ],
    "temperature": 0.1,
    "max_tokens": 2000
  }'
```

## N8N Configuration

### HTTP Request Node Settings

**URL**: `https://api.groq.com/openai/v1/chat/completions`

**Method**: `POST`

**Authentication**: `Generic Credential Type` → `HTTP Header Auth`

**Headers**:
```json
{
  "Authorization": "Bearer {{$credentials.groqApi.apiKey}}",
  "Content-Type": "application/json"
}
```

**Body** (JSON):
```json
{
  "model": "llama-3.3-70b-versatile",
  "messages": [
    {
      "role": "system",
      "content": "You are a resume parser."
    },
    {
      "role": "user",
      "content": "{{$json.prompt}}"
    }
  ],
  "temperature": 0.1,
  "max_tokens": 2000
}
```

## Response Format

```json
{
  "id": "chatcmpl-...",
  "object": "chat.completion",
  "created": 1234567890,
  "model": "llama-3.3-70b-versatile",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "{\"skills\": [\"Python\", \"AWS\"], ...}"
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 100,
    "completion_tokens": 50,
    "total_tokens": 150
  }
}
```

## Parsing Response in N8N

```javascript
// Code Node
const response = $input.first().json;
const content = response.choices[0].message.content;
const parsed = JSON.parse(content);

return { json: parsed };
```

## Error Handling

### Rate Limit Error
```json
{
  "error": {
    "message": "Rate limit exceeded",
    "type": "rate_limit_error",
    "code": "rate_limit_exceeded"
  }
}
```

**Solution**: Add retry logic with exponential backoff

### Invalid API Key
```json
{
  "error": {
    "message": "Invalid API key",
    "type": "invalid_request_error"
  }
}
```

**Solution**: Check API key in credentials

## Best Practices

1. **Use Low Temperature**: Set `temperature: 0.1` for consistent JSON output
2. **Set Max Tokens**: Limit to what you need (saves quota)
3. **System Prompts**: Always include clear system instructions
4. **JSON Mode**: Request JSON explicitly in prompts
5. **Error Handling**: Catch and retry on rate limits

## Cost Comparison

| Provider | Free Tier | Cost (1M tokens) |
|----------|-----------|------------------|
| Groq | 14,400 req/day | $0.05 - $0.27 |
| OpenAI | $5 credit | $2.50 - $15.00 |
| Anthropic | None | $3.00 - $15.00 |

## Monitoring Usage

Check usage at: [console.groq.com/settings/usage](https://console.groq.com/settings/usage)

## Support

- **Docs**: [console.groq.com/docs](https://console.groq.com/docs)
- **Discord**: [groq.com/discord](https://groq.com/discord)
- **Status**: [status.groq.com](https://status.groq.com)
