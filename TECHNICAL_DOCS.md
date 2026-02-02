# 🔧 Technical Documentation - AI Resume Builder Optimizer

Developer documentation untuk memahami arsitektur, algoritma, dan implementasi teknis.

---

## 📐 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     User Interface (Streamlit)               │
│  ┌──────────┬──────────┬──────────┬──────────┬──────────┐  │
│  │ Input    │ Job Desc │ Generate │ Analytics│ Recommend│  │
│  │ Data     │ Analysis │ Resume   │ & Score  │ -ations  │  │
│  └──────────┴──────────┴──────────┴──────────┴──────────┘  │
└─────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                      Core Processing Layer                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  NLP Processing (TF-IDF, Cosine Similarity)          │  │
│  │  • Keyword Extraction                                 │  │
│  │  • Text Similarity Calculation                        │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Scoring Engine                                       │  │
│  │  • ATS Score Calculation                              │  │
│  │  • Section Completeness Check                         │  │
│  │  • Tone Fit Analysis                                  │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Content Generation                                   │  │
│  │  • AI Summary Generator                               │  │
│  │  • Recommendation Engine                              │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                      Export Layer                            │
│  ┌──────────┬──────────┬──────────┬──────────────────────┐ │
│  │   PDF    │   Word   │   CSV    │   Future: LinkedIn   │ │
│  │  (FPDF)  │  (docx)  │ (pandas) │   API, Google Drive  │ │
│  └──────────┴──────────┴──────────┴──────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧮 Core Algorithms

### 1. Keyword Extraction (TF-IDF)

**Algorithm**: Term Frequency-Inverse Document Frequency

**Implementation**:
```python
from sklearn.feature_extraction.text import TfidfVectorizer

def extract_keywords(text, top_n=20):
    # Preprocessing
    text = text.lower()
    text = re.sub(r'[^\w\s]', ' ', text)
    
    # TF-IDF vectorization
    vectorizer = TfidfVectorizer(
        max_features=top_n,
        stop_words='english',
        ngram_range=(1, 2)  # Unigrams and bigrams
    )
    
    tfidf_matrix = vectorizer.fit_transform([text])
    feature_names = vectorizer.get_feature_names_out()
    scores = tfidf_matrix.toarray()[0]
    
    # Sort by score
    keywords = [(feature_names[i], scores[i]) 
                for i in range(len(feature_names))]
    keywords.sort(key=lambda x: x[1], reverse=True)
    
    return keywords
```

**Why TF-IDF?**
- ✅ Industry standard for keyword extraction
- ✅ Balances frequency with uniqueness
- ✅ Works well with job descriptions (technical terms get higher weight)
- ✅ No external API needed

**Parameters**:
- `max_features=20`: Top 20 keywords
- `ngram_range=(1,2)`: Single words + two-word phrases
- `stop_words='english'`: Remove common words (the, is, at, etc.)

**Example Output**:
```python
[
    ('python', 0.451),
    ('machine learning', 0.398),
    ('data analysis', 0.375),
    ('sql', 0.342),
    ...
]
```

---

### 2. Keyword Overlap Calculation (Cosine Similarity)

**Algorithm**: Cosine similarity between TF-IDF vectors

**Formula**:
```
similarity = (A · B) / (||A|| × ||B||)

where:
A = TF-IDF vector of resume
B = TF-IDF vector of job description
· = dot product
|| || = vector magnitude
```

**Implementation**:
```python
from sklearn.metrics.pairwise import cosine_similarity

def calculate_keyword_overlap(resume_text, job_desc_text):
    vectorizer = TfidfVectorizer(
        stop_words='english',
        ngram_range=(1, 2)
    )
    
    # Create TF-IDF matrix
    tfidf_matrix = vectorizer.fit_transform([resume_text, job_desc_text])
    
    # Calculate similarity (0-1 range)
    similarity = cosine_similarity(
        tfidf_matrix[0:1],  # Resume
        tfidf_matrix[1:2]   # Job desc
    )[0][0]
    
    # Extract missing keywords
    job_keywords = set([kw[0] for kw in extract_keywords(job_desc_text, 30)])
    resume_keywords = set([kw[0] for kw in extract_keywords(resume_text, 30)])
    missing_keywords = list(job_keywords - resume_keywords)[:10]
    
    return similarity, missing_keywords
```

**Interpretation**:
- `similarity >= 0.7`: Excellent match
- `0.5 <= similarity < 0.7`: Good match
- `0.3 <= similarity < 0.5`: Moderate match
- `similarity < 0.3`: Poor match

---

### 3. ATS Score Formula

**Complete Formula**:
```
Total_Score = (Keyword_Score × 50%) + 
              (Section_Score × 30%) + 
              (Tone_Score × 10%) + 
              (Length_Score × 10%)

where:
Keyword_Score = cosine_similarity(resume, job_desc) × 50
Section_Score = (completed_sections / total_required) × 100 × 0.30
Tone_Score = tone_industry_match × 0.10
Length_Score = 10 if page_count <= 2 else 5

Max score = 100
```

**Implementation**:
```python
def calculate_ats_score(resume_data, job_desc, params):
    # Combine resume text
    resume_text = f"{resume_data.get('summary', '')} " \
                  f"{resume_data.get('pengalaman', '')} " \
                  f"{resume_data.get('skills', '')}"
    
    # 1. Keyword Overlap (50%)
    similarity, missing = calculate_keyword_overlap(resume_text, job_desc)
    keyword_score = similarity * 50
    
    # 2. Section Completeness (30%)
    required = ['nama', 'email', 'pendidikan', 'pengalaman', 'skills']
    completed = sum(1 for s in required if resume_data.get(s))
    section_score = (completed / len(required)) * 100 * 0.30
    
    # 3. Tone Fit (10%)
    tone_score = calculate_tone_fit(params['tone'], params['industri']) * 0.10
    
    # 4. Length Compliance (10%)
    word_count = len(resume_text.split())
    page_count = word_count / 300  # ~300 words per page
    length_score = 10 if page_count <= 2 else 5
    
    total = min(keyword_score + section_score + tone_score + length_score, 100)
    
    # Confidence interval
    ci = 10 if params['level'] == 'entry-level' else \
         8 if params['level'] == 'mid-level' else 5
    
    return {
        'total_score': round(total, 1),
        'keyword_score': round(keyword_score, 1),
        'section_score': round(section_score, 1),
        'tone_score': round(tone_score, 1),
        'length_score': round(length_score, 1),
        'missing_keywords': missing,
        'confidence_interval': ci,
        'ats_compliant': total >= 70 and page_count <= 2
    }
```

**Score Weights Rationale**:
- **50% Keyword Match**: Most critical for ATS parsing
- **30% Section Complete**: Structure matters for ATS
- **10% Tone Fit**: Important but subjective
- **10% Length**: Standard but less critical

---

### 4. Tone-Industry Mapping

**Logic**:
```python
def calculate_tone_fit(tone, industri):
    tone_industry_map = {
        'professional': ['finance', 'consulting', 'legal', 'healthcare'],
        'creative': ['marketing', 'design', 'media', 'arts'],
        'technical': ['it', 'engineering', 'data science', 'technology']
    }
    
    industri_lower = industri.lower()
    
    # Perfect match = 100
    for tone_type, industries in tone_industry_map.items():
        if tone.lower() == tone_type:
            for ind in industries:
                if ind in industri_lower:
                    return 100
            return 70  # Partial match
    
    return 80  # Neutral
```

**Examples**:
- Professional tone + Finance industry = 100
- Creative tone + Marketing industry = 100
- Professional tone + Marketing industry = 70
- Technical tone + Unknown industry = 80

---

## 🎨 Content Generation

### AI Summary Generator

**Template-Based Approach**:
```python
def generate_ai_summary(params, resume_data):
    templates = {
        'entry-level': 
            "{level} professional di bidang {industri} dengan "
            "passion untuk belajar dan berkontribusi. Memiliki dasar "
            "yang kuat dalam {skills}.",
        
        'mid-level': 
            "Profesional {industri} berpengalaman dengan track record "
            "yang solid dalam {pengalaman}. Mahir dalam {skills}.",
        
        'senior-level': 
            "Senior {industri} professional dengan pengalaman ekstensif "
            "dalam leadership dan strategic planning. Expert dalam {skills}."
    }
    
    base = templates.get(params['level'], templates['mid-level'])
    
    # Customize by tone
    if params['tone'] == 'creative':
        base = "🚀 " + base + " Selalu siap untuk challenge baru!"
    elif params['tone'] == 'technical':
        base += " Focus pada technical excellence dan best practices."
    
    # Format with data
    summary = base.format(
        level=params['level'],
        industri=params['industri'],
        skills=resume_data.get('skills', 'berbagai skills relevan'),
        pengalaman=resume_data.get('pengalaman', 'berbagai proyek')
    )
    
    return summary
```

**Future Enhancement**: Replace with actual LLM (GPT-4, Claude, etc.)

---

### Recommendation Engine

**Priority Logic**:
```python
def generate_recommendations(analysis_result, params):
    recommendations = []
    score = analysis_result['total_score']
    
    # HIGH Priority: Critical issues
    if score < 70:
        recommendations.append({
            'priority': 'HIGH',
            'action': f"Tambahkan keyword: {', '.join(missing[:5])}",
            'impact': '+15-20% score'
        })
    
    # MEDIUM Priority: Important improvements
    if len(missing_keywords) > 5:
        recommendations.append({
            'priority': 'MEDIUM',
            'action': f"Integrasikan {len(missing)} missing keywords",
            'impact': '+10-15% score'
        })
    
    # LOW Priority: Nice to have
    if score >= 70 and score < 85:
        recommendations.append({
            'priority': 'LOW',
            'action': "Optimalkan formatting dan action verbs",
            'impact': '+5-10% score'
        })
    
    return recommendations
```

---

## 📊 Data Flow

### Session State Management

Streamlit uses `st.session_state` for persistence:

```python
# Initialize
if 'resume_data' not in st.session_state:
    st.session_state.resume_data = {}

# Store data
st.session_state.resume_data = {
    'nama': 'John Doe',
    'email': 'john@example.com',
    ...
}

# Access data
if st.session_state.resume_data:
    name = st.session_state.resume_data['nama']
```

**Key Session Variables**:
- `resume_data`: User input data
- `job_desc`: Job description text
- `job_keywords`: Extracted keywords
- `analysis_result`: ATS score results
- `language`: UI language preference

---

## 🔄 File Export

### PDF Generation (FPDF)

```python
def create_pdf_resume(resume_data, params, template='modern'):
    pdf = FPDF()
    pdf.add_page()
    
    # Header
    pdf.set_font('Arial', 'B', 24)
    pdf.cell(0, 10, resume_data['nama'], ln=True, align='C')
    
    # Contact info
    pdf.set_font('Arial', '', 10)
    pdf.cell(0, 6, resume_data['email'], ln=True, align='C')
    
    # Sections
    sections = [
        ('PROFESSIONAL SUMMARY', 'summary'),
        ('WORK EXPERIENCE', 'pengalaman'),
        ('EDUCATION', 'pendidikan'),
        ('SKILLS', 'skills')
    ]
    
    for title, key in sections:
        if resume_data.get(key):
            pdf.set_font('Arial', 'B', 14)
            pdf.cell(0, 8, title, ln=True)
            pdf.set_font('Arial', '', 10)
            pdf.multi_cell(0, 5, resume_data[key])
            pdf.ln(3)
    
    return pdf.output(dest='S').encode('latin-1')
```

**Limitations**:
- Limited font support (Arial, Times, Courier)
- Basic formatting only
- No advanced layout features

**Future**: Consider using ReportLab for advanced PDF generation

### Word Generation (python-docx)

```python
def create_word_resume(resume_data, params, template='modern'):
    doc = Document()
    
    # Header
    header = doc.add_heading(resume_data['nama'], 0)
    header.alignment = WD_ALIGN_PARAGRAPH.CENTER
    
    # Contact
    contact = doc.add_paragraph()
    contact.add_run(f"{resume_data['email']} | {resume_data['telepon']}")
    contact.alignment = WD_ALIGN_PARAGRAPH.CENTER
    
    # Sections
    for title, key in sections:
        if resume_data.get(key):
            doc.add_heading(title, 2)
            doc.add_paragraph(resume_data[key])
    
    # Save to BytesIO
    doc_io = io.BytesIO()
    doc.save(doc_io)
    doc_io.seek(0)
    return doc_io.getvalue()
```

**Advantages**:
- Editable by user
- ATS-friendly
- Support formatting (bold, italic, lists)

---

## 📈 Visualization

### Score Breakdown Chart

```python
def plot_score_breakdown(analysis_result):
    fig, ax = plt.subplots(figsize=(10, 6))
    
    categories = ['Keyword\nMatch', 'Section\nComplete', 
                  'Tone\nFit', 'Length\nCompliance']
    scores = [
        analysis_result['keyword_score'],
        analysis_result['section_score'],
        analysis_result['tone_score'],
        analysis_result['length_score']
    ]
    colors = ['#667eea', '#764ba2', '#f093fb', '#4facfe']
    
    bars = ax.bar(categories, scores, color=colors, alpha=0.8)
    
    # Add value labels
    for bar, score in zip(bars, scores):
        height = bar.get_height()
        ax.text(bar.get_x() + bar.get_width()/2., height,
                f'{score:.1f}', ha='center', va='bottom')
    
    ax.set_ylabel('Score')
    ax.set_title('Resume Score Breakdown')
    ax.set_ylim(0, 60)
    ax.grid(axis='y', alpha=0.3)
    
    return fig
```

---

## 🔐 Security Considerations

### Current Implementation
- ✅ **No external data upload**: All processing client-side
- ✅ **Session-based**: Data clears on browser close
- ✅ **No authentication**: Simplicity for MVP

### For Production (Future)
- 🔒 **Add user authentication** (OAuth, JWT)
- 🔒 **Encrypt sensitive data** at rest
- 🔒 **Rate limiting** to prevent abuse
- 🔒 **Input sanitization** for XSS protection
- 🔒 **HTTPS only** for deployment

**Example Rate Limiting**:
```python
from functools import lru_cache
import time

@lru_cache(maxsize=100)
def rate_limit_check(user_id, action):
    # Implement rate limiting logic
    pass
```

---

## 🚀 Performance Optimization

### Caching

Streamlit provides `@st.cache_data` for expensive operations:

```python
@st.cache_data
def extract_keywords(text, top_n=20):
    # This function result is cached
    # Only recomputes if inputs change
    ...
```

**What to cache**:
- ✅ Keyword extraction (text → keywords)
- ✅ TF-IDF vectorization
- ✅ Visualization generation

**What NOT to cache**:
- ❌ User input processing
- ❌ Session-specific data
- ❌ Random/time-dependent operations

### Optimization Tips

1. **Lazy Loading**:
   ```python
   # Don't load heavy libraries until needed
   if button_clicked:
       import heavy_library
       result = heavy_library.process()
   ```

2. **Batch Processing**:
   ```python
   # Process multiple items at once
   results = [process(item) for item in batch]
   ```

3. **Memory Management**:
   ```python
   # Clear large objects when done
   del large_dataframe
   import gc
   gc.collect()
   ```

---

## 🧪 Testing

### Unit Tests (Future)

```python
import unittest

class TestATS(unittest.TestCase):
    def test_keyword_extraction(self):
        text = "Python developer with machine learning experience"
        keywords = extract_keywords(text, top_n=5)
        self.assertEqual(len(keywords), 5)
        self.assertIn('python', [kw[0] for kw in keywords])
    
    def test_score_calculation(self):
        resume_data = {'nama': 'Test', 'email': 'test@example.com', ...}
        job_desc = "Looking for Python developer..."
        params = {'level': 'mid-level', ...}
        
        result = calculate_ats_score(resume_data, job_desc, params)
        
        self.assertGreaterEqual(result['total_score'], 0)
        self.assertLessEqual(result['total_score'], 100)
```

### Integration Tests

```python
def test_end_to_end_workflow():
    # 1. Input data
    # 2. Analyze job desc
    # 3. Calculate score
    # 4. Generate recommendations
    # 5. Export PDF
    pass
```

---

## 📊 Analytics & Logging (Future)

### User Analytics

```python
import logging

logging.basicConfig(
    filename='app.log',
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def log_user_action(action, metadata):
    logging.info(f"Action: {action}, Metadata: {metadata}")
```

**What to track**:
- Resume creation count
- Average ATS scores
- Most common industries
- Popular templates
- Export format preferences

### Error Tracking

```python
try:
    result = calculate_ats_score(...)
except Exception as e:
    logging.error(f"Error in ATS calculation: {str(e)}")
    st.error("An error occurred. Please try again.")
```

---

## 🔮 Future Enhancements

### Phase 1: AI Integration
```python
# OpenAI GPT integration
import openai

def generate_ai_summary_gpt(resume_data, job_desc):
    prompt = f"""
    Generate professional resume summary for:
    Experience: {resume_data['pengalaman']}
    Skills: {resume_data['skills']}
    Job target: {job_desc[:200]}
    """
    
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    
    return response.choices[0].message.content
```

### Phase 2: LinkedIn Integration
```python
# LinkedIn API
import linkedin

def fetch_linkedin_data(profile_url):
    api = linkedin.LinkedIn(access_token=TOKEN)
    profile = api.get_profile(profile_url)
    
    return {
        'nama': profile['firstName'] + ' ' + profile['lastName'],
        'pengalaman': profile['positions'],
        'pendidikan': profile['educations'],
        'skills': profile['skills']
    }
```

### Phase 3: Advanced Analytics
```python
# A/B Testing
def track_conversion(resume_id, hired=False):
    # Track if resume led to job offer
    pass

# Benchmark data
def get_industry_benchmark(industri):
    # Return average ATS score for industry
    pass
```

---

## 📝 Code Style Guide

### Python Conventions
- Follow PEP 8
- Use type hints
- Docstrings for all functions

```python
def calculate_ats_score(
    resume_data: dict,
    job_desc: str,
    params: dict
) -> dict:
    """
    Calculate ATS score for resume vs job description.
    
    Args:
        resume_data: Dictionary containing resume fields
        job_desc: Job description text
        params: Configuration parameters (level, industry, etc.)
    
    Returns:
        Dictionary with score breakdown and recommendations
    """
    ...
```

### Naming Conventions
- Functions: `snake_case`
- Classes: `PascalCase`
- Constants: `UPPER_CASE`
- Private: `_leading_underscore`

---

## 🤝 Contributing (Future)

While currently non-open-source, future guidelines:

1. Fork repository
2. Create feature branch
3. Write tests
4. Submit PR with description
5. Code review process

---

**Questions?**

Email: aryhharyanto@proton.me  
Subject: [Technical Question] Your Query

---

Created by Ary HH  
© 2026 AI Resume Builder Optimizer
