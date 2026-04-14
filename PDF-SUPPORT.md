# PDF Resume Support

The workflow now supports both PDF and text file resumes!

## How It Works

### Workflow Flow

```
User uploads file
    ↓
Download Resume
    ↓
Is PDF File? (Check MIME type)
    ↓
   / \
  /   \
PDF    Text/DOCX
 ↓      ↓
Extract Convert
PDF     Text
Text    File
 ↓      ↓
  \   /
   \ /
    ↓
Parse Resume with AI (Groq)
    ↓
Store Resume Data
    ↓
Confirm to User
```

## Supported File Types

### ✅ Fully Supported
- **PDF** (.pdf) - Extracted using N8N's built-in PDF parser
- **Text** (.txt) - Direct text extraction
- **Markdown** (.md) - Treated as text

### ✅ Supported (with N8N Extract node)
- **DOCX** (.docx) - Microsoft Word documents
- **ODT** (.odt) - OpenDocument text
- **RTF** (.rtf) - Rich Text Format

### ⚠️ Limited Support
- **DOC** (.doc) - Old Word format (may need conversion)
- **HTML** (.html) - Extracts text content

## Technical Details

### PDF Extraction Node

The workflow uses N8N's `extractFromFile` node:

```json
{
  "operation": "toText",
  "options": {}
}
```

**Features:**
- Extracts text from PDF pages
- Preserves formatting where possible
- Handles multi-page documents
- Works with most PDF types

**Limitations:**
- Scanned PDFs (images) won't work without OCR
- Complex layouts may lose formatting
- Tables might not parse perfectly

### Text File Handling

For non-PDF files, the workflow:
1. Checks if data is a Buffer
2. Converts to UTF-8 string
3. Passes to AI parser

## File Size Limits

### Telegram Limits
- **Free bots**: 20 MB per file
- **Premium users**: 2 GB per file

### N8N Limits
- **Default**: 50 MB per execution
- **Configurable**: Can be increased in settings

### Groq API Limits
- **Context window**: 32k tokens (~24k words)
- **Typical resume**: 500-2000 tokens
- **Max resume size**: ~20 pages

## Handling Scanned PDFs (OCR)

If users upload scanned PDFs (images), you'll need OCR. Here are options:

### Option 1: Add OCR.space API (Free)

Add this node after "Is PDF File":

```json
{
  "name": "OCR Scanned PDF",
  "type": "n8n-nodes-base.httpRequest",
  "parameters": {
    "method": "POST",
    "url": "https://api.ocr.space/parse/image",
    "sendBody": true,
    "bodyParameters": {
      "parameters": [
        {
          "name": "base64Image",
          "value": "={{$json.data.toString('base64')}}"
        },
        {
          "name": "apikey",
          "value": "YOUR_OCR_SPACE_KEY"
        }
      ]
    }
  }
}
```

**Free tier**: 25,000 requests/month

### Option 2: Use Google Cloud Vision API

```json
{
  "name": "Google Vision OCR",
  "type": "n8n-nodes-base.googleCloudVision",
  "parameters": {
    "operation": "documentTextDetection",
    "binaryData": true
  }
}
```

**Free tier**: 1,000 requests/month

### Option 3: Use Tesseract (Self-hosted)

Add to docker-compose.yml:

```yaml
tesseract:
  image: tesseractshadow/tesseract4re
  volumes:
    - ./uploads:/uploads
```

## Error Handling

### PDF Extraction Failed

```javascript
// Add error handling node
try {
  const text = $json.text;
  if (!text || text.length < 50) {
    throw new Error('PDF extraction failed or empty');
  }
  return { json: { text } };
} catch (error) {
  // Send error message to user
  return {
    json: {
      error: true,
      message: 'Could not extract text from PDF. Please try a text file or different PDF.'
    }
  };
}
```

### Unsupported File Type

The workflow checks MIME type. Add this node:

```javascript
const supportedTypes = [
  'application/pdf',
  'text/plain',
  'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  'application/msword'
];

const mimeType = $json.mimeType;

if (!supportedTypes.includes(mimeType)) {
  return {
    json: {
      error: true,
      message: `Unsupported file type: ${mimeType}. Please upload PDF, TXT, or DOCX.`
    }
  };
}

return { json: $json };
```

## Testing

### Test with Sample Files

1. **PDF Resume**
   ```bash
   # Upload example-resume.pdf via Telegram
   ```

2. **Text Resume**
   ```bash
   # Upload example-resume.txt via Telegram
   ```

3. **DOCX Resume**
   ```bash
   # Upload resume.docx via Telegram
   ```

### Expected Output

After upload, bot should respond:

```
✅ Resume processed!

📋 Found:
• 15 skills
• 5 years experience
• Current role: Senior Software Engineer

Now tell me your job preferences...
```

## Troubleshooting

### "Could not extract text from PDF"

**Causes:**
- Scanned PDF (image-based)
- Encrypted/password-protected PDF
- Corrupted file

**Solutions:**
1. Ask user to export PDF as text
2. Try converting to DOCX first
3. Add OCR support (see above)

### "File too large"

**Solutions:**
1. Ask user to compress PDF
2. Split into multiple pages
3. Convert to text file

### "Parsing failed"

**Causes:**
- Unusual resume format
- Non-English characters
- Corrupted extraction

**Solutions:**
1. Check extracted text in N8N logs
2. Adjust AI prompt for better parsing
3. Ask user for text version

## Best Practices

### For Users
1. **Use text-based PDFs** (not scanned images)
2. **Keep under 5 pages** for best results
3. **Use standard resume format** (chronological)
4. **Avoid complex layouts** (tables, columns)

### For Developers
1. **Log extracted text** for debugging
2. **Add file size checks** before processing
3. **Implement retry logic** for failed extractions
4. **Cache parsed resumes** to avoid re-processing

## Alternative: Use Groq Vision (Future)

When Groq adds vision support, you can send PDF images directly:

```json
{
  "model": "llama-3.2-90b-vision",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "image_url",
          "image_url": {
            "url": "data:image/pdf;base64,{{$json.data.toString('base64')}}"
          }
        },
        {
          "type": "text",
          "text": "Extract resume information..."
        }
      ]
    }
  ]
}
```

## Summary

✅ **PDF support added** via N8N's extractFromFile node
✅ **Text files supported** via buffer conversion
✅ **DOCX supported** via extractFromFile node
✅ **Error handling** for unsupported types
⚠️ **OCR needed** for scanned PDFs (optional add-on)

Users can now upload resumes in any common format!
