# Telegram Bot Commands

## Available Commands

### `/start`
Initialize the bot and get welcome instructions

**Response:**
```
👋 Welcome to LinkedIn Job Matcher!

📄 Please upload your resume (PDF or TXT)

Then provide:
• Job titles you're interested in
• Preferred locations
• Experience level

I'll find matching LinkedIn jobs for you!
```

### `/help`
Show available commands and usage instructions

### `/status`
Check current session status and stored resume data

## Usage Flow

### Step 1: Upload Resume
Send your resume as a file attachment

**Supported formats:**
- ✅ PDF (.pdf) - Most common
- ✅ Text (.txt) - Plain text
- ✅ Word (.docx) - Microsoft Word
- ✅ Markdown (.md) - Markdown format
- ⚠️ Scanned PDFs require OCR (see PDF-SUPPORT.md)

**Example:**
- Attach `resume.pdf`
- Or attach `resume.docx`
- Or attach `resume.txt`

**Bot Response:**
```
✅ Resume processed!

📋 Found:
• 15 skills
• 5 years experience
• Current role: Senior Software Engineer

Now tell me your job preferences...
```

### Step 2: Specify Job Criteria
Send a message with your job preferences in natural language or structured format

**Example 1 (Natural):**
```
I'm looking for Software Engineer or Backend Developer roles 
in San Francisco or Remote positions at Mid to Senior level
```

**Example 2 (Structured):**
```
Job Title: Software Engineer, Backend Developer, Full Stack Developer
Location: San Francisco, New York, Remote
Level: Mid-Level, Senior
```

**Example 3 (Simple):**
```
Job Title: Data Scientist
Location: Remote
Level: Senior
```

### Step 3: Receive Matches
Bot will scrape LinkedIn and return matched jobs

**Example Response:**
```
🎯 Found 5 matching jobs:

1. Senior Software Engineer at Google
📍 San Francisco, CA
✨ Match: 95%
✅ Reasons: Python, AWS, Microservices match
⚠️ Missing: Kubernetes
🔗 https://linkedin.com/jobs/...

2. Backend Developer at Stripe
📍 Remote
✨ Match: 92%
✅ Reasons: Strong backend experience, API design
🔗 https://linkedin.com/jobs/...

...
```

## Advanced Features

### Filter by Salary
```
Job Title: Software Engineer
Location: Remote
Level: Senior
Salary: $150k+
```

### Multiple Searches
```
Search 1:
Job Title: Data Engineer
Location: Seattle

Search 2:
Job Title: ML Engineer
Location: Remote
```

### Save Favorites
```
/save 1
```
Saves job #1 from last results

### View Saved Jobs
```
/saved
```
Shows all saved job listings

## Tips

1. **Resume Format**: Include clear sections for skills, experience, education
2. **Job Titles**: Use common industry terms (e.g., "Software Engineer" not "Code Wizard")
3. **Locations**: Be specific (city names) or use "Remote"
4. **Levels**: Use standard terms (Entry, Mid-Level, Senior, Lead, Principal)
5. **Update Resume**: Upload a new resume anytime to update your profile

## Troubleshooting

### "No jobs found"
- Try broader job titles
- Expand location search
- Check if LinkedIn has listings for your criteria

### "Resume parsing failed"
- Ensure resume is in PDF or TXT format
- Check file size (max 10MB)
- Use standard resume format

### "Rate limit exceeded"
- Wait a few minutes between searches
- Apify has rate limits on free tier

## Privacy

- Resumes are processed in-memory only
- No data is permanently stored
- All communication is encrypted via Telegram
