# 🚀 Deployment Guide - AI Resume Builder Optimizer

Panduan lengkap untuk deploy aplikasi ke berbagai platform cloud.

---

## 📋 Daftar Isi

1. [Local Development](#local-development)
2. [Deploy ke Streamlit Cloud](#deploy-ke-streamlit-cloud)
3. [Deploy ke Heroku](#deploy-ke-heroku)
4. [Deploy ke Google Cloud Run](#deploy-ke-google-cloud-run)
5. [Deploy ke AWS EC2](#deploy-ke-aws-ec2)
6. [Deploy ke Railway](#deploy-ke-railway)

---

## 💻 Local Development

### Prerequisites
- Python 3.8 atau lebih tinggi
- pip package manager
- Git (optional)

### Setup Steps

1. **Clone atau Download Project**
   ```bash
   git clone <your-repo-url>
   cd ai-resume-builder-optimizer
   ```

2. **Create Virtual Environment (Recommended)**
   ```bash
   # Windows
   python -m venv venv
   venv\Scripts\activate

   # macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run Application**
   ```bash
   streamlit run app.py
   ```

5. **Open Browser**
   - App akan otomatis open di: `http://localhost:8501`
   - Atau manual buka browser dan navigate ke URL tersebut

### Development Tips

**Hot Reload**: Streamlit auto-reload saat file berubah

**Debug Mode**:
```bash
streamlit run app.py --logger.level=debug
```

**Custom Port**:
```bash
streamlit run app.py --server.port=8080
```

---

## ☁️ Deploy ke Streamlit Cloud

**Keuntungan**: Free, mudah, support langsung dari Streamlit

### Prerequisites
- GitHub account
- Streamlit Cloud account (gratis di [share.streamlit.io](https://share.streamlit.io))

### Steps

1. **Push ke GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin <your-github-repo-url>
   git push -u origin main
   ```

2. **Login ke Streamlit Cloud**
   - Go to [share.streamlit.io](https://share.streamlit.io)
   - Click "New app"

3. **Configure Deployment**
   - Repository: Select your GitHub repo
   - Branch: main
   - Main file path: `app.py`
   - App URL: Pilih custom subdomain (e.g., `ai-resume-builder.streamlit.app`)

4. **Advanced Settings (Optional)**
   ```toml
   # .streamlit/config.toml sudah include
   ```

5. **Deploy**
   - Click "Deploy!"
   - Wait 2-5 minutes untuk build

6. **Access App**
   - URL: `https://your-app-name.streamlit.app`

### Update App

Simply push changes ke GitHub:
```bash
git add .
git commit -m "Update features"
git push
```

Streamlit Cloud auto-redeploy!

---

## 🔶 Deploy ke Heroku

**Keuntungan**: Scalable, support custom domain

### Prerequisites
- Heroku account
- Heroku CLI installed

### Additional Files Needed

1. **Create `Procfile`**
   ```bash
   echo "web: sh setup.sh && streamlit run app.py" > Procfile
   ```

2. **Create `setup.sh`**
   ```bash
   cat > setup.sh << EOF
   mkdir -p ~/.streamlit/
   echo "\
   [server]\n\
   headless = true\n\
   port = \$PORT\n\
   enableCORS = false\n\
   \n\
   " > ~/.streamlit/config.toml
   EOF
   ```

3. **Create `runtime.txt`**
   ```bash
   echo "python-3.10.13" > runtime.txt
   ```

### Deployment Steps

1. **Login to Heroku**
   ```bash
   heroku login
   ```

2. **Create Heroku App**
   ```bash
   heroku create ai-resume-builder
   ```

3. **Deploy**
   ```bash
   git add .
   git commit -m "Deploy to Heroku"
   git push heroku main
   ```

4. **Open App**
   ```bash
   heroku open
   ```

### Scaling (Optional)

```bash
# Check current dynos
heroku ps

# Scale up
heroku ps:scale web=1

# Check logs
heroku logs --tail
```

---

## ☁️ Deploy ke Google Cloud Run

**Keuntungan**: Pay-as-you-go, auto-scaling, production-ready

### Prerequisites
- Google Cloud account
- gcloud CLI installed
- Docker installed

### Additional Files Needed

1. **Create `Dockerfile`**
   ```dockerfile
   FROM python:3.10-slim

   WORKDIR /app

   # Copy requirements
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt

   # Copy app files
   COPY . .

   # Expose port
   EXPOSE 8501

   # Health check
   HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

   # Run app
   ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
   ```

2. **Create `.dockerignore`**
   ```
   __pycache__
   *.pyc
   *.pyo
   *.pyd
   .Python
   env/
   venv/
   .git
   .gitignore
   README.md
   .DS_Store
   ```

### Deployment Steps

1. **Enable Cloud Run API**
   ```bash
   gcloud services enable run.googleapis.com
   ```

2. **Build Docker Image**
   ```bash
   gcloud builds submit --tag gcr.io/PROJECT_ID/ai-resume-builder
   ```

3. **Deploy to Cloud Run**
   ```bash
   gcloud run deploy ai-resume-builder \
     --image gcr.io/PROJECT_ID/ai-resume-builder \
     --platform managed \
     --region asia-southeast1 \
     --allow-unauthenticated \
     --memory 1Gi \
     --cpu 1
   ```

4. **Access App**
   - URL akan diberikan setelah deploy
   - Format: `https://ai-resume-builder-xxx-uc.a.run.app`

### Update

```bash
# Rebuild and redeploy
gcloud builds submit --tag gcr.io/PROJECT_ID/ai-resume-builder
gcloud run deploy ai-resume-builder --image gcr.io/PROJECT_ID/ai-resume-builder
```

---

## 🖥️ Deploy ke AWS EC2

**Keuntungan**: Full control, customizable

### Prerequisites
- AWS account
- EC2 instance (t2.micro untuk testing)
- SSH key pair

### Steps

1. **Launch EC2 Instance**
   - AMI: Ubuntu 22.04 LTS
   - Instance Type: t2.small (minimum)
   - Security Group: Allow HTTP (80), HTTPS (443), SSH (22)

2. **Connect via SSH**
   ```bash
   ssh -i your-key.pem ubuntu@ec2-xx-xx-xx-xx.compute.amazonaws.com
   ```

3. **Install Dependencies**
   ```bash
   # Update system
   sudo apt update && sudo apt upgrade -y

   # Install Python
   sudo apt install python3-pip python3-venv -y

   # Install Nginx (optional, for reverse proxy)
   sudo apt install nginx -y
   ```

4. **Setup Application**
   ```bash
   # Clone repo
   git clone <your-repo-url>
   cd ai-resume-builder-optimizer

   # Create virtual environment
   python3 -m venv venv
   source venv/bin/activate

   # Install requirements
   pip install -r requirements.txt
   ```

5. **Run with Screen (keeps running after logout)**
   ```bash
   # Install screen
   sudo apt install screen -y

   # Start screen session
   screen -S streamlit

   # Run app
   streamlit run app.py --server.port=8501 --server.address=0.0.0.0

   # Detach: Ctrl+A, then D
   # Reattach: screen -r streamlit
   ```

6. **Setup Nginx Reverse Proxy (Optional)**
   ```bash
   sudo nano /etc/nginx/sites-available/streamlit
   ```

   Add:
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:8501;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection "upgrade";
           proxy_set_header Host $host;
       }
   }
   ```

   Enable:
   ```bash
   sudo ln -s /etc/nginx/sites-available/streamlit /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl restart nginx
   ```

7. **Setup Systemd Service (Run on boot)**
   ```bash
   sudo nano /etc/systemd/system/streamlit.service
   ```

   Add:
   ```ini
   [Unit]
   Description=Streamlit App
   After=network.target

   [Service]
   User=ubuntu
   WorkingDirectory=/home/ubuntu/ai-resume-builder-optimizer
   ExecStart=/home/ubuntu/ai-resume-builder-optimizer/venv/bin/streamlit run app.py --server.port=8501
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

   Enable:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable streamlit
   sudo systemctl start streamlit
   sudo systemctl status streamlit
   ```

---

## 🚂 Deploy ke Railway

**Keuntungan**: Super mudah, free tier, auto CI/CD

### Prerequisites
- Railway account ([railway.app](https://railway.app))
- GitHub repo

### Steps

1. **Login to Railway**
   - Go to [railway.app](https://railway.app)
   - Sign in with GitHub

2. **Create New Project**
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Choose your repository

3. **Railway Auto-Detects**
   - Railway auto-detect Python app
   - Uses `requirements.txt` for dependencies

4. **Add Start Command**
   - In Railway dashboard → Settings
   - Start Command: `streamlit run app.py --server.port=$PORT --server.address=0.0.0.0`

5. **Environment Variables (if needed)**
   - Go to Variables tab
   - Add any needed env vars

6. **Deploy**
   - Railway auto-deploys on git push
   - Get public URL from dashboard

### Update

Just push to GitHub:
```bash
git add .
git commit -m "Update"
git push
```

Railway auto-redeploys!

---

## 🔒 Environment Variables (For Production)

Create `.env` file untuk sensitive data:

```bash
# .env (DO NOT COMMIT TO GIT!)
STREAMLIT_SERVER_PORT=8501
STREAMLIT_SERVER_ADDRESS=0.0.0.0
STREAMLIT_THEME_PRIMARY_COLOR=#1E3A8A
STREAMLIT_BROWSER_GATHER_USAGE_STATS=false

# Future API keys (when integrating)
OPENAI_API_KEY=your_key_here
LINKEDIN_API_KEY=your_key_here
GOOGLE_DRIVE_API_KEY=your_key_here
```

Load in app:
```python
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv('OPENAI_API_KEY')
```

---

## 📊 Monitoring & Maintenance

### Streamlit Cloud
- Built-in analytics di dashboard
- Check logs: App → Manage → Logs

### Heroku
```bash
heroku logs --tail
heroku ps
```

### Google Cloud Run
```bash
gcloud logging read "resource.type=cloud_run_revision"
```

### AWS EC2
```bash
# Check service status
sudo systemctl status streamlit

# Check logs
journalctl -u streamlit -f
```

---

## 🐛 Troubleshooting

### Issue: App tidak start

**Solution**:
```bash
# Check Python version
python --version  # Should be 3.8+

# Reinstall dependencies
pip install -r requirements.txt --force-reinstall
```

### Issue: Port already in use

**Solution**:
```bash
# Kill process on port 8501
# Windows
netstat -ano | findstr :8501
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:8501 | xargs kill -9
```

### Issue: Memory error on small instances

**Solution**:
- Upgrade to larger instance
- Or add swap space (Linux):
  ```bash
  sudo fallocate -l 2G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  ```

---

## 💰 Cost Estimation

| Platform | Free Tier | Paid Start | Best For |
|----------|-----------|------------|----------|
| Streamlit Cloud | ✅ 1 app | N/A | Development, demos |
| Railway | ✅ $5 credit | $5/month | Small projects |
| Heroku | ✅ 550 hours | $7/month | Production lite |
| Google Cloud Run | ✅ 2M requests/month | Pay-as-you-go | Production, scale |
| AWS EC2 | ✅ 750 hours t2.micro (1 year) | $3.5/month | Full control |

---

## 🎯 Recommended Setup

**For Development/Testing**: Streamlit Cloud (Free)  
**For Small Business**: Railway ($5/month)  
**For Production**: Google Cloud Run (Scalable)  
**For Full Control**: AWS EC2 + Nginx

---

**Need Help?**

Email: aryhharyanto@proton.me  
Subject: [Deployment Help] Your Issue

---

**Happy Deploying! 🚀**

Created by Ary HH  
© 2026 AI Resume Builder Optimizer
