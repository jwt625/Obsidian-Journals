
2025-05-31T22:02:14-07:00
Previous: [[20240623 Setup the blog]].

# Proposal for adding a chat
Cursor's proposal:


Based on your current setup and requirements, I'll propose a solution that minimizes costs while providing a robust RAG-based chatbot. Here's the comprehensive proposal:

### 1. Architecture Overview

```
Frontend (Jekyll + Custom JS) <-> Backend (FastAPI) <-> Vector DB (Chroma) <-> LLM (Ollama)
```

### 2. Backend Components
1. FastAPI Server:
- Host on a small VPS (e.g., DigitalOcean droplet, ~$5/month)
- Implement endpoints for:
- Chat completion
- Document ingestion
- Search functionality
1. Vector Database:
- Use Chroma DB (open-source, can run locally)
- Store embeddings of all blog posts
- Implement similarity search
1. LLM Integration:
- Use Ollama (open-source) with models like:
- Mistral 7B
- Llama2 7B
- Run locally on the VPS
- Implement RAG pipeline
1. Document Processing:
- Create a script to:
- Parse Jekyll markdown files
- Extract text content
- Generate embeddings
- Store in Chroma DB
### 3. Frontend Components
1. Chat Interface:
- Add a floating chat widget to your Jekyll theme
- Implement using vanilla JavaScript or minimal framework
- Features:
- Real-time chat
- Markdown rendering
- Code highlighting
- Loading states
1. Search Integration:
- Enhance existing search with semantic capabilities
- Add filters for:
- Date ranges
- Categories
- Tags
### 4. Implementation Steps
1. Backend Setup:
```python
# main.py
from fastapi import FastAPI
from chromadb import Client
import ollama
from typing import List

app = FastAPI()
chroma_client = Client()

@app.post("/chat")
async def chat(message: str, history: List[dict]):
    # RAG implementation
    relevant_docs = search_documents(message)
    context = format_context(relevant_docs)
    response = ollama.chat(model="mistral", 
                          messages=[{"role": "user", 
                                   "content": f"Context: {context}\n\nQuestion: {message}"}])
    return response
```

2. Frontend Integration:

```javascript
// chat-widget.js
class ChatWidget {
    constructor() {
        this.container = document.createElement('div');
        this.container.className = 'chat-widget';
        this.init();
    }

    async sendMessage(message) {
        const response = await fetch('/api/chat', {
            method: 'POST',
            body: JSON.stringify({ message, history: this.history })
        });
        return response.json();
    }
}
```

3. Jekyll Integration:
```html
<!-- _includes/chat-widget.html -->
<div id="chat-widget-container"></div>
<script src="/assets/js/chat-widget.js"></script>
```
 
### 5. Observability Implementation
1. Logging:
- Use Python's built-in logging
- Implement structured logging with JSON format
- Log:
- User queries
- Response times
- Error rates
- Token usage
1. Monitoring:
- Prometheus + Grafana (open-source)
- Monitor:
- API response times
- Memory usage
- CPU utilization
- Vector DB performance
1. Error Tracking:
- Sentry (free tier available)
- Track:
- Frontend errors
- Backend exceptions
- API failures
### 6. Cost Optimization
1. Hosting:
- Use a small VPS (~$5/month)
- Implement caching for frequent queries
- Use CDN for static assets (Cloudflare free tier)
1. LLM:
- Run Ollama locally
- Implement response caching
- Use smaller models when possible
1. Vector DB:
- Run Chroma DB locally
- Implement periodic cleanup of old vectors
- Use efficient indexing
### 7. Security Considerations
1. API Security:
- Implement rate limiting
- Add API key authentication
- Use CORS policies
1. Data Protection:
- Sanitize user inputs
- Implement content filtering
- Regular security updates
### Next Steps
1. Set up the VPS and install dependencies
2. Implement the backend API
3. Create the document processing pipeline
4. Develop the frontend chat widget
5. Integrate with your Jekyll site
6. Set up monitoring and logging
7. Test and optimize


## More clear view of the architecture
2025-06-01T11:33:38-07:00

```
Frontend (Jekyll/GH Pages) <-> Backend (Cloud Server) <-> DeepSeek API
                                    ↕
                              Database (PostgreSQL)
                                    ↕
                              Vector DB (Chroma)
```

### 1. Backend Hosting Options
Oracle Cloud Free Tier (Best Overall Free Option)
- Always Free resources:
- 2 AMD-based Compute VMs (1 GB RAM each)
- 4 ARM-based Compute instances (24 GB RAM total)
- 200 GB total storage
- 10 Mbps bandwidth
- Free outbound data transfer: 10 TB/month
- Perfect for running:
- FastAPI backend
- PostgreSQL database
- Vector database
- Limitations:
- Requires credit card for verification
- Setup is slightly more complex than other providers
Render Free Tier
- Free web services:
- 750 hours/month
- 512 MB RAM
- Shared CPU
- 1 concurrent instance
- Limitations:
- Spins down after 15 minutes of inactivity
- Cold starts can be slow
- 100 GB/month bandwidth
- No persistnt storage

Fly.io Free Tier
- Includes:
- 3 shared-cpu-1x 256MB VMs
- 3GB persistent volume storage
- 160GB outbound data transfer
- Limitations:
- Requires credit card
- Limited RAM might be tight for LLM operations
### 2. Authentication & Database Services
Supabase Free Tier (Recommended)
- Authentication features:
- Unlimited users
- Social providers (Google, GitHub, etc.)
- Email/password authentication
- JWT tokens
- Row Level Security
- Database:
- PostgreSQL database (500MB)
- 50,000 monthly active users
- 500K edge function invocations
- Real-time listeners
- Limitations:
- Database pauses after 1 week of inactivity
- 2GB bandwidth/month
- 50MB file storage
Firebase Free Tier
- Authentication:
- 10K monthly active users
- Multiple sign-in methods
- Email verification
- Realtime Database:
- 1GB storage
- 10GB/month transfer
- Limitations:
- Complex pricing after free tier
- Lock-in to Google ecosystem
Auth0 Free Tier
- Features:
- 7,000 active users
- Up to 2 social identity providers
- Unlimited logins
- Limitations:
- No passwordless authentication in free tier
- Limited customization
- Community support only


### 3. Recommended Setup
Based on the free tier offerings, here's the most cost-effective yet robust setup:

1. Backend + Database: Oracle Cloud Free Tier
```bash
sudo apt update && sudo apt upgrade
sudo apt install python3-pip postgresql nginx
python3 -m venv venv
source venv/bin/activate
pip install fastapi uvicorn psycopg2-binary chromadb
sudo -u postgres createdb chatbot
sudo -u postgres createuser chatbot_user
```

2. Authentication: Supabase
```javascript
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('YOUR_SUPABASE_URL', 'YOUR_SUPABASE_ANON_KEY')
async function signIn(email, password) {
  const { user, error } = await supabase.auth.signIn({ email, password })
  return { user, error }
}
```

3. Backend Implementation with Supabase
```python
from fastapi import FastAPI, Depends, HTTPException
from supabase import create_client, Client
from typing import Optional
app = FastAPI()
supabase: Client = create_client(supabase_url="YOUR_SUPABASE_URL", supabase_key="YOUR_SUPABASE_SERVICE_KEY")
async def get_current_user(token: str):
    try:
        user = supabase.auth.api.get_user(token)
        return user
    except Exception as e:
        raise HTTPException(status_code=401, detail="Invalid token")
@app.post("/chat")
async def chat(message: str, user = Depends(get_current_user)):
    # Your chat implementation
    pass
```
4. Database Schema in Supabase
```sql
CREATE TABLE conversations (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    user_id UUID REFERENCES auth.users(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW()),
    title TEXT
);
CREATE TABLE messages (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    conversation_id UUID REFERENCES conversations(id),
    content TEXT,
    role TEXT CHECK (role IN ('user', 'assistant')),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW())
);
ALTER TABLE conversations ENABLE ROW LEVEL SECURITY;
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can only access their own conversations"
ON conversations FOR ALL
USING (auth.uid() = user_id);
CREATE POLICY "Users can only access messages from their conversations"
ON messages FOR ALL
USING (
    conversation_id IN (
        SELECT id FROM conversations WHERE user_id = auth.uid()
    )
);
```

### 4. Deployment Steps

1. Setup Oracle Cloud VM:
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22/tcp
sudo ufw enable
sudo nano /etc/nginx/sites-available/chatbot
```

2. Configure Nginx:
```nginx
server {
    server_name your-domain.com;
    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }    
}
```

3. Setup SSL with Let's Encrypt:
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

4. Run the Application:
```bash
sudo nano /etc/systemd/system/chatbot.service
[Unit]
Description=Chatbot FastAPI application
After=network.target
[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/chatbot
Environment="PATH=/home/ubuntu/chatbot/venv/bin"
ExecStart=/home/ubuntu/chatbot/venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000
[Install]
WantedBy=multi-user.target
sudo systemctl start chatbot
sudo systemctl enable chatbot
```

Would you like me to:
5. Provide more detailed setup instructions for any of these components?
6. Show how to implement specific features like rate limiting or caching?
7. Explain how to set up monitoring and logging on the free tier?
8. Demonstrate how to implement the vector database on Oracle Cloud?
# Setup various cloud
2025-06-01T11:50:45-07:00

## Oracle
Got an oracle account. This feels evil.
There is a cloud account as well as a user name. Kind of confusing.
![[Pasted image 20250601115259.png]]



Got a ssh key.

2025-06-01T12:19:26-07:00
Instance seems to be running:
```
Tools Available in Cloud Shell

CloudShell provides a collection of pre-installed and pre-authenticated tools readily accessible to users which are updated on a regular basis including:

    *  OCI Command Line Interface (CLI)
    *  Source Code Management: Git
    *  SQL: SQLcl, mysql-community-client
    *  Kubernetes utilities: kubectl, helm
    *  Build tools: maven, make
    *  Provisioning and configuration management: terraform, ansible
    *  Editors: vim, nano, emacs
    *  Linux Shells: bash, sh, tmux
    *  Linux utilities: iputils, jq, wget, zip/unzip, tar (and many more)
    *  Python tools: pip, iPython
    *  Typescript/Javascript support: node.js, NPM, nvm
    *  Docker: Docker engine
```

`oci iam region list`

```
We support the following programming languages in Cloud Shell so you can write your own applications:

    *  Java including multiple versions of Oracle Java as well as GraalVM Enterprise JDK 17
    *  Python (2 and 3)
    *  Ruby
    *  Golang
    *  JavaScript/NodeJS
    *  C/C++ using gcc
    *  sh and bash scripts
```


SSH:
```
You can connect to your Oracle Cloud instance using SSH from the Oracle Cloud Shell in the browser by following these steps:

1. Open the Oracle Cloud Console.
2. Navigate to the Compute section.
3. Find your instance and click on it.
4. Click on the "SSH" button.
5. A pop-up will appear with the SSH command.

Alternatively, you can use the Oracle Cloud Shell command:
```bash
ssh -i <private_key> <username>@<public_ip>
```
Replace `<private_key>` with the path to your private key file, `<username>` with your instance's username (usually `ubuntu` or `opc`), and `<public_ip>` with your instance's public IP address.

If you have not generated a private key, you can do so using:
```bash
ssh-keygen
```
Then, upload the public key to your Oracle Cloud instance:
```bash
oci compute instance attach-vnic --instance-id <instance_id> --shape <shape> --public-key-file <public_key_file>
```

see if it drains my credit card:
![[Pasted image 20250601122516.png]]
https://docs.oracle.com/en-us/iaas/compute-cloud-at-customer/topics/compute/connecting-to-a-compute-instance.htm

https://docs.oracle.com/en-us/iaas/compute-cloud-at-customer/topics/compute/tutorial-launching-your-first-instance.htm


Ok the instance is in the compute, and the getway etc are in networking:
- https://cloud.oracle.com/compute
- https://cloud.oracle.com/networking

https://docs.oracle.com/en-us/iaas/compute-cloud-at-customer/topics/compute/5-launch-an-instance.htm

https://docs.oracle.com/en-us/iaas/compute-cloud-at-customer/topics/compute/7-connect-to-your-instance.htm#_7-connect-to-your-instance

2025-06-01T12:39:04-07:00
![[Pasted image 20250601123905.png]]

2025-06-01T12:51:25-07:00
logged in:
` ssh -i ~/.ssh/oracle_cloud_key ubuntu@146.235.193.141`


```
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 6.8.0-1025-oracle x86_64)
* Documentation: https://help.ubuntu.com
* Management: https://landscape.canonical.com
* Support: https://ubuntu.com/pro

System information as of Sun Jun 1 19:50:42 UTC 2025
System load: 0.08 Processes: 112
Usage of /: 4.9% of 44.96GB Users logged in: 0
Memory usage: 26% IPv4 address for ens3: 10.0.0.208
Swap usage: 0%

Expanded Security Maintenance for Applications is not enabled.
0 updates can be applied immediately.
Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status
The list of available updates is more than a week old.
To check for new updates run: sudo apt update
New release '24.04.2 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

The programs included with the Ubuntu system are free software; the exact distribution terms for each program are described in the individual files in /usr/share/doc/*/copyright.
Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by applicable law.
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```


2025-06-01T12:58:10-07:00
Going to SSH with cursor, and bring the project and context there:

# Project: Jekyll Blog Chatbot with RAG

## Overview
Adding an interactive chatbot to an existing Jekyll blog (jwt625.github.io) hosted on GitHub Pages. The chatbot will use RAG (Retrieval Augmented Generation) to answer questions based on blog content.

## Infrastructure
- Frontend: Existing Jekyll blog on GitHub Pages
- Backend: Oracle Cloud Free Tier VM
- LLM: DeepSeek API
- Authentication: Supabase (Free Tier)
- Database: PostgreSQL (on Oracle VM)
- Vector DB: ChromaDB (on Oracle VM)

## Core Features
1. Chat interface integrated into Jekyll blog
2. User authentication
3. Conversation logging
4. RAG capabilities over blog content
5. Semantic search

## Technical Stack
- Backend Framework: FastAPI
- Database: PostgreSQL + ChromaDB
- Web Server: Nginx
- SSL: Let's Encrypt
- Python Requirements:
  - FastAPI
  - Uvicorn
  - SQLAlchemy
  - psycopg2-binary
  - chromadb
  - python-jose
  - passlib
  - python-multipart

## Project Structure
```
`/home/ubuntu/chatbot/`
├── app/
│  ├── init.py
│  ├── main.py
│  ├── config.py
│  ├── database.py
│  └── api/
│     ├── init.py
│     ├── auth.py
│     ├── chat.py
│     └── users.py
├── alembic/
│  └── versions/
├── tests/
├── requirements.txt
└── alembic.ini

```


## Current Status
- Oracle Cloud VM is set up
- SSH access is configured
- Ready to begin backend infrastructure setup

## Next Steps
1. Initial server setup and security
2. Install dependencies
3. Configure PostgreSQL and ChromaDB
4. Set up Python environment
5. Deploy FastAPI application
6. Configure Nginx and SSL
7. Implement authentication
8. Develop chat interface
9. Integrate with Jekyll blog

## Security Requirements
- JWT-based authentication
- HTTPS encryption
- Rate limiting
- Input validation
- Secure API key storage
- Regular backups

## Database Schema (PostgreSQL)
```sql
CREATE TABLE conversations (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    user_id UUID REFERENCES auth.users(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW()),
    title TEXT
);

CREATE TABLE messages (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    conversation_id UUID REFERENCES conversations(id),
    content TEXT,
    role TEXT CHECK (role IN ('user', 'assistant')),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW())
);
```

## Free Tier Services Being Used
1. Oracle Cloud VM (Always Free)
   - 4 ARM-based Compute instances (24 GB RAM total)
   - 200 GB storage
2. Supabase (Free Tier)
   - Authentication
   - 500MB PostgreSQL database
   - 50,000 monthly active users
3. GitHub Pages (Free)
   - Static site hosting

## Goals
1. Minimize reliance on paid services
2. Maintain data privacy
3. Ensure scalability
4. Implement proper logging and monitoring
5. Future extension for image search capabilities


# Backend chat server
2025-06-01T13:20:42-07:00
Installing shit on the oracle instance.

Current backend status:

The basic structure is now set up. Here's what we've accomplished:
1. Installed system dependencies (Python, PostgreSQL, Nginx)
2. Created virtual environment
3. Set up project structure
4. Created core application files:
- requirements.txt with all dependencies
- config.py for environment settings
- database.py for SQLAlchemy setup
- main.py with FastAPI application
- Basic directory structure for the project

Would you like me to proceed with:
1. Setting up PostgreSQL configuration
2. Creating the database and user
3. Setting up the initial migration with Alembic
4. Or something else?


2025-06-01T14:04:51-07:00
Done setting up the git repo.
FastAPI server running
```
ubuntu@instance-20250601-1223:~$ cd /home/ubuntu/chatbot && source venv/bin/activate && uvicorn app.main:app --host 0.0.0.0 --port 8000
INFO:     Started server process [9955]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)

```


2025-06-01T14:17:30-07:00
lost connection. Going to reboot it.

2025-06-01T19:19:35-07:00
Reboot done. Got a haircut and food.
Setup 4 GB swap since the oracle cloud free tier only has 1 GB memory.

Serve RAG:
`cd /home/ubuntu/chatbot && source venv/bin/activate && uvicorn app.main:app --host 0.0.0.0 --port 8000`

```
============================================ warnings summary =============================================

venv/lib/python3.10/site-packages/pydantic/_internal/_config.py:271

/home/ubuntu/chatbot/venv/lib/python3.10/site-packages/pydantic/_internal/_config.py:271: PydanticDeprecat

edSince20: Support for class-based `config` is deprecated, use ConfigDict instead. Deprecated in Pydantic V2

.0 to be removed in V3.0. See Pydantic V2 Migration Guide at https://errors.pydantic.dev/2.5/migration/

warnings.warn(DEPRECATION_MESSAGE, DeprecationWarning)

  

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html

========================================= short test summary info ==========================================

FAILED tests/rag/test_ingestion.py::test_fetch_content_from_github - AssertionError: assert <Mock name='mock

.text' id='136254739074640'> == '---\nlayout: post\ntitle: Test ...

FAILED tests/rag/test_ingestion.py::test_update_content_success - AssertionError: assert 'error' == 'success

'

================================= 2 failed, 10 passed, 1 warning in 19.11s =================================
```


2025-06-01T20:22:58-07:00
RAG test with just the most recent post:
`curl -X POST http://localhost:8000/rag/update -H "Content-Type: application/json" -d '{"most_recent_only": true}'`

2025-06-01T20:35:23-07:00
RAG/update endpoint working:
```bash
(venv) ubuntu@instance-20250601-1223:~/chatbot$ source venv/bin/activate && uvicorn app.main:app --host 0.0.0.0 --port 8000 --log-level debug
2025-06-02 03:32:47,854 - app.rag.ingestion - INFO - Initializing ContentIngester
2025-06-02 03:32:47,882 - chromadb.telemetry.product.posthog - INFO - Anonymized telemetry enabled. See                     https://docs.trychroma.com/telemetry for more information.
2025-06-02 03:32:48,394 - app.rag.ingestion - INFO - Connected to ChromaDB collection: blog_content
2025-06-02 03:32:48,394 - app.rag.text_processing - INFO - TextProcessor initialized with chunk_size=500, chunk_overlap=100
INFO:     Started server process [8423]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
2025-06-02 03:33:32,138 - app.rag.ingestion - INFO - Starting content update
2025-06-02 03:33:32,262 - app.rag.ingestion - INFO - Fetching files from: https://api.github.com/repos/jwt625/jwt625.github.io/contents/_posts
2025-06-02 03:33:32,457 - httpx - INFO - HTTP Request: GET https://api.github.com/repos/jwt625/jwt625.github.io/contents/_posts "HTTP/1.1 200 OK"
2025-06-02 03:33:32,459 - app.rag.ingestion - INFO - Found 74 blog posts
2025-06-02 03:33:32,459 - app.rag.ingestion - INFO - Selected most recent post: 2025-05-26-weekly-OFS-48.md
2025-06-02 03:33:32,459 - app.rag.ingestion - INFO - Downloading: 2025-05-26-weekly-OFS-48.md from https://raw.githubusercontent.com/jwt625/jwt625.github.io/master/_posts/2025-05-26-weekly-OFS-48.md
2025-06-02 03:33:32,560 - httpx - INFO - HTTP Request: GET https://raw.githubusercontent.com/jwt625/jwt625.github.io/master/_posts/2025-05-26-weekly-OFS-48.md "HTTP/1.1 200 OK"
2025-06-02 03:33:32,561 - app.rag.ingestion - INFO - Processing 1 posts
2025-06-02 03:33:32,562 - app.rag.ingestion - INFO - Processing post: 2025-05-26-weekly-OFS-48.md
2025-06-02 03:33:32,562 - app.rag.text_processing - INFO - Processing post: 2025-05-26-weekly-OFS-48.md
2025-06-02 03:33:32,565 - app.rag.text_processing - INFO - Generated 41 chunks for post: 2025-05-26-weekly-OFS-48.md
2025-06-02 03:33:32,566 - app.rag.ingestion - INFO - Generated 41 chunks for post: 2025-05-26-weekly-OFS-48.md
2025-06-02 03:33:32,566 - app.rag.ingestion - INFO - Upserting 41 chunks to ChromaDB
2025-06-02 03:34:01,951 - app.rag.ingestion - INFO - Successfully stored chunks in ChromaDB
2025-06-02 03:34:01,982 - app.rag.ingestion - INFO - Updated 1 posts with 41 chunks
INFO:     127.0.0.1:51742 - "POST /rag/update HTTP/1.1" 200 OK
INFO:     127.0.0.1:36820 - "GET /rag/status HTTP/1.1" 200 OK
```


Query example:
`curl -X POST http://localhost:8000/rag/search -H "Content-Type: application/json" -d '{"query": "What was discussed about quantum computing?", "limit": 3}'`

2025-06-01T21:27:43-07:00
Holy shit inserted 2600+ chunks, took quite a while, 23 min:
```
2025-06-02 03:56:54,983 - app.rag.ingestion - INFO - Upserting 2641 chunks to ChromaDB
2025-06-02 04:19:04,530 - app.rag.ingestion - INFO - Successfully stored chunks in ChromaDB
2025-06-02 04:19:04,599 - app.rag.ingestion - INFO - Updated 74 posts with 2641 chunks
INFO:     127.0.0.1:49836 - "POST /rag/update HTTP/1.1" 200 OK

```

2025-06-01T21:53:57-07:00
deepseek api tested.

Run fastAPI with debug:
`source venv/bin/activate && uvicorn app.main:app --host 0.0.0.0 --port 8000 --log-level debug`

2025-06-01T22:15:41-07:00
RAG tested:

```json
{
    "answer": "Based on the provided context, here are the latest developments in quantum cryptography, specifically focusing on **post-quantum cryptography (PQC)** and NIST standards:\n\n1. **NIST Standardization Timeline**:\n   - NIST initiated a call for proposals in **2017** to identify quantum-resistant cryptographic algorithms.\n   - In **2022**, NIST selected **4 algorithms** for standardization.\n   - The standards were officially released in **2024**, marking a significant milestone in PQC adoption [This week's outside five sigma (#38) (Date: Unknown)].\n\n2. **Deployment in Industry**:\n   - Some of these quantum-resistant algorithms have already been deployed in real-world applications, such as **AWS (Amazon Web Services)** [This week's outside five sigma (#38) (Date: Unknown)].\n   - The urgency stems from the \"**harvest now, decrypt later**\" threat, where attackers collect encrypted data today to decrypt it later using quantum computers.\n\n3. **Key References**:\n   - **FIPS 203 (ML-KEM)**: A standardized PQC algorithm already implemented in AWS cloud services. A presentation titled *\"Building Post-Quantum Cloud Services: Deploying FIPS 203:ML-KEM at AWS\"* discusses its deployment [This week's outside five sigma (#38) (Date: Unknown)].\n   - **NIST Workshop**: Further guidance on Key Encapsulation Mechanisms (KEMs) is available through NIST events like the *\"Workshop on Guidance for KEMs\"* [This week's outside five sigma (#38) (Date: Unknown)].\n\n### Limitations:\n- The context does not provide details on the **technical workings** of the selected algorithms (e.g., lattice-based, hash-based).\n- No information is given about other quantum cryptography advancements (e.g., quantum key distribution (QKD)).\n\nFor deeper insights, you may explore the linked NIST resources and AWS documentation cited in the blog. Let me know if you'd like help summarizing those as well!",
    "context_used": [
        {
            "content": "- EverBright - [EB-FCP-300-135-0976-0.5-V](https://v4.cecdn.yun300.cn/100001_2205175052/EB-FCP-300-105-0976-0.5-V.pdf) ![20250311_154748_0.jpg](/assets/images/2025/20250108_20250318/20250311_154748_0.jpg) ![20250311_154748_1.jpg](/assets/images/2025/20250108_20250318/20250311_154748_1.jpg) # Post-Quantum Cryptography and NIST Standards In addition to the recent quantum computing news from the big players, there are also relatively quiet and recent development in post-quantum encryption.",
            "metadata": {
                "categories": "OFS",
                "chunk_index": "55",
                "header": "{\"cover\": \"/assets/images/2025/20250108_20250318/20250308_164713_1.jpg\", \"overlay_image\": \"/assets/images/2025/20250108_20250318/20250308_164713_1.jpg\", \"show_overlay_excerpt\": false, \"overlay_filter\": 0.5}",
                "post_name": "2025-03-17-weekly-OFS-38.md",
                "tags": "Electroreception, Laser_Doppler_Vibrometry, Pulse_Tube_Cryocooler, Super_resolution_Microscopy, Expansion_Microscopy, Quantum_Frequency_Conversion, Silicon_Carbide_Waveguide, Lithium_Niobate, Optical_Modulator, Photonics, Chu_Harrington_Limit, Graphene_Electrode, Superconducting_Nanowire_SPD, Quantum_Computing, Trapped_Ion_Qubit, Post_Quantum_Cryptography, ADC, Microfluidics, Structured_Light, Optical_Machine_Gun, Micro_Transfer_Printing, Nanofab, Plasmonic_Modulator, MEMS_Switch, Vortex_Beam, Low_Coherence_Light_Source, 2D_Metal, Soft_Gripper, Robotics, Near_Infrared_Spectroscopy, Annular_Core_Fiber",
                "title": "This week's outside five sigma (#38)",
                "toc": "True",
                "toc_sticky": "True",
                "total_chunks": "100",
                "url": "https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2025-03-17-weekly-OFS-38.md",
                "use_math": "True"
            },
            "distance": 0.6276986002922058
        },
        {
            "content": "- EverBright - [EB-FCP-300-135-0976-0.5-V](https://v4.cecdn.yun300.cn/100001_2205175052/EB-FCP-300-105-0976-0.5-V.pdf) ![20250311_154748_0.jpg](/assets/images/2025/20250108_20250318/20250311_154748_0.jpg) ![20250311_154748_1.jpg](/assets/images/2025/20250108_20250318/20250311_154748_1.jpg) # Post-Quantum Cryptography and NIST Standards In addition to the recent quantum computing news from the big players, there are also relatively quiet and recent development in post-quantum encryption. It's relevant today because you could \"harvest now, decrypt later\". NIST had a call for proposal in 2017, selected 4 quantum-resistant algorithms for standardization in 2022, and released the standards in 2024. Some of them have already been deployed at e.g. AWS. I don't know how they work (yet), and just thought time flies and these are actually already in action. ![20250312_014831_0.jpg](/assets/images/2025/20250108_20250318/20250312_014831_0.jpg) ![20250312_014831_1.jpg](/assets/images/2025/20250108_20250318/20250312_014831_1.jpg) - Fig. 1 from [Building Post-Quantum Cloud Services: Deploying FIPS 203:ML-KEM at AWS](https://csrc.nist.gov/Presentations/2025/building-post-quantum-cloud-services) - See more at [NIST Workshop on Guidance for KEMs](https://csrc.nist.gov/events/2025/workshop-on-guidance-for-kems) - Fig. 2 from [Using Post-Quantum Cryptography on AWS - Appendix: PQC details - The QC risk](https://catalog.workshops.aws/using-pq-crypto-on-aws/en-US/800-appendix/2-qc-risk) ![20250312_015048_0.jpg](/assets/images/2025/20250108_20250318/20250312_015048_0.jpg) # Evolution of Analog-to-Digital Converters The first commercial ADC from 1954: 11 bit, 50 kHz sample rate, 150 lbs, 500 W, $8,500 \"ADC\" at similar frequency today: 24 bit, 192 kHz, <3 lbs, <2.5 W, ~$100 ![20250312_020421_0.jpg](/assets/images/2025/20250108_20250318/20250312_020421_0.jpg) ![20250312_020421_1.jpg](/assets/images/2025/20250108_20250318/20250312_020421_1.jpg) - Fig. 1: [The Data Conversion Handbook, 2005, Chapter 1](https://analog.com/en/resources/technical-books/data-conversion-handbook.html) - Fig. 2: [behringer - UMC404HD](https://behringer.com/product.html?modelCode=0805-AAT) - ([datasheet](https://fullcompass.com/common/files/24700-BehringerUMC404HDDatasheet.pdf)) ![20250312_020614_0.jpg](/assets/images/2025/20250108_20250318/20250312_020614_0.jpg) # On-Chip Thermal Emission for MIR and LWIR Found an integrated \"incandescent light bulb\": heating an on-chip floating silicon nitride waveguide to >1000 C Efficiency is only ~1e-10 because it is radiating everywhere but hey you get broadband emission and some rare MIR and LWIR power on a chip. ![20250312_061310_0.jpg](/assets/images/2025/20250108_20250318/20250312_061310_0.jpg) ![20250312_061310_1.jpg](/assets/images/2025/20250108_20250318/20250312_061310_1.jpg) - Pruessner2025: [A waveguide thermal emitter](https://doi.org/10.1063/5.0252536) ![20250312_061412_0.jpg](/assets/images/2025/20250108_20250318/20250312_061412_0.jpg) ![20250312_061412_1.jpg](/assets/images/2025/20250108_20250318/20250312_061412_1.jpg) # Inertial Focusing in Microfluidics There is an effect called \"inertial focusing\" in inertial microfluidics, it involves multiple forces with different scalings, and let you control the focusing positions. ![20250313_013839_0.jpg](/assets/images/2025/20250108_20250318/20250313_013839_0.jpg) ![20250313_013839_1.jpg](/assets/images/2025/20250108_20250318/20250313_013839_1.jpg) - Wang2013: [Vortex-aided inertial microfluidic device for continuous particle separation with high size-selectivity, efficiency, and purity](https://doi.org/10.1063/1.4818906) - Zhou2013: [Fundamentals of inertial focusing in microchannels](https://doi.org/10.1039/C2LC41248A) - Looks fun but must be pretty ![20250313_014116_0.jpg](/assets/images/2025/20250108_20250318/20250313_014116_0.jpg) ![20250313_014116_1.jpg](/assets/images/2025/20250108_20250318/20250313_014116_1.jpg) - what the hell this is crazy: (Hydroporation employs microfluidic vortex shedding (\u00b5VS), a hydrodynamic phenomenon whereby oscillating fluid forces gently permeabilize the cell membrane, allowing delivery of gene editing payloads such as Cas9 ribonucleoprotein (RNP) complexes.) - Sytsma2025: [Scalable intracellular delivery via microfluidic vortex shedding enhances the function of chimeric antigen receptor T-cells](https://doi.org/10.1038/s41598-025-89070-5) ![20250313_043943_0.jpg](/assets/images/2025/20250108_20250318/20250313_043943_0.jpg) # High-Accuracy 3D Face Scanning with Structured Light This is what you need to scan someone's face with ~100 um RMSE at half a meter away. It is using structured light from three integrated fringe masks, carefully calibrated and projected with 120 deg phase offset (thus no need for mechanical projectors), plus some magic reconstruction, you get the high accuracy 3D face. (PAMPA: pre-aligned micro-projection array) ![20250313_035520_0.jpg](/assets/images/2025/20250108_20250318/20250313_035520_0.jpg) ![20250313_035520_1.jpg](/assets/images/2025/20250108_20250318/20250313_035520_1.jpg) - Guo2025: [Real-time high-accuracy 3D facial (micro) expression measurement based on pre-aligned micro-projection arrays](https://doi.org/10.1364/OE.555406) ![20250313_035632_0.jpg](/assets/images/2025/20250108_20250318/20250313_035632_0.jpg) - Is this the OG structured light paper? - Srinivasan1984: [Automated phase-measuring profilometry of 3-D diffuse objects](https://doi.org/10.1364/AO.23.003105) ![20250313_035746_0.jpg](/assets/images/2025/20250108_20250318/20250313_035746_0.jpg) ![20250313_035746_1.jpg](/assets/images/2025/20250108_20250318/20250313_035746_1.jpg) - They have an older version with just one mask and a stepper motor to shift its phase. They complain about its step size inconsistency and it needs feedback control. - Guo2024: [High accuracy, compact 3D face imaging method based on translational and online-switchable phase-shifting fringe projection](https://doi.org/10.1364/OE.513951) ![20250313_040406_0.jpg](/assets/images/2025/20250108_20250318/20250313_040406_0.jpg) ![20250313_040406_1.jpg](/assets/images/2025/20250108_20250318/20250313_040406_1.jpg) # Optical Machine Gun for Microparticle Acceleration BREAKING: shots fired just outside an ACF, multiple micro particles ejected at high speed. Full story coming up on the evening news. (They actually called it an \"optical machine gun\"... Images of the light beam outside the fiber is actually real, from fluorescence dye rhodamine B. ACF: annular-core capillary fiber) ![20250313_042819_0.jpg](/assets/images/2025/20250108_20250318/20250313_042819_0.jpg) ![20250313_042819_1.jpg](/assets/images/2025/20250108_20250318/20250313_042819_1.jpg) ![20250313_042819_2.jpg](/assets/images/2025/20250108_20250318/20250313_042819_2.jpg) - Du2025: [Capillary fiber-based optical machine gun for micro particle continuous shooting](https://doi.org/10.1364/OE.554337) - The \"high speed\" is about 1 mm/s. ![20250313_043535_0.jpg](/assets/images/2025/20250108_20250318/20250313_043535_0.jpg) ![20250313_043535_1.jpg](/assets/images/2025/20250108_20250318/20250313_043535_1.jpg) # Micro-Transfer Printing for Photonic Integration I've done some transfer print with PDMS myself. Pretty crazy how big of a piece (40 um x 1 mm) they could pick up and put down. I had no idea there's already a commercial micro-transfer printing tool (X-Celeprint \u00b5TP-100). ![20250313_160526_0.jpg](/assets/images/2025/20250108_20250318/20250313_160526_0.jpg) ![20250313_160526_1.jpg](/assets/images/2025/20250108_20250318/20250313_160526_1.jpg) - Vandekerckhove2025: [A scalable quadratic nonlinear silicon photonics platform with printable entangled photon-pair sources](https://arxiv.org/pdf/2503.08783) - No mention of how fast it can be done, but I do like the argument of \"PPLN devices would typically be only a small part of a larger ![20250313_160924_0.jpg](/assets/images/2025/20250108_20250318/20250313_160924_0.jpg) # Ultra-Wideband Plasmonic Electro-Optic Modulators A plasmonic + electro-optic polymer modulator with a bandwidth of, *checks notes*, 997 GHz! They have to use eight different sources based on electrical mixers to generate signals across the frequency range from 10 MHz to 1.14 THz I want to see their probing setup.. ![20250313_161912_0.jpg](/assets/images/2025/20250108_20250318/20250313_161912_0.jpg) - 10 um length, IL = 5.6 dB, Vpi*L = 234 V*um. - Horst2025: [Ultra-wideband MHz to THz plasmonic EO modulator](https://doi.org/10.1364/OPTICA.544016) - They must have some close friends at Virginia Diodes. ![20250313_162129_0.jpg](/assets/images/2025/20250108_20250318/20250313_162129_0.jpg) # MOEMS Optical Switching and Flexure Mechanics \"Mega\" structures like this on a chip for tiny photons make me yearn for mega flexure structures irl. ![20250314_015149_0.jpg](/assets/images/2025/20250108_20250318/20250314_015149_0.jpg) ![20250314_015149_1.jpg](/assets/images/2025/20250108_20250318/20250314_015149_1.jpg) - Heidari2025: [Reconfigurable electrostatically actuated 1 \u00d7 5 rotary MOEMS switch](https://doi.org/10.1364/OE.545403) - They have a nice table for \"Comparison of various switching mechanisms employing silicon nitride as the waveguide material\" ![20250314_015348_0.jpg](/assets/images/2025/20250108_20250318/20250314_015348_0.jpg) # High-Dimensional Structured Light and Vortex Beams You think you know about light? Think twice. (too many buzz words tbh, what the hell is a flying donut?? Why can't you be normal and call it radially polarized beam. Vortex beam? Could've called them quantum tornadoes in polarization space) ![20250314_020148_0.jpg](/assets/images/2025/20250108_20250318/20250314_020148_0.jpg) - He2022: [Towards higher-dimensional structured light](https://doi.org/10.1038/s41377-022-00897-3) - \"I added up a giant superposition of separable states (plane waves), and now the spatial and polarization dofs are non-separable, how surprising!\" ![20250314_020713_0.jpg](/assets/images/2025/20250108_20250318/2",
            "metadata": {
                "categories": "['OFS']",
                "chunk_index": "106",
                "header": "{'cover': '/assets/images/2025/20250108_20250318/20250308_164713_1.jpg', 'overlay_image': '/assets/images/2025/20250108_20250318/20250308_164713_1.jpg', 'show_overlay_excerpt': False, 'overlay_filter': 0.5}",
                "post_name": "2025-03-17-weekly-OFS-38.md",
                "tags": "['Electroreception', 'Laser_Doppler_Vibrometry', 'Pulse_Tube_Cryocooler', 'Super_resolution_Microscopy', 'Expansion_Microscopy', 'Quantum_Frequency_Conversion', 'Silicon_Carbide_Waveguide', 'Lithium_Niobate', 'Optical_Modulator', 'Photonics', 'Chu_Harrington_Limit', 'Graphene_Electrode', 'Superconducting_Nanowire_SPD', 'Quantum_Computing', 'Trapped_Ion_Qubit', 'Post_Quantum_Cryptography', 'ADC', 'Microfluidics', 'Structured_Light', 'Optical_Machine_Gun', 'Micro_Transfer_Printing', 'Nanofab', 'Plasmonic_Modulator', 'MEMS_Switch', 'Vortex_Beam', 'Low_Coherence_Light_Source', '2D_Metal', 'Soft_Gripper', 'Robotics', 'Near_Infrared_Spectroscopy', 'Annular_Core_Fiber']",
                "title": "This week's outside five sigma (#38)",
                "toc": "True",
                "toc_sticky": "True",
                "total_chunks": "120",
                "url": "https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2025-03-17-weekly-OFS-38.md",
                "use_math": "True"
            },
            "distance": 0.7374597787857056
        },
        {
            "content": "It's relevant today because you could \"harvest now, decrypt later\". NIST had a call for proposal in 2017, selected 4 quantum-resistant algorithms for standardization in 2022, and released the standards in 2024. Some of them have already been deployed at e.g. AWS. I don't know how they work (yet), and just thought time flies and these are actually already in action.",
            "metadata": {
                "categories": "OFS",
                "chunk_index": "56",
                "header": "{\"cover\": \"/assets/images/2025/20250108_20250318/20250308_164713_1.jpg\", \"overlay_image\": \"/assets/images/2025/20250108_20250318/20250308_164713_1.jpg\", \"show_overlay_excerpt\": false, \"overlay_filter\": 0.5}",
                "post_name": "2025-03-17-weekly-OFS-38.md",
                "tags": "Electroreception, Laser_Doppler_Vibrometry, Pulse_Tube_Cryocooler, Super_resolution_Microscopy, Expansion_Microscopy, Quantum_Frequency_Conversion, Silicon_Carbide_Waveguide, Lithium_Niobate, Optical_Modulator, Photonics, Chu_Harrington_Limit, Graphene_Electrode, Superconducting_Nanowire_SPD, Quantum_Computing, Trapped_Ion_Qubit, Post_Quantum_Cryptography, ADC, Microfluidics, Structured_Light, Optical_Machine_Gun, Micro_Transfer_Printing, Nanofab, Plasmonic_Modulator, MEMS_Switch, Vortex_Beam, Low_Coherence_Light_Source, 2D_Metal, Soft_Gripper, Robotics, Near_Infrared_Spectroscopy, Annular_Core_Fiber",
                "title": "This week's outside five sigma (#38)",
                "toc": "True",
                "toc_sticky": "True",
                "total_chunks": "100",
                "url": "https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2025-03-17-weekly-OFS-38.md",
                "use_math": "True"
            },
            "distance": 0.7609630823135376
        }
    ]
}
```

## Production?
2025-06-01T22:22:08-07:00

Here's what we've set up:

1. Security Features:
- Rate limiting (20 requests/minute)
- API key authentication
- JWT-based user authentication
- Request validation
- CORS protection
- Trusted host middleware
- Gzip compression

2. Chat & Session Management:
- User accounts and authentication
- Chat sessions with history
- Message persistence in PostgreSQL
- Context tracking for each message
- Conversation history in prompts

3. Production Setup:
- Logging with rotation
- Error tracking with Sentry
- Database connection pooling
- Response compression
- Disabled API docs in production
1. Next Steps:

To deploy this to production, you'll need to:

1. Set up a production environment:
```bash
# Install production dependencies
pip install gunicorn uvicorn[standard] psycopg2-binary sentry-sdk
# Create production .env file
cp .env.example .env.prod
```

2. Configure environment variables in .env.prod:
```env
POSTGRES_USER=your_prod_user
POSTGRES_PASSWORD=your_secure_password
POSTGRES_HOST=your_db_host
POSTGRES_DB=your_prod_db
JWT_SECRET_KEY=your_secure_jwt_key
DEEPSEEK_API_KEY=your_deepseek_key
SENTRY_DSN=your_sentry_dsn
ALLOWED_HOSTS=your-domain.com
```

3. Set up a reverse proxy (e.g., Nginx):
```nginx
server {
    listen 80;
    server_name your-domain.com;
    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

4. Run with production server:
```bash
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

5. Set up monitoring:
- Configure Sentry for error tracking
- Set up logging rotation
- Monitor server resources

1. Regular maintenance:
- Database backups
- Log rotation
- Security updates
- SSL certificate renewal

Would you like me to help you implement any of these specific steps or provide more details about any part of the setup?










## Continue with claude code
2025-06-06T00:18:00-07:00
Damn claude code is good.

lmao:
```
Bash(sleep 3 && curl -X POST "http://localhost:8000/rag/generate" -H "Content-Type:
      application/json" -H "Authorization: Bearer $TOKEN" -d '{"query": "What are recen…)
  ⎿ {
      "detail": "Not authenticated"
    }
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
100    83    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100   113  100    30  100    835  0:00:01  0:00:01 --:--:--    65
100   113  100    30  100    83     14     39 53
    0:00:02  0:00:02 --:--:--    53

● The token expired! Let me get a fresh token and test again:
```




## Firewall config
2025-06-07T15:33:56-07:00
locally it is working, but I cannot eevn ping the server.


hmm:
● Bash(sudo ufw allow 8000/tcp)
  ⎿  Rules updated
     Rules updated (v6)

● Bash(sudo ufw --force enable)
  ⎿  Firewall is active and enabled on system startup

● Bash(sudo ufw status)
  ⎿  Status: active

     To                         Action      From
     --                         ------      ----
     8000/tcp                   ALLOW       Anywhere
     8000/tcp (v6)              ALLOW       Anywhere (v6)



 What You Need to Do in OCI Console:

  1. Find your instance's VCN:
    - Go to Compute → Instances → Your Instance
    - Note the Virtual Cloud Network name
  2. Configure Security List:
    - Go to Networking → Virtual Cloud Networks → [Your VCN]
    - Click Security Lists → Default Security List (or the one your
  instance uses)
    - Click Add Ingress Rules
    - Add:
    Source Type: CIDR
  Source CIDR: 0.0.0.0/0
  IP Protocol: TCP
  Destination Port Range: 8000
  Description: RAG API Server
  3. Test Access:
  Once configured, test with:
  curl -s http://YOUR_INSTANCE_PUBLIC_IP:8000/

  Alternative: Use Standard Ports

  If you prefer not to open custom ports, you could:

● Write(chatbot/start_on_port_80.sh)


2025-06-07T15:37:48-07:00
ok seems like I need to go to VCN "instance", route table, and then I see route rules.
![[Pasted image 20250607154016.png]]

Ok seems like the wrong place. At the create security list, I could allow rules for ingress
- 2025-06-07T15:43:34-07:00 created one for the chatbot at port 8000
- `Go to Security Lists, not Route Rules, and add the ingress rule for 
  port 8000 with source CIDR 0.0.0.0/0.`
- ok seems like 8000 should be destination port, not source port. Make sense...
- so source of `0.0.0.0/0` basically means allowing everything?


```

  To Fix This:

  1. Go to OCI Console
  2. Navigate to: Networking → Virtual Cloud Networks → [Your VCN] →
  Security Lists
  3. Add Ingress Rule:
    - Source CIDR: 0.0.0.0/0
    - IP Protocol: TCP
    - Destination Port Range: 8000
```

2025-06-07T15:50:58-07:00
- ohh maybe I should add it to the default security list instead of creating a new list.
- Let's see.
- nope. Still not working.

```

  Common OCI Security List Issues:

  1. Check Rule Configuration:

  Your ingress rule should look like:
  Source Type: CIDR
  Source CIDR: 0.0.0.0/0
  IP Protocol: TCP
  Source Port Range: All (leave blank)
  Destination Port Range: 8000
  Stateful: YES (this is usually the default)

  2. Double-check you're editing the RIGHT security list:
```


Hmm all seem fine to me. Maybe we need a new security group?

Why do I have two of this shit?
![[Pasted image 20250607155806.png]]

2025-06-07T16:02:35-07:00
hmm maybe it's the network security group of the instance?
![[Pasted image 20250607160249.png]]
- just added, let's see.

```
  Most OCI security lists block ping by default. You might need:
  Ingress Rule:
  Source: 0.0.0.0/0
  IP Protocol: ICMP
  Type and Code: All
```


There i a freaking subnet, and with a security list attached to it, the default.
And the default security list's security rules already have the added port 8000 ingress rules.
```

● If port 80 doesn't work either (despite SSH working), this points to a
  few specific issues:

  Likely Causes:

  1. Network Security Groups (NSGs) Override

  If your instance has NSGs attached, they override security list rules:
  - Go to Compute → Instances → Your Instance
  - Look for Network Security Groups section
  - If any NSGs are listed, you need to add rules to the NSG, not the
  security list

  2. Regional vs AD-specific Security Lists

  - Make sure you're editing the security list in the correct 
  Availability Domain
  - Some VCNs have different security lists per AD

  3. Instance-level Firewall Still Active

  Even though we enabled UFW for port 8000, let's check if it's blocking
  HTTP:

  Check if the server is actually running and accessible locally:
  curl -v http://localhost/
  curl -v http://localhost:8000/

  4. ICMP (Ping) Typically Blocked by Default

  OCI blocks ping by default, so that's normal. But HTTP should work.

  Key Questions:

  1. Does your instance show any Network Security Groups attached?
  2. Is the server actually running on the expected port?

  The fact that SSH works but HTTP doesn't, despite having ingress rules,
   strongly suggests either:
  - NSGs are overriding the security list rules, or
  - Server isn't actually listening on the expected interface

  Can you check if your instance has any Network Security Groups attached
   in the OCI console?
```


2025-06-07T16:13:53-07:00
Wait a sec, the subnet is not connected to internet? Why can I ssh??
![[Pasted image 20250607161410.png]]

ok it does say this VNC already has an internet gateway:
![[Pasted image 20250607161439.png]]

2025-06-07T16:21:05-07:00
Trying out the OCI path analysis:
- https://cloud.oracle.com/networking/path-analyzer?region=us-sanjose-1

Hmm it seems fine:
![[Pasted image 20250607162203.png]]

2025-06-07T16:36:04-07:00
going to restart the instance...

```
wentaojiang@Mac RAG-chat-backend % curl -v http://146.235.193.141:8000/
*   Trying 146.235.193.141:8000...
* connect to 146.235.193.141 port 8000 from 192.168.12.43 port 59061 failed: Operation timed out
* Failed to connect to 146.235.193.141 port 8000 after 50645 ms: Couldn't connect to server
* Closing connection
curl: (28) Failed to connect to 146.235.193.141 port 8000 after 50645 ms: Couldn't connect to server
wentaojiang@Mac RAG-chat-backend % 
wentaojiang@Mac RAG-chat-backend % 
wentaojiang@Mac RAG-chat-backend % 
wentaojiang@Mac RAG-chat-backend % 
wentaojiang@Mac RAG-chat-backend % 
wentaojiang@Mac RAG-chat-backend % curl -v http://146.235.193.141:8000/
*   Trying 146.235.193.141:8000...
* connect to 146.235.193.141 port 8000 from 192.168.12.43 port 63894 failed: Connection refused
* Failed to connect to 146.235.193.141 port 8000 after 77 ms: Couldn't connect to server
* Closing connection
curl: (7) Failed to connect to 146.235.193.141 port 8000 after 77 ms: Couldn't connect to server
```

2025-06-07T16:45:18-07:00
Finally, only took **1 h 15 min** !
- the trick is to restart, who would have thought!

I got an answer!
```
wentaojiang@Mac RAG-chat-backend % curl -X POST "http://146.235.193.141:8000/rag/generate-test" \
    -H "Content-Type: application/json" \
    -d '{"query": "any recent progress and discussion on quantum cryptography?", "context_limit": 1}'
{"answer":"Based on the provided context, there is a brief mention of recent developments in **post-quantum cryptography** (PQC) in the blog post *This week's outside five sigma (#38)* (Date Unknown). Specifically, it highlights \"relatively quiet and recent developments in post-quantum encryption,\" though no further details are provided about specific algorithms, standards, or breakthroughs.  \n\nThe post also references a link to a document (possibly technical specifications) titled *EB-FCP-300-135-0976-0.5-V*, but its relevance to quantum cryptography is unclear from the context.  \n\n**Limitation**: The context does not provide enough information to summarize recent progress in quantum cryptography (e.g., NIST's PQC standardization updates, new attacks, or implementations). For a comprehensive answer, additional sources would be needed.  \n\n**Cited Source**:  \n- *This week's outside five sigma (#38)* (Date Unknown)","context_used":[{"content":"- EverBright - [EB-FCP-300-135-0976-0.5-V](https://v4.cecdn.yun300.cn/100001_2205175052/EB-FCP-300-105-0976-0.5-V.pdf) ![20250311_154748_0.jpg](/assets/images/2025/20250108_20250318/20250311_154748_0.jpg) ![20250311_154748_1.jpg](/assets/images/2025/20250108_20250318/20250311_154748_1.jpg) # Post-Quantum Cryptography and NIST Standards In addition to the recent quantum computing news from the big players, there are also relatively quiet and recent development in post-quantum encryption.","metadata":{"categories":"OFS","chunk_index":"55","header":"{\"cover\": \"/assets/images/2025/20250108_20250318/20250308_164713_1.jpg\", \"overlay_image\": \"/assets/images/2025/20250108_20250318/20250308_164713_1.jpg\", \"show_overlay_excerpt\": false, \"overlay_filter\": 0.5}","post_name":"2025-03-17-weekly-OFS-38.md","tags":"Electroreception, Laser_Doppler_Vibrometry, Pulse_Tube_Cryocooler, Super_resolution_Microscopy, Expansion_Microscopy, Quantum_Frequency_Conversion, Silicon_Carbide_Waveguide, Lithium_Niobate, Optical_Modulator, Photonics, Chu_Harrington_Limit, Graphene_Electrode, Superconducting_Nanowire_SPD, Quantum_Computing, Trapped_Ion_Qubit, Post_Quantum_Cryptography, ADC, Microfluidics, Structured_Light, Optical_Machine_Gun, Micro_Transfer_Printing, Nanofab, Plasmonic_Modulator, MEMS_Switch, Vortex_Beam, Low_Coherence_Light_Source, 2D_Metal, Soft_Gripper, Robotics, Near_Infrared_Spectroscopy, Annular_Core_Fiber","title":"This week's outside five sigma (#38)","toc":"True","toc_sticky":"True","total_chunks":"100","url":"https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2025-03-17-weekly-OFS-38.md","use_math":"True"},"distance":0.7594912648200989}]}%         
```

This is slow, took about (I forgot).

2025-06-07T16:53:55-07:00
going to time it as well as bump context limit to 3.
- `0.00s user 0.01s system 0% cpu 31.334 total`
- huh it must be the first run is loading some shit then.


## Rate limiting and auth
2025-06-07T16:52:26-07:00
Applied rate limiting to `app/api/rag.py`.

Also asking Claude to update README and CLAUDE.md.

ok how tf does auth work?

JWT Authentication Flow in Our System:
	cd ~/Docu
  1. JWT Structure
2. 

  JWTs are stateless tokens with three parts (separated by dots):
  header.payload.signature

  - Header: Algorithm info (HS256)
  - Payload: User data (username, user ID, expiration)
  - Signature: Cryptographic signature using secret key

  2. How We Implement It:

● Read(chatbot/app/security.py)
  ⎿  Read 13 lines (ctrl+r to expand)

● Read(chatbot/app/api/auth.py)
  ⎿  Read 20 lines (ctrl+r to expand)

● 3. Authentication Process:

  A. User Registration:

  1. Password Hashing: We store bcrypt-hashed passwords in PostgreSQL
  2. User Storage: User accounts saved to database
  3. No tokens yet: Registration doesn't create tokens

  B. Login Process:

  4. Credential Validation: Check username/password against database
  5. Token Creation: If valid, create JWT with user info:
  # In app/api/auth.py
  access_token = create_access_token(
      data={"sub": user.username, "id": user.id},  # Payload
      expires_delta=access_token_expires
  )

  C. Token Verification:

  6. No Database Lookup: JWT is stateless - we don't store tokens in
  database
  7. Signature Verification: We verify the token using our secret key:
  # In app/security.py  
  payload = jwt.decode(
      token,
      settings.JWT_SECRET_KEY.get_secret_value(),  # Secret key
      algorithms=[settings.JWT_ALGORITHM]          # HS256
  )

  8. Key Security Points:

  ✅ What We Store in Database:

  - User accounts: username, email, hashed_password
  - Chat sessions: linked to user IDs
  - Message history: conversation records

  ❌ What We DON'T Store:

  - JWT tokens: They're stateless and self-contained
  - Plain passwords: Only bcrypt hashes
  - Session data: Everything needed is in the JWT

  🔐 Security Features:

  # Token contains:
  {
    "sub": "username",        # Subject (username)
    "id": 1,                 # User ID for database queries
    "exp": 1749198000        # Expiration timestamp
  }

  5. Why JWT is Secure:

  6. Tamper-proof: Any modification breaks the signature
  7. Expiring: Tokens auto-expire (30 minutes in our config)
  8. Stateless: No server-side session storage needed
  9. Cryptographically signed: Uses HMAC with secret key

  10. Verification Flow:

  Client sends: Authorization: Bearer eyJ...
  ↓
  Server extracts token from header
  ↓
  Verify signature with secret key
  ↓
  Extract user info from payload
  ↓
  Use user.id for database operations

  The beauty of JWT is that it's self-contained - we don't need to store
   session data or look up tokens in the database. The token itself
  proves the user's identity through cryptographic verification.




# Usage
2025-06-19T16:56:47-07:00

```bash
curl -X POST "http://146.235.193.141:8000/rag/generate-test" -H "Content-Type: application/json" -d '{"query": "paper from one of the big telcos on using robots to unplug and plug fibers, and could move around in a warehouse or datacenter with vision", "context_limit": 3}'
```

```json
{"answer":"Based on the provided context, there is no direct mention of a paper from a major telecom company (telco) about using robots to unplug/plug fibers in a warehouse or data center with vision capabilities. However, here are the closest relevant points from the context:\n\n1. **Large-Scale Robotic Fiber Switch Array**: The blog references a robotic system capable of moving \"a few thousand fibers at the same time,\" though it doesn’t specify if this is in a warehouse/data center or involve vision [This week's outside five sigma (#31) (Date Unknown)].  \n\n2. **Telepresence Robot with Arms**: IIT’s hydraulic quadruped robot is mentioned as carrying \"massive arms,\" which could theoretically be adapted for fiber manipulation, but this is speculative and not tied to telcos or data centers [This week's outside five sigma (#34) (Date Unknown)].  \n\nIf you need details on telco-specific implementations, additional sources would be required. The current context only hints at robotic fiber manipulation at scale.","context_used":[{"content":"Either need an active cable or fiber optic. 9. Imagine doing computer vision research in 1966 as an undergrad summer project ![20240706_231654_0.jpg](/assets/images/2024/20240608_20241127/20240706_231654_0.jpg) - [Computer Vision: Algorithms and Applications, 2nd ed.](https://szeliski.org/Book/) 10.","metadata":{"categories":"OFS","chunk_index":"21","header":"{\"cover\": \"/assets/images/2024/20240608_20241127/20240608_041250_0.jpg\", \"overlay_image\": \"/assets/images/2024/20240608_20241127/20240608_041250_0.jpg\", \"show_overlay_excerpt\": false, \"overlay_filter\": 0.5}","post_name":"2024-11-30-weekly-OFS-23.md","tags":"Microwave, Tubes, VED, Quantum, Qubit, Pasta, COMSOL, Nanofab, TTV, Wafer, TFLN, Tapeout, HDMI, Fiber, CV, Camera, Ethernet, MXene, Energy_harvest, 3D_print, Wirebond, Photonics, Corrosion","title":"This week's outside five sigma (#23)","toc":"True","toc_sticky":"True","total_chunks":"33","url":"https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2024-11-30-weekly-OFS-23.md","use_math":"True"},"distance":0.9200011491775513},{"content":"Telepresence Anywhere - IIT’s hydraulic quadruped can carry a pair of massive arms](https://spectrum.ieee.org/telepresence-robot) ![20250211_211219_0.jpg](/assets/images/2025/20241101_20250216/20250211_211219_0.jpg) # Microplastics in Human Brains How micro/nano plastics might look like in our brains.","metadata":{"categories":"OFS","chunk_index":"33","header":"{\"cover\": \"/assets/images/2025/20241101_20250216/20250208_014851_1.jpg\", \"overlay_image\": \"/assets/images/2025/20241101_20250216/20250208_014851_1.jpg\", \"show_overlay_excerpt\": false, \"overlay_filter\": 0.5}","post_name":"2025-02-16-weekly-OFS-34.md","tags":"Copper_interconnects, Robotics, Optomechanics, SAW, LUS, Transient_grating_spectroscopy, Asynchronous_optical_sampling, Water_wave_diodes, Microfluidics, Capillary_waves, Acoustic_levitation, Ultrasound_phased_arrays, CMOS, Optical_tweezers, Photophoretic_trapping, Photonic_nanojet, Optical_whispering_gallery_modes, Dielectric_rod_antennas, Fiberscopic_robotics, Magnetic_manipulation, Sonic_boom_analysis, Niobium_superconducting_qubits, Metalens, Plasma_etching, Nanofab","title":"This week's outside five sigma (#34)","toc":"True","toc_sticky":"True","total_chunks":"73","url":"https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2025-02-16-weekly-OFS-34.md","use_math":"True"},"distance":0.9616615772247314},{"content":"waveguide and its application to a modularized broadband high-level squeezer](https://doi.org/10.1063/5.0063118) - The [actual article from Riken](https://riken.jp/pr/news/2024/20241108_2/index.html) ![20250118_012317_0.jpg](/assets/images/2025/20241101_20250124/20250118_012317_0.jpg) # Large-Scale Robotic Fiber Switch Array Have you seen a robot moving a few thousand fibers at the same time?","metadata":{"categories":"OFS","chunk_index":"4","header":"{\"cover\": \"/assets/images/2025/20241101_20250124/20241215_005740_0.jpg\", \"overlay_image\": \"/assets/images/2025/20241101_20250124/20241215_005740_0.jpg\", \"show_overlay_excerpt\": false, \"overlay_filter\": 0.5}","post_name":"2025-01-26-weekly-OFS-31.md","tags":"EDWA, MEMS, PPLN, LCoS, OFDR, DAS, Josephson_junction, SNSPD, Cryogenics, Waveguide, Photonics, WDM, Microlens, ALD, DRIE, Nanopillar, YOLOv7, CNN, Nanofab, Vibrometry, Holography, Deflectometry, Copackaged_optics, Benes_network","title":"This week's outside five sigma (#31)","toc":"True","toc_sticky":"True","total_chunks":"48","url":"https://github.com/jwt625/jwt625.github.io/blob/master/_posts/2025-01-26-weekly-OFS-31.md","use_math":"True"},"distance":0.9887418150901794}]}**%**
```


# Update 20250706
2025-07-06T23:59:37-07:00
Current database status:
- from running `./scripts/check_last_update.sh `
```

🔍 Checking Last Update Information
==================================

📁 ChromaDB Data Directory:
Last modified: 2025-06-02 04:32:01.387493255 +0000
Directory size: 213M

📝 Recent Update Activity in Logs:
Recent update-related log entries:
2025-06-08 00:07:27,279 - app.rag.ingestion - INFO - Initializing ContentIngester
2025-06-08 00:07:28,575 - app.rag.ingestion - INFO - Connected to ChromaDB collection: blog_content

📋 Update Log Files:
No update log files found

📊 Current System Status:
Current document count: 4451
Status: ok

🔄 Server Process Info:
Server PID: 5817
Server started: Sun Jun  8 00:07:07 2025
Server uptime: 29-06:51:48

💡 Summary:
To check for the most recent update activity:
1. Look at ChromaDB directory modification time above
2. Check the most recent update log files
3. Run the update script to see current document count

To perform a new update:
  ./scripts/update_blog_content.sh

To get detailed status (requires auth):
  curl -X GET "http://localhost:8000/rag/status" -H "Authorization: Bearer YOUR_TOKEN"
```

2025-07-07T00:13:22-07:00
Fixed some middleware error and wohoo update seems worked:
```
{"status":"success","message":"Updated 5 posts with 278 chunks","progress":{"stage":"complete","current":278,"total":278,"message":"Successfully stored all chunks"}}
```

These are satisfying to watch, poor little machine but it is free
![[Pasted image 20250707001730.png]]
![[Pasted image 20250707001744.png]]




# 20250721 Remove `_site` folder

2025-07-21T23:32:29-07:00


# Repository Cleanup: Removing `_site` from Git History

  

## Overview

This document explains the process used to remove the `_site` directory from the entire git history of the Jekyll blog repository, reducing repository size and cleaning up tracked generated files.

  

## Problem

- Repository size was 1.7GB due to tracking Jekyll's generated `_site` directory

- Generated HTML files were being committed to git history

- This caused unnecessary bloat and potential merge conflicts

  

## Solution: Git History Rewriting

  

### Tools Used

- **`git filter-repo`** - Modern, safe tool for rewriting git history

- **`git filter-branch`** - Alternative older method (not used)

  

### Step-by-Step Process

  

#### 1. Initial Assessment

```bash

# Check repository size

du -sh .git

# Result: 1.7G

  

# Identify largest files in history

git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sed -n 's/^blob //p' | sort -nr | head -20

# Confirmed most large files were in _site/

```

  

#### 2. Update .gitignore

```bash

# Uncommented lines in .gitignore:

_site/

.jekyll-metadata

```

  

#### 3. Remove _site from Entire Git History

```bash

# Use git filter-repo to remove _site directory from all commits

git filter-repo --path _site --invert-paths --force

```

  

**What this command does:**

- `--path _site` - Target the _site directory

- `--invert-paths` - Remove (rather than keep) the specified path

- `--force` - Required for safety confirmation

- Rewrites ALL commits in the repository history

  

#### 4. Re-add Remote and Push

```bash

# git filter-repo removes remotes for safety

git remote add origin https://github.com/jwt625/jwt625.github.io.git

  

# Force push the cleaned history

git push --force origin master

```

  

### Results

  

#### Before Cleanup

- `.git` directory: **1.7GB**

- Tracking generated HTML files in history

- 223 commits with _site content

  

#### After Cleanup

- `.git` directory: **1.6GB** (~100MB saved)

- Clean history with only source files

- All 223 commits rewritten (new SHAs)

  

### Important Notes

  

#### ⚠️ Breaking Changes

- **All commit SHAs changed** - Anyone with clones needs to re-clone

- **History rewritten** - This is a destructive operation

- **Force push required** - Overwrites remote history

  

#### ✅ GitHub Pages Unaffected

- GitHub builds Jekyll sites from source files

- Does NOT use local `_site` directory

- Site continues working exactly as before

- Build process unchanged

  

#### 🔄 Future Benefits

- Cleaner commits (only source files tracked)

- Faster git operations

- No merge conflicts from generated content

- Smaller repository size

  

### Alternative Approaches Considered

  

1. **BFG Repo-Cleaner** - Alternative tool, similar results

2. **Fresh start** - Create new repository, safer but loses history

3. **Manual cleanup** - Too time-consuming for large histories

  

### Verification Commands

  

```bash

# Check final size

du -sh .git

  

# Verify _site not in history

git log --name-only | grep _site

# Should return nothing

  

# Check current status

git status

# _site/ should be untracked (ignored)

```

  

### Recovery Instructions (If Needed)

  

If issues arise, the original repository state exists on GitHub until the force push. To recover:

  

1. Clone a fresh copy from GitHub (before force push)

2. Or restore from local backup if created

3. The force push overwrote the remote, so recovery requires backup

  

---

  

**Date:** 2025-07-22

**Tool Used:** git filter-repo

**Repository:** jwt625.github.io

**Status:** ✅ Successfully completed