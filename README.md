# 📄 AI Resume Builder Optimizer

> **Micro SaaS AI-Powered Resume Builder untuk Indonesia**  
> Membantu job seekers, freelancers, dan fresh graduates membuat resume ATS-friendly yang tailored untuk job application mereka.

---

## 🎯 Vision & Tujuan

**AI Resume Builder Optimizer** adalah tool web application yang dirancang untuk:

- ✅ **Analisis Job Description** - Ekstraksi keyword ATS-friendly dari job posting
- ✅ **Generate Resume Content** - AI-powered content generation untuk summary, experience, dan skills
- ✅ **Template Selection** - Pilih dari 10+ template design profesional
- ✅ **Export PDF/Word** - Download resume dalam format siap pakai
- ✅ **Analytics & Scoring** - Skor resume vs job match dengan confidence interval
- ✅ **Actionable Recommendations** - Saran konkret untuk meningkatkan peluang diterima

**Target Users:**
- 👨‍💼 Job seekers yang ingin optimasi resume
- 💼 Freelancers yang butuh resume tailored per project
- 🎓 Fresh graduates yang memulai karir
- 🚀 Professionals yang ingin career pivot

---

## 🚀 Fitur Utama

### 1. **Input Data Resume**
- Form lengkap untuk data personal, pendidikan, pengalaman, dan skills
- Support multi-parameter: level pengalaman, industri, bahasa, tone
- Generate AI-powered professional summary

### 2. **Analisis Job Description**
- Paste atau upload job description (.txt)
- Ekstraksi top 20 keyword menggunakan TF-IDF
- Identifikasi missing keywords untuk optimasi

### 3. **Resume Generation**
- AI-generated summary berdasarkan level & industri
- Template selection (Modern, Classic, Creative, dll)
- Customisasi tone (Professional, Creative, Technical)

### 4. **Analytics & ATS Score**
- **Overall Score** - Skor total dengan confidence interval (±5-10%)
- **Score Breakdown**:
  - Keyword Match (50%) - TF-IDF cosine similarity
  - Section Completeness (30%)
  - Tone Fit (10%)
  - Length Compliance (10%)
- **Visualisasi** - Bar chart breakdown, metrics cards
- **What-If Simulator** - Simulasi perubahan parameter real-time
- **Benchmark Comparison** - Compare vs average ATS score (80% dari Jobscan 2026)

### 5. **Recommendations**
- Prioritized recommendations (HIGH/MEDIUM/LOW)
- Actionable steps dengan expected impact
- Export options: PDF, Word, CSV analytics

---

## 📊 Formula & Metodologi

### ATS Score Calculation

```
Total Score = (Keyword Score × 50%) + (Section Score × 30%) + (Tone Score × 10%) + (Length Score × 10%)
```

**Detail Formula:**

1. **Keyword Match (50%):**
   - TF-IDF vectorization pada resume vs job description
   - Cosine similarity > 0.5 = good match
   - Missing keywords identification

2. **Section Completeness (30%):**
   - Required sections: Name, Email, Education, Experience, Skills
   - Score = (Completed Sections / Total Required) × 100%

3. **Tone Fit (10%):**
   - Mapping tone ke industri (e.g., Creative → Marketing = 100%)
   - Default fit = 80%

4. **Length Compliance (10%):**
   - 1-2 halaman (300-600 kata) = Full score
   - Estimasi: 300 kata per halaman

**Confidence Interval:**
- Entry-level: ±10%
- Mid-level: ±8%
- Senior-level: ±5%

---

## 🛠️ Tech Stack

- **Frontend/Backend:** Streamlit (Python web framework)
- **Data Processing:** Pandas, NumPy
- **NLP/ML:** Scikit-learn (TF-IDF, Cosine Similarity)
- **Visualization:** Matplotlib
- **Export:** FPDF (PDF), python-docx (Word), openpyxl (Excel)

---

## 📦 Installation & Setup

### Prerequisites
- Python 3.8+ 
- pip (Python package manager)

### Quick Start

1. **Clone atau Download Repository**
   ```bash
   # Jika dari git
   git clone <repository-url>
   cd ai-resume-builder-optimizer
   
   # Atau extract dari ZIP
   unzip ai-resume-builder-optimizer.zip
   cd ai-resume-builder-optimizer
   ```

2. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run Application**
   ```bash
   streamlit run app.py
   ```

4. **Open Browser**
   - App akan otomatis membuka di browser
   - Atau akses manual: `http://localhost:8501`

---

## 💻 Cara Penggunaan

### Step-by-Step Guide

1. **Setup Parameter (Sidebar)**
   - Pilih bahasa: Indonesia/English
   - Isi nama lengkap
   - Pilih level pengalaman, industri, panjang resume, tone
   - Pilih template design

2. **Tab 1: Input Data**
   - Isi data personal (nama, email, telepon)
   - Tambahkan pendidikan, pengalaman kerja, skills
   - Opsional: Tulis professional summary (atau biarkan AI generate)
   - Klik "Simpan Data Resume"

3. **Tab 2: Analisis Job Desc**
   - Paste atau upload job description
   - Klik "Analisis Keyword"
   - Review top 20 ATS keywords

4. **Tab 3: Generate Resume**
   - Klik "Generate Professional Summary" untuk AI summary
   - Preview resume lengkap

5. **Tab 4: Analytics & Score**
   - Klik "Hitung ATS Score"
   - Review overall score, breakdown, dan missing keywords
   - Gunakan What-If Simulator untuk optimasi

6. **Tab 5: Recommendations**
   - Review prioritized recommendations
   - Download PDF/Word resume
   - Export analytics CSV

---

## 📈 Contoh Output

### Sample ATS Score Report

```
Overall ATS Score: 85.3% (±8% confidence)
Status: ✅ PASS

Score Breakdown:
- Keyword Match: 42.5/50
- Section Complete: 27.0/30
- Tone Fit: 9.0/10
- Length Compliance: 10.0/10

Missing Keywords: python, agile, scrum, docker, kubernetes

Recommendations:
[HIGH] Tambahkan keyword: python, agile, scrum → +15% score
[MEDIUM] Integrasikan 5 missing keywords ke experience → +10% score
```

---

## 🔮 Future Roadmap

### Phase 1 (Current) ✅
- Core resume builder functionality
- ATS scoring & analytics
- Basic export (PDF/Word/CSV)

### Phase 2 (Planned)
- 🔗 **LinkedIn API Integration** - Auto-pull profile data
- 💾 **Google Drive Integration** - Save & sync resume
- 🤖 **Advanced AI** - Hugging Face transformers for better keyword extraction
- 📧 **Email API** - Auto-send resume ke HR

### Phase 3 (Future)
- 🏢 **B2B Features** - Custom branding, team collaboration
- 📊 **Advanced Analytics** - Industry benchmarking, success rate tracking
- 🌐 **Multi-language** - Full English, Mandarin support
- 💰 **Premium Tiers** - Advanced features, unlimited exports

---

## 🔒 Privacy & Security

- ✅ **No External Data Upload** - Semua perhitungan di-client side
- ✅ **Session-Based** - Data hanya tersimpan dalam browser session
- ✅ **No Third-Party Tracking** - Tidak ada analytics atau tracking eksternal
- ⚠️ **Disclaimer** - Tool ini memberikan estimasi berdasarkan best practices ATS. Hasil adalah panduan kasar, bukan pengganti review profesional.

---

## 📄 License & Copyright

**Non-Open Source Project**

© 2026 Ary HH (aryhharyanto@proton.me)  
All rights reserved.

This project is **NOT open source** and is **NOT available for public collaboration or contribution**. 

**Usage Restrictions:**
- ❌ Tidak boleh di-redistribute
- ❌ Tidak boleh di-modify untuk commercial use tanpa izin
- ❌ Tidak boleh di-fork untuk public repository
- ✅ Boleh digunakan untuk personal use

Untuk licensing atau partnership inquiries, hubungi: **aryhharyanto@proton.me**

---

## 🤝 Contact & Support

**Creator:** Ary HH  
**Email:** aryhharyanto@proton.me  
**Project:** AI Resume Builder Optimizer  

**Untuk:**
- 💼 Business inquiries
- 🐛 Bug reports
- 💡 Feature requests
- 📧 General questions

Silakan email ke: aryhharyanto@proton.me

---

## 🎓 Credits & References

**Metodologi berdasarkan:**
- ATS Best Practices dari Jobscan (2023-2026)
- LinkedIn Resume Guidelines
- Industry research on resume optimization

**Tech Stack:**
- Streamlit - Web framework
- Scikit-learn - Machine learning
- FPDF & python-docx - Document generation

---

## ⚠️ Disclaimer

Tool ini memberikan **estimasi dan panduan** berdasarkan ATS best practices yang tersedia secara publik. 

**TIDAK MENGGANTIKAN:**
- ❌ Review manual oleh HR professional
- ❌ Konsultasi karir profesional
- ❌ Proofreading oleh native speaker (untuk resume Bahasa Inggris)

**Best Practice:**
1. Gunakan tool ini sebagai starting point
2. Review dan customize manual sesuai job spesifik
3. Minta feedback dari mentor atau HR professional
4. Lakukan proofreading menyeluruh sebelum submit

---

**Made with ❤️ for Indonesian job seekers**

*Membantu Anda mendapatkan pekerjaan impian dengan resume yang optimal!* 🚀
