AWS Deployment
==============

Run **ChatLab** on AWS with managed DB, Redis, S3, API behind EB, and CDN. This follows the AWS console flows for Route 53, ACM, Elastic Beanstalk (Docker), RDS, ElastiCache, S3, and CloudFront. See AWS docs for details. 

Prereqs
-------

- AWS account (IAM user with EB, RDS, ElastiCache, S3, CloudFront, Route 53, ACM)
- Domain you can edit DNS for
- API keys: ``OPENAI_API_KEY``, ``ANTHROPIC_API_KEY``

Env vars
--------

Set in **Elastic Beanstalk → Configuration → Software → Environment properties**.
While building the **React frontend, set REACT_APP_API_URL, building static files**.

- DB: ``DATABASE_URL=...``
- Cache: ``REDIS_URL=redis://<endpoint>:6379/0``
- App: keys, secrets, etc.

Check ``sample.env``

Step 1 — Domain + TLS
---------------------

.. code-block:: text

   Route 53 → Hosted zones → Create hosted zone (your-domain.com)
   → copy NS to your registrar

   ACM → Request public certificate
   - Names: your-domain.com, *.your-domain.com
   - Validation: DNS

Use this cert later in CloudFront.

Step 2 — Database (RDS)
-----------------------

- Create an RDS instance.
- Put it in the same VPC/subnets as the app.
- Note endpoint, username, password.
- In EB env vars set: ``DATABASE_URL=mysql://user:pass@host:3306/dbname``

Step 3 — Cache (ElastiCache / Redis)
-------------------------------------

- Create Redis in the same VPC.
- Copy the primary endpoint.
- In EB env vars set: ``REDIS_URL=redis://<endpoint>:6379/0``

Step 4 — App storage (S3)
-------------------------

.. code-block:: text

   S3 → Create bucket: chatlab-files-prod → EB Service Access Permissions 

Keep bucket name in EB env vars.

Step 5 — Backend (Elastic Beanstalk, Docker)
--------------------------------------------

.. code-block:: text

   Elastic Beanstalk → Create application
   → Create environment (Web server)
     - Platform: Docker
     - Source: Backend directory ZIP containing Dockerfile
     - VPC: same as RDS/Redis
     - Security group: verify inbound/outbound rules

Step 6 — Frontend (S3 + CloudFront + Route 53)
----------------------------------------------

Build locally:

.. code-block:: bash

   cd frontend
   npm install
   npm run REACT_APP_API_URL=<your-api-url> build

Deploy static files:

.. code-block:: text

   S3 → bucket: your-domain.com → upload build/

Create CDN:

.. code-block:: text

   CloudFront → origin: S3 (your-domain.com)
   - Alternate domain (CNAME): your-domain.com
   - SSL: select ACM cert from Step 1
   - Behaviors:
     /api/* → origin: EB load balancer, caching: disabled
     /ws/*  → origin: EB load balancer, caching: disabled

Point DNS:

.. code-block:: text

   Route 53 → A record (alias) → CloudFront distribution


Access
------

.. code-block:: text

   https://your-domain.com            → Web Client
   https://your-domain.com/api/admin  → API Server

