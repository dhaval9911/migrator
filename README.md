# Quick fix on your VM (no re-download needed)
```
sed -i "/import CodePanel/d" ~/splunk-secops-migrator/frontend/src/pages/MigratePage.jsx
```
# Then retry the build
```
cd ~/splunk-secops-migrator/frontend
npm run build
```



-------------------------------------------------------------------------------------

Step 1 — Download & Extract
Download the zip from the chat, then on your VM:
bash
```
cd ~
unzip splunk-secops-migrator-local-ai.zip
cd splunk-secops-migrator
```
Step 2 — Install Ollama (local AI engine)
bash
```
curl -fsSL https://ollama.com/install.sh | sh
```
Verify it installed:

bash
```
ollama --version
```

Step 3 — Start Ollama & Pull a Model
Start the Ollama server in a separate terminal (keep it running):
bash
```
ollama serve
```
In your original terminal, pull a model based on your RAM:
Your RAMCommandDownload Size4 GBollama pull phi32.3 GB6 GBollama pull llama3.22.0 GB8 GB+ollama pull mistral4.1 GB
bash

# Example for 8GB+ machine:
```
ollama pull mistral
```
Verify it downloaded:
```
ollama list
```
Step 4 — Configure the Backend
bash
```
cd ~/splunk-secops-migrator/backend
cp .env.example .env
nano .env
OLLAMA_MODEL=mistral    # or phi3, llama3.2, etc.
```
Save with Ctrl+O, Enter, Ctrl+X.

Step 5 — Set Up Python Backend

```
cd ~/splunk-secops-migrator/backend
```
# Create virtual environment
```
python3 -m venv venv
source venv/bin/activate
```

# Install dependencies
```pip install -r requirements.txt```

# Initialize the database
```python3 -c "from app.core.database import init_db; init_db()"```

Step 6 — Set Up & Build Frontend
First check if Node.js is installed:

```
node --version
```
If not installed:

```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash
 -**
sudo apt-get install -y nodejs
```
Then build the frontend:
```
cd ~/splunk-secops-migrator/frontend
npm install
npm run build```
```
Step 7 — Run the App
```
cd ~/splunk-secops-migrator/backend
source venv/bin/activate
python3 run.py
```


You should see:

🚀 Splunk → SecOps Migrator API starting...
📖 Swagger docs: http://localhost:8000/docs
Open your browser: http://localhost:8000

Every Time After First Setup
You only need two terminals:
Terminal 1 — Ollama:
bash
ollama serve
Terminal 2 — App:
bash
cd ~/splunk-secops-migrator/backend
source venv/bin/activate
python3 run.py
Then open http://localhost:8000

