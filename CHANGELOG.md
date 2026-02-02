# Changelog

All notable changes to AI Resume Builder Optimizer will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] - 2026-02-02

### 🎉 Initial Release

First public release of AI Resume Builder Optimizer - Micro SaaS untuk membuat resume ATS-friendly.

### ✨ Features Added

#### Core Functionality
- **Resume Input System**
  - Comprehensive form untuk data personal, pendidikan, pengalaman, skills
  - Multi-parameter support: level pengalaman, industri, bahasa, tone, panjang
  - Real-time data validation
  - Session state management untuk data persistence

- **Job Description Analysis**
  - TF-IDF-based keyword extraction (top 20 keywords)
  - Support paste text dan upload .txt file
  - Automatic relevance scoring untuk setiap keyword
  - Visual keyword display dengan metrics

- **ATS Score Calculation**
  - Comprehensive scoring algorithm:
    - Keyword Match (50%) - Cosine similarity
    - Section Completeness (30%)
    - Tone Fit (10%)
    - Length Compliance (10%)
  - Confidence interval calculation (±5-10% based on level)
  - ATS compliance check
  - Missing keywords identification

- **AI Content Generation**
  - Template-based professional summary generator
  - Level-specific templates (entry/mid/senior)
  - Tone customization (professional/creative/technical)
  - Industry-aware content generation

- **Recommendations Engine**
  - Priority-based recommendations (HIGH/MEDIUM/LOW)
  - Actionable steps dengan expected impact
  - Automatic analysis-based suggestions
  - Score improvement estimations

- **Visualization & Analytics**
  - Score breakdown bar chart (matplotlib)
  - Metric cards untuk key indicators
  - What-If Simulator untuk parameter changes
  - Real-time delta score calculation
  - Benchmark comparison (vs 80% industry average)

- **Export Functionality**
  - PDF export dengan FPDF (ATS-friendly format)
  - Word export dengan python-docx (editable format)
  - CSV export untuk analytics data
  - Template selection (10+ designs)
  - Automatic download link generation

#### User Interface
- **Modern Web Design**
  - Responsive layout (mobile-friendly)
  - Clean, professional aesthetic
  - Custom CSS dengan gradient colors
  - Tab-based navigation (5 tabs)
  - Sidebar parameter controls

- **Interactive Features**
  - Real-time form validation
  - Progress indicators
  - Success/warning/error notifications
  - Expandable sections
  - Metric visualization

#### Internationalization
- Bahasa Indonesia (default)
- English support
- Language switcher di sidebar

#### Documentation
- Comprehensive README.md dengan setup guide
- PANDUAN_PENGGUNA.md (Indonesian user guide)
- DEPLOYMENT_GUIDE.md (multi-platform deployment)
- TECHNICAL_DOCS.md (developer documentation)
- API_DOCS.md (future API specification)
- SAMPLE_DATA.md (testing scenarios)

### 🔧 Technical Implementation

#### Dependencies
- streamlit==1.31.0 (web framework)
- pandas==2.1.4 (data processing)
- numpy==1.26.3 (numerical operations)
- scikit-learn==1.4.0 (ML algorithms)
- matplotlib==3.8.2 (visualization)
- fpdf==1.7.2 (PDF generation)
- python-docx==1.1.0 (Word generation)
- Pillow==10.2.0 (image processing)
- openpyxl==3.1.2 (Excel support)

#### Algorithms
- TF-IDF vectorization untuk keyword extraction
- Cosine similarity untuk resume-job matching
- Template-based content generation
- Rule-based recommendation engine

#### Architecture
- Single-file Streamlit application
- Session-based state management
- Caching untuk performance optimization
- Modular function design

### 📊 Formula & Calculations

#### ATS Score Formula
```
Total Score = (Keyword × 50%) + (Section × 30%) + (Tone × 10%) + (Length × 10%)
Max: 100%
```

#### Confidence Intervals
- Entry-level: ±10%
- Mid-level: ±8%
- Senior-level: ±5%

#### Keyword Extraction
- TF-IDF with ngram_range=(1,2)
- Stop words removal (English)
- Top 20 features selection

### 🎯 Supported Features

#### Parameters
- **Level Pengalaman**: entry-level, mid-level, senior-level
- **Industri**: IT/Technology, Marketing, Finance, Healthcare, Engineering, Design, Education, Consulting, Sales, Other
- **Bahasa**: Indonesia, English
- **Panjang Resume**: 1 halaman, 1-2 halaman, 2 halaman
- **Tone**: professional, creative, technical
- **Template**: Modern, Classic, Creative, Minimalist, Professional, Bold, Elegant, Tech, Corporate, Startup

#### Export Formats
- PDF (ATS-friendly, read-only)
- Word (.docx, editable)
- CSV (analytics data)

### 🚀 Performance

- Initial load time: <3 seconds
- ATS calculation: <1 second
- PDF/Word generation: <2 seconds
- Keyword extraction: <0.5 seconds

### 🔒 Security & Privacy

- No external data upload
- Session-based data storage
- No third-party tracking
- Local computation only
- HTTPS-ready for deployment

### 📝 License

Proprietary License - Non-Open Source
Copyright © 2026 Ary HH

---

## [Unreleased]

### Planned for v1.1.0

#### Features to Add
- [ ] Advanced AI integration (OpenAI GPT-4, Claude API)
- [ ] LinkedIn API integration untuk auto-pull data
- [ ] Google Drive integration untuk save/sync
- [ ] Email API untuk auto-send resume
- [ ] User authentication system
- [ ] Database persistence (PostgreSQL/MongoDB)
- [ ] More template designs (20+ total)
- [ ] Multi-language support (Mandarin, Japanese)

#### Improvements
- [ ] Enhanced keyword extraction dengan transformers
- [ ] Better visualization dengan Plotly/Chart.js
- [ ] Advanced analytics dashboard
- [ ] A/B testing untuk recommendations
- [ ] Industry-specific benchmarking data
- [ ] Real-time collaboration features

#### Bug Fixes
- None reported yet

### Planned for v2.0.0 (Major Update)

#### B2B Features
- [ ] Team collaboration tools
- [ ] Custom branding options
- [ ] White-label solution
- [ ] Admin dashboard
- [ ] Usage analytics
- [ ] API access untuk enterprise
- [ ] SSO integration
- [ ] Bulk resume processing

#### Advanced Analytics
- [ ] Success rate tracking
- [ ] Interview conversion metrics
- [ ] Industry trend analysis
- [ ] Competitive benchmarking
- [ ] Predictive scoring

#### Premium Tiers
- [ ] Free tier: 5 resumes/month
- [ ] Basic: $9.99/month - 50 resumes
- [ ] Pro: $29.99/month - Unlimited + AI
- [ ] Enterprise: Custom pricing - White-label + API

---

## Version History Summary

| Version | Release Date | Status | Key Features |
|---------|--------------|--------|--------------|
| 1.0.0 | 2026-02-02 | ✅ Released | Initial release, core ATS features |
| 1.1.0 | TBD | 📋 Planned | AI integration, LinkedIn/Drive |
| 2.0.0 | TBD | 💭 Roadmap | B2B features, Premium tiers |

---

## Migration Guide

### From Preview to v1.0.0

No migration needed - first public release.

### Future Migrations

Migration guides will be provided for breaking changes between major versions.

---

## Support & Feedback

### Reporting Issues

Please report issues via email to: **aryhharyanto@proton.me**

Include:
- Version number
- Browser/OS information
- Steps to reproduce
- Expected vs actual behavior
- Screenshots (if applicable)

### Feature Requests

Send feature requests to: **aryhharyanto@proton.me**  
Subject: [Feature Request] Your Idea

### Questions

For questions, contact: **aryhharyanto@proton.me**  
Subject: [Question] Your Question

---

## Credits

**Created by**: Ary HH (aryhharyanto@proton.me)  
**License**: Proprietary - Non-Open Source  
**Copyright**: © 2026 Ary HH. All Rights Reserved.

### Acknowledgments

- Streamlit team untuk amazing web framework
- Scikit-learn contributors untuk ML tools
- Python community untuk excellent libraries
- Job seekers yang memberikan feedback selama development

---

## Notes

- Changelog follows [Keep a Changelog](https://keepachangelog.com/) format
- Version numbering follows [Semantic Versioning](https://semver.org/)
- All dates in YYYY-MM-DD format (ISO 8601)

**[Unreleased]**: Features in development, not yet released  
**[Version]**: Released version with date

---

**Last Updated**: February 2, 2026  
**Maintained by**: Ary HH

*Keep building amazing resumes!* 🚀
