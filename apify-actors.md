# Apify LinkedIn Actors

## Available LinkedIn Scrapers

### 1. LinkedIn Jobs Scraper
**Actor ID:** `apify/linkedin-jobs-scraper`

**Features:**
- Search jobs by keywords
- Filter by location
- Experience level filtering
- Company size filtering
- Remote/hybrid options

**Input Parameters:**
```json
{
  "keywords": "Software Engineer",
  "location": "San Francisco",
  "datePosted": "past-week",
  "experienceLevel": ["mid-senior"],
  "jobType": ["full-time", "contract"],
  "remote": true,
  "maxResults": 20
}
```

**Output:**
```json
{
  "title": "Senior Software Engineer",
  "company": "Google",
  "location": "San Francisco, CA",
  "description": "...",
  "salary": "$150k - $200k",
  "postedDate": "2024-01-15",
  "url": "https://linkedin.com/jobs/...",
  "skills": ["Python", "AWS", "Docker"]
}
```

### 2. LinkedIn Profile Scraper
**Actor ID:** `apify/linkedin-profile-scraper`

**Use Case:** Extract skills from LinkedIn profiles for comparison

**Input:**
```json
{
  "profileUrls": ["https://linkedin.com/in/johndoe"],
  "includeSkills": true,
  "includeExperience": true
}
```

### 3. LinkedIn Company Scraper
**Actor ID:** `apify/linkedin-company-scraper`

**Use Case:** Get company details for job postings

**Input:**
```json
{
  "companyUrls": ["https://linkedin.com/company/google"],
  "includeJobs": true
}
```

## MCP Server Integration

### Using Apify MCP Tools

The Apify MCP server provides these tools:

#### 1. `apify_run_actor`
Run any Apify actor

```javascript
{
  "actorId": "apify/linkedin-jobs-scraper",
  "input": {
    "keywords": "Data Scientist",
    "location": "Remote",
    "maxResults": 10
  },
  "waitForFinish": true
}
```

#### 2. `apify_get_dataset_items`
Retrieve scraped data

```javascript
{
  "datasetId": "abc123",
  "format": "json",
  "clean": true
}
```

#### 3. `apify_list_actors`
Browse available actors

```javascript
{
  "category": "JOBS",
  "limit": 10
}
```

## N8N Integration Examples

### Example 1: Basic Job Search

```javascript
// HTTP Request Node
POST https://api.apify.com/v2/acts/apify~linkedin-jobs-scraper/runs
Headers: {
  "Authorization": "Bearer YOUR_TOKEN"
}
Body: {
  "keywords": "{{$json.jobTitle}}",
  "location": "{{$json.location}}",
  "maxResults": 20
}
```

### Example 2: Advanced Filtering

```javascript
// Function Node
const input = {
  keywords: $json.jobTitles.join(' OR '),
  location: $json.location,
  datePosted: "past-week",
  experienceLevel: $json.levels,
  remote: $json.includeRemote,
  salary: {
    min: $json.minSalary
  },
  maxResults: 50
};

return { json: input };
```

### Example 3: Batch Processing

```javascript
// Loop through multiple searches
const searches = [
  { keywords: "Software Engineer", location: "SF" },
  { keywords: "Data Scientist", location: "NYC" },
  { keywords: "Product Manager", location: "Remote" }
];

return searches.map(search => ({ json: search }));
```

## Rate Limits

### Free Tier
- 100 actor runs/month
- 10 concurrent runs
- 1 hour max runtime

### Paid Tier
- Unlimited runs
- 50 concurrent runs
- Custom runtime limits

## Best Practices

1. **Use Specific Keywords**: "Senior Python Developer" > "Developer"
2. **Limit Results**: Start with 20-50 results
3. **Cache Results**: Store in database to avoid re-scraping
4. **Handle Errors**: LinkedIn may block aggressive scraping
5. **Respect Rate Limits**: Add delays between requests

## Error Handling

```javascript
// N8N Error Handler
try {
  const result = await apifyRun();
  return result;
} catch (error) {
  if (error.code === 'RATE_LIMIT') {
    // Wait and retry
    await sleep(60000);
    return await apifyRun();
  }
  throw error;
}
```

## Cost Optimization

1. **Filter Early**: Use precise search criteria
2. **Incremental Updates**: Only scrape new jobs
3. **Shared Datasets**: Reuse scraped data
4. **Scheduled Runs**: Run during off-peak hours
