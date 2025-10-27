Deployment Guide (AWS)
======================

ChatLab is designed to be deployed on AWS using Docker containers. This setup
creates two services:

- A **frontend webview** for embedding into surveys  
- A **backend API** for managing bots, participants, and data collection  

AWS Setup
---------

1. Launch an EC2 instance (Ubuntu 22.04 LTS recommended).  
2. Install Docker and Docker Compose:  
   .. code-block:: bash

      sudo apt update && sudo apt install docker.io docker-compose -y

3. Clone your ChatLab repository and create a `.env` file with environment variables.

   .. code-block:: bash

      DJANGO_SECRET_KEY=your_secret
      OPENAI_API_KEY=sk-...
      ANTHROPIC_API_KEY=sk-ant-...
      MYSQL_PASSWORD=your_db_password

4. Start ChatLab:

   .. code-block:: bash

      make start

Access Points
-------------

- Frontend (chat webview): `https://<your-domain>:3000`
- Admin panel / API: `https://<your-domain>:8000/api/admin`

Security
--------

Use HTTPS via AWS Load Balancer or Nginx reverse proxy.
Restrict admin access to authorized users only.
