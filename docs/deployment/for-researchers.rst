For Researchers: How to Deploy ChatLab
======================================

Overview
--------

ChatLab can be deployed to the cloud automatically — no coding or server setup required.
All you need is an **AWS account** and a **GitHub repository** containing ChatLab.
Everything else happens for you.

When you click "Run workflow" in GitHub, ChatLab uses a tool called **Terraform**
to build your entire cloud environment automatically. This includes:

- A secure database (RDS)
- A chatbot backend (Elastic Beanstalk)
- A web frontend (S3)
- A fast global content delivery network (CloudFront)
- A public HTTPS domain (Route 53 + SSL certificate)

You'll get your own ChatLab website at a URL like:

``https://chatlab.yourdomain.org``

No terminal commands or AWS Console steps needed.

---

Before You Begin
----------------

You'll need:

1. **An AWS Account**

   Create one at `https://aws.amazon.com` using your university or lab email.
   Add a payment method (AWS Free Tier covers small studies).

2. **IAM Credentials**

   These are secure "keys" that let GitHub deploy to your AWS account.

   - Log into AWS.
   - Go to **IAM → Users → Create User**.
   - Check **Programmatic access**.
   - Grant **AdministratorAccess** (you can restrict this later).
   - Download:
     - Access key ID
     - Secret access key

3. **A Domain Name**

   You'll need a domain to host your ChatLab site (e.g., ``mychatstudy.org``).
   You can:
   - Buy one in **Route 53** (recommended), or
   - Transfer an existing one to AWS.

---

Setting Up GitHub
-----------------

1. Fork the ChatLab repository on GitHub.  
   (Click **Fork** in the top-right of the ChatLab repo.)

2. In your fork, go to **Settings → Secrets and Variables → Actions**.

3. Add the following secrets:

   - ``AWS_ACCESS_KEY_ID``  
   - ``AWS_SECRET_ACCESS_KEY``

   These are the keys you downloaded from AWS.  
   They are encrypted and only visible to GitHub Actions.

---

Deploying ChatLab
-----------------

1. Go to the **Actions** tab in your ChatLab repository.
2. Find **“Deploy ChatLab to AWS (One-Click)”**.
3. Click **Run workflow**.
4. Fill in:
   - AWS region: ``us-east-1`` (default)
   - Domain: your base domain (e.g., ``mychatstudy.org``)
   - Subdomain: what you want for ChatLab (e.g., ``chatlab``)
   - Database username and password (choose any)
5. Click **Run**.

That's it!  
GitHub will automatically:

- Create your AWS infrastructure  
- Build your frontend website  
- Upload everything to AWS  
- Generate an HTTPS certificate  
- Print your site URL (e.g., ``https://chatlab.mychatstudy.org``)

Deployment takes ~15 minutes.

---

After Deployment
----------------

When the workflow finishes, you'll see something like this:

::

   ✅ Deployed ChatLab
   🌐 Site:  https://chatlab.mychatstudy.org
   🧠 API:   https://chatlab.mychatstudy.org/api
   🗄️  DB:    chatlab-db.xxxxx.us-east-1.rds.amazonaws.com
   🚀 EB:     chatlab-env.eba-xxxxx.us-east-1.elasticbeanstalk.com

You now have a fully functional ChatLab instance!

You can embed it in Qualtrics or REDCap surveys using the instructions in
:doc:`/survey-integration/index`.

---

Removing ChatLab
----------------

If you ever want to remove everything and stop AWS charges:

1. Duplicate the deployment workflow.
2. Replace this line:

   ``terraform apply -auto-approve``

   with:

   ``terraform destroy -auto-approve``

3. Run that workflow once — Terraform will delete all AWS resources safely.

---

Troubleshooting
----------------

- **Error: Missing credentials** → double-check your AWS secrets in GitHub.
- **Domain not resolving** → make sure your domain is in AWS Route 53.
- **Slow first load** → CloudFront may take a few minutes to propagate globally.

---

FAQ
---

**Q: Do I need to install Terraform or Docker?**  
No — GitHub runs Terraform automatically in the cloud. You don't install anything.

**Q: Do I pay Terraform?**  
No — Terraform is free and open source. You only pay AWS for what's used.

**Q: Can multiple people use the same AWS account?**  
Yes. You can create separate IAM users for collaborators and give them limited access.

**Q: What happens if I delete my GitHub repo?**  
Your AWS resources will remain running. To delete them, run the "destroy" workflow first.

---

Summary
-------

- 🧩 **You own everything** — it's deployed to your AWS account.  
- 🧠 **You control it** — GitHub handles the deployment.  
- 🚀 **You don't need DevOps** — just click "Run workflow.""
- 🔐 **Secure** — HTTPS by default, credentials stay private.

This setup lets researchers deploy ChatLab in minutes — no AWS console, no manual setup, and no coding required.
