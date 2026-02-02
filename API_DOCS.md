# 🔌 API Documentation - AI Resume Builder Optimizer

Future API endpoints untuk integrasi eksternal dan scalability.

---

## 📋 Overview

API ini akan memungkinkan integrasi dengan:
- LinkedIn untuk auto-pull profile data
- Google Drive untuk save/sync resume
- Job boards untuk auto-apply
- Email services untuk kirim resume
- Analytics platforms untuk tracking

**Base URL** (Future): `https://api.ai-resume-builder.com/v1`

**Authentication**: Bearer Token (JWT)

**Rate Limits**:
- Free tier: 100 requests/day
- Premium tier: 10,000 requests/day
- Enterprise: Unlimited

---

## 🔐 Authentication

### Register User
```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secure_password",
  "name": "John Doe"
}
```

**Response**:
```json
{
  "success": true,
  "user_id": "usr_1234567890",
  "message": "Registration successful"
}
```

### Login
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secure_password"
}
```

**Response**:
```json
{
  "success": true,
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600
}
```

---

## 📄 Resume Operations

### Create Resume
```http
POST /resumes
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_data": {
    "nama": "John Doe",
    "email": "john@example.com",
    "telepon": "+62 812 3456 7890",
    "pendidikan": "S1 Computer Science - UI",
    "pengalaman": "Software Engineer at...",
    "skills": "Python, JavaScript, React"
  },
  "params": {
    "level_pengalaman": "mid-level",
    "industri": "IT/Technology",
    "bahasa": "Indonesia",
    "panjang_resume": "1-2 halaman",
    "tone": "professional"
  }
}
```

**Response**:
```json
{
  "success": true,
  "resume_id": "res_1234567890",
  "created_at": "2026-02-02T10:30:00Z",
  "message": "Resume created successfully"
}
```

### Get Resume
```http
GET /resumes/{resume_id}
Authorization: Bearer {access_token}
```

**Response**:
```json
{
  "success": true,
  "resume": {
    "id": "res_1234567890",
    "resume_data": {...},
    "params": {...},
    "created_at": "2026-02-02T10:30:00Z",
    "updated_at": "2026-02-02T11:00:00Z"
  }
}
```

### Update Resume
```http
PUT /resumes/{resume_id}
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_data": {
    "skills": "Python, JavaScript, React, Node.js, Docker"
  }
}
```

**Response**:
```json
{
  "success": true,
  "resume_id": "res_1234567890",
  "updated_at": "2026-02-02T12:00:00Z",
  "message": "Resume updated successfully"
}
```

### Delete Resume
```http
DELETE /resumes/{resume_id}
Authorization: Bearer {access_token}
```

**Response**:
```json
{
  "success": true,
  "message": "Resume deleted successfully"
}
```

### List User Resumes
```http
GET /resumes?page=1&limit=10
Authorization: Bearer {access_token}
```

**Response**:
```json
{
  "success": true,
  "resumes": [
    {
      "id": "res_1234567890",
      "nama": "John Doe",
      "created_at": "2026-02-02T10:30:00Z",
      "last_ats_score": 85.3
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25,
    "total_pages": 3
  }
}
```

---

## 🔍 Job Description Analysis

### Analyze Job Description
```http
POST /analyze/job-description
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "job_description": "We are looking for a Python developer...",
  "top_keywords": 20
}
```

**Response**:
```json
{
  "success": true,
  "keywords": [
    {
      "keyword": "python",
      "score": 0.451,
      "category": "technical_skill"
    },
    {
      "keyword": "machine learning",
      "score": 0.398,
      "category": "technical_skill"
    }
  ],
  "total_keywords": 20,
  "analysis_time": 1.23
}
```

---

## 📊 ATS Score Calculation

### Calculate ATS Score
```http
POST /analyze/ats-score
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_id": "res_1234567890",
  "job_description": "Looking for Python developer..."
}
```

**Response**:
```json
{
  "success": true,
  "analysis": {
    "total_score": 85.3,
    "keyword_score": 42.5,
    "section_score": 27.0,
    "tone_score": 9.0,
    "length_score": 10.0,
    "confidence_interval": 8,
    "ats_compliant": true,
    "missing_keywords": [
      "docker",
      "kubernetes",
      "aws"
    ],
    "recommendations": [
      {
        "priority": "MEDIUM",
        "action": "Add missing keywords: docker, kubernetes",
        "impact": "+10-15% score"
      }
    ]
  },
  "benchmark": {
    "industry_average": 80,
    "percentile": 75
  }
}
```

---

## ✨ AI Content Generation

### Generate Professional Summary
```http
POST /generate/summary
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_data": {
    "pengalaman": "3 years as Software Engineer",
    "skills": "Python, React, Node.js"
  },
  "params": {
    "level_pengalaman": "mid-level",
    "industri": "IT/Technology",
    "tone": "professional"
  }
}
```

**Response**:
```json
{
  "success": true,
  "summary": "Mid-level IT/Technology professional with 3 years of experience...",
  "word_count": 45,
  "generation_time": 0.8
}
```

### Generate Experience Bullets
```http
POST /generate/experience
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "job_title": "Software Engineer",
  "company": "PT Tech Indonesia",
  "responsibilities": "Developed web apps, led team",
  "level": "mid-level",
  "tone": "professional"
}
```

**Response**:
```json
{
  "success": true,
  "bullets": [
    "Developed and deployed 5+ web applications using React and Node.js",
    "Led cross-functional team of 3 developers in Agile environment",
    "Improved application performance by 40% through optimization"
  ]
}
```

---

## 📥 Export & Download

### Export to PDF
```http
POST /export/pdf
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_id": "res_1234567890",
  "template": "modern"
}
```

**Response**:
```json
{
  "success": true,
  "download_url": "https://cdn.ai-resume-builder.com/exports/res_1234567890.pdf",
  "expires_at": "2026-02-03T10:30:00Z"
}
```

### Export to Word
```http
POST /export/word
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_id": "res_1234567890",
  "template": "classic"
}
```

**Response**:
```json
{
  "success": true,
  "download_url": "https://cdn.ai-resume-builder.com/exports/res_1234567890.docx",
  "expires_at": "2026-02-03T10:30:00Z"
}
```

---

## 🔗 Third-Party Integrations

### LinkedIn Integration

#### Import from LinkedIn
```http
POST /integrations/linkedin/import
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "linkedin_profile_url": "https://linkedin.com/in/johndoe",
  "linkedin_access_token": "AQV..."
}
```

**Response**:
```json
{
  "success": true,
  "imported_data": {
    "nama": "John Doe",
    "email": "john@example.com",
    "pendidikan": [...],
    "pengalaman": [...],
    "skills": [...]
  },
  "resume_id": "res_1234567890"
}
```

### Google Drive Integration

#### Save to Google Drive
```http
POST /integrations/google-drive/save
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_id": "res_1234567890",
  "format": "pdf",
  "google_access_token": "ya29..."
}
```

**Response**:
```json
{
  "success": true,
  "file_id": "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms",
  "file_url": "https://drive.google.com/file/d/...",
  "message": "Resume saved to Google Drive successfully"
}
```

### Email Integration

#### Send Resume via Email
```http
POST /integrations/email/send
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_id": "res_1234567890",
  "to": "recruiter@company.com",
  "subject": "Application for Software Engineer Position",
  "body": "Dear Hiring Manager, ...",
  "format": "pdf"
}
```

**Response**:
```json
{
  "success": true,
  "message_id": "msg_1234567890",
  "sent_at": "2026-02-02T14:30:00Z",
  "message": "Resume sent successfully"
}
```

---

## 📊 Analytics & Insights

### Get User Analytics
```http
GET /analytics/user
Authorization: Bearer {access_token}
```

**Response**:
```json
{
  "success": true,
  "analytics": {
    "total_resumes": 12,
    "average_ats_score": 82.5,
    "total_exports": 45,
    "most_used_template": "modern",
    "industries": {
      "IT/Technology": 8,
      "Marketing": 3,
      "Finance": 1
    },
    "score_trend": [
      {"date": "2026-01-01", "avg_score": 75},
      {"date": "2026-02-01", "avg_score": 82.5}
    ]
  }
}
```

### Get Industry Benchmarks
```http
GET /analytics/benchmarks?industry=IT/Technology&level=mid-level
Authorization: Bearer {access_token}
```

**Response**:
```json
{
  "success": true,
  "benchmarks": {
    "industry": "IT/Technology",
    "level": "mid-level",
    "average_score": 80,
    "median_score": 82,
    "top_keywords": [
      "python",
      "javascript",
      "react",
      "node.js",
      "sql"
    ],
    "sample_size": 15420
  }
}
```

---

## 🎯 Recommendations API

### Get Personalized Recommendations
```http
POST /recommendations
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "resume_id": "res_1234567890",
  "job_description": "Looking for Python developer...",
  "analysis_result": {
    "total_score": 75.5,
    "missing_keywords": ["docker", "kubernetes"]
  }
}
```

**Response**:
```json
{
  "success": true,
  "recommendations": [
    {
      "priority": "HIGH",
      "action": "Add missing keywords: docker, kubernetes",
      "impact": "+15-20% score",
      "implementation": "Add these skills to your Skills section and mention Docker/Kubernetes experience in relevant work experience bullets"
    },
    {
      "priority": "MEDIUM",
      "action": "Improve professional summary",
      "impact": "+5-10% score",
      "implementation": "Include industry-specific keywords and quantifiable achievements"
    }
  ]
}
```

---

## 🔔 Webhooks (Future)

### Register Webhook
```http
POST /webhooks
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "url": "https://your-app.com/webhook",
  "events": ["resume.created", "ats_score.calculated", "export.completed"]
}
```

**Response**:
```json
{
  "success": true,
  "webhook_id": "whk_1234567890",
  "secret": "whsec_1234567890abcdef"
}
```

### Webhook Event Examples

**resume.created**:
```json
{
  "event": "resume.created",
  "timestamp": "2026-02-02T10:30:00Z",
  "data": {
    "resume_id": "res_1234567890",
    "user_id": "usr_1234567890"
  }
}
```

**ats_score.calculated**:
```json
{
  "event": "ats_score.calculated",
  "timestamp": "2026-02-02T10:35:00Z",
  "data": {
    "resume_id": "res_1234567890",
    "score": 85.3,
    "ats_compliant": true
  }
}
```

---

## ⚠️ Error Responses

### Standard Error Format
```json
{
  "success": false,
  "error": {
    "code": "INVALID_TOKEN",
    "message": "The provided authentication token is invalid or expired",
    "details": "Token expired at 2026-02-02T09:00:00Z"
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_TOKEN` | 401 | Invalid or expired authentication token |
| `INSUFFICIENT_PERMISSIONS` | 403 | User lacks required permissions |
| `RESOURCE_NOT_FOUND` | 404 | Requested resource doesn't exist |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |
| `VALIDATION_ERROR` | 422 | Invalid request data |
| `INTERNAL_ERROR` | 500 | Server error |

---

## 📚 SDK Examples

### Python SDK
```python
from ai_resume_builder import Client

# Initialize client
client = Client(api_key="your_api_key")

# Create resume
resume = client.resumes.create(
    resume_data={
        "nama": "John Doe",
        "email": "john@example.com",
        ...
    },
    params={
        "level_pengalaman": "mid-level",
        "industri": "IT/Technology"
    }
)

# Calculate ATS score
analysis = client.analyze.ats_score(
    resume_id=resume.id,
    job_description="Looking for Python developer..."
)

print(f"ATS Score: {analysis.total_score}%")

# Export to PDF
pdf_url = client.export.pdf(
    resume_id=resume.id,
    template="modern"
)

print(f"Download PDF: {pdf_url}")
```

### JavaScript SDK
```javascript
import { AIResumeBuilder } from 'ai-resume-builder-sdk';

// Initialize client
const client = new AIResumeBuilder({ apiKey: 'your_api_key' });

// Create resume
const resume = await client.resumes.create({
  resumeData: {
    nama: 'John Doe',
    email: 'john@example.com',
    ...
  },
  params: {
    level_pengalaman: 'mid-level',
    industri: 'IT/Technology'
  }
});

// Calculate ATS score
const analysis = await client.analyze.atsScore({
  resumeId: resume.id,
  jobDescription: 'Looking for Python developer...'
});

console.log(`ATS Score: ${analysis.totalScore}%`);

// Export to PDF
const pdfUrl = await client.export.pdf({
  resumeId: resume.id,
  template: 'modern'
});

console.log(`Download PDF: ${pdfUrl}`);
```

---

## 🔄 Versioning

API uses semantic versioning. Current version: **v1**

**Breaking changes** will be released as new major versions (v2, v3, etc.)

**Non-breaking changes** (new endpoints, optional fields) are added to current version.

---

## 📞 Support

For API support, contact:

**Email**: api-support@ai-resume-builder.com  
**Documentation**: https://docs.ai-resume-builder.com  
**Status Page**: https://status.ai-resume-builder.com

---

**Note**: This API documentation is for future implementation. Current version (v1.0) is web-based only.

Created by Ary HH  
© 2026 AI Resume Builder Optimizer
