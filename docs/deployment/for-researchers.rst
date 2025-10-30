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

   These are secure “keys” that let GitHub deploy to your AWS account.

   Step-by-step in AWS:

   1. Sign in to the AWS Console.
   2. Go to **IAM** (search “IAM” in the top search bar).
   3. In the sidebar, click **Users** → **Create user**.
   4. Name your user something like ``chatlab-deployer``.
   5. Under “Select AWS access type,” check **Programmatic access**.
   6. On the next screen, choose **Attach existing policies directly**.
   7. Search for and select **AdministratorAccess** (you can restrict this later).
   8. Click **Next → Create user**.
   9. Download the credentials file or copy:
      - **Access key ID**
      - **Secret access key**

3. **A Domain Name**

   You'll need a domain to host your ChatLab site (e.g., ``mychatstudy.org``).
   You can:
   - Buy one in **Route 53** (recommended), or
   - Transfer an existing one to AWS.

---

Adding Your AWS Keys to GitHub
------------------------------

Once you have your AWS access keys, store them in your GitHub repository as **encrypted secrets**.  
These are required for the "one-click deploy" workflow to connect to your AWS account.

Follow these steps carefully:

1. Go to your repository:  
   **https://github.com/wwbp/humanlike-chatbot**

2. Click the **⚙️ Settings** tab (top of the page).

3. In the sidebar, scroll down and click **Secrets and variables → Actions**.

   Direct link:  
   ``https://github.com/wwbp/humanlike-chatbot/settings/secrets/actions``

4. Click the **New repository secret** button.

5. Add your first secret:

   - **Name:** ``AWS_ACCESS_KEY_ID``  
   - **Value:** paste your Access Key ID from AWS  
   - Click **Add secret**

6. Click **New repository secret** again and add your second secret:

   - **Name:** ``AWS_SECRET_ACCESS_KEY``  
   - **Value:** paste your Secret Access Key from AWS  
   - Click **Add secret**

7. Confirm both appear in your secrets list:

   ::

      AWS_ACCESS_KEY_ID        updated a few seconds ago
      AWS_SECRET_ACCESS_KEY    updated a few seconds ago

These secrets are stored securely by GitHub and only accessible by your workflows.
They are **never visible to the public** or committed to your repository.

---

Deploying ChatLab
-----------------

1. Go to your repo's **Actions** tab.
2. Find **“Deploy ChatLab to AWS (One-Click)”**.
3. Click **Run workflow**.
4. Fill in the form:
   - AWS region: ``us-east-1`` (default)
   - Domain: your base domain (e.g., ``mychatstudy.org``)
   - Subdomain: what you want for ChatLab (e.g., ``chatlab``)
   - Database username and password (choose any)
5. Click **Run**.

GitHub will automatically:

- Connect to your AWS account using the secrets you added
- Create all AWS services (database, web app, storage, SSL)
- Build and upload your frontend
- Print your live URL (e.g., ``https://chatlab.mychatstudy.org``)

Deployment takes about 10-15 minutes.

---

After Deployment
----------------

When the workflow finishes, scroll to the end of the GitHub Action log.
You'll see something like this:

::

   ✅ Deployed ChatLab
   🌐 Site:  https://chatlab.mychatstudy.org
   🧠 API:   https://chatlab.mychatstudy.org/api
   🗄️  DB:    chatlab-db.xxxxx.us-east-1.rds.amazonaws.com
   🚀 EB:     chatlab-env.eba-xxxxx.us-east-1.elasticbeanstalk.com

Your ChatLab instance is now live and ready to embed in Qualtrics or REDCap.
See :doc:`/survey-integration/index` for details.

---

Removing ChatLab
----------------

If you want to stop paying for AWS resources or reset your environment:

1. Duplicate the deployment workflow file.
2. Replace the line:

   ``terraform apply -auto-approve``

   with:

   ``terraform destroy -auto-approve``

3. Save and run that new workflow once.
4. Terraform will automatically delete all AWS resources for you.

---

FAQ
---

**Q: Do I need to install Terraform or Docker?**  
No — GitHub handles Terraform automatically in the cloud.

**Q: Do I pay Terraform?**  
No — Terraform is free and open source. You only pay AWS for usage.

**Q: Where do my AWS credentials live?**  
They're encrypted inside your GitHub repository settings and only available to Actions.

**Q: Can multiple team members deploy ChatLab?**  
Yes. Add multiple IAM users in AWS and share the repo. Each person can reuse the same secrets.

**Q: What if I made a mistake in my secrets?**  
Go back to **Settings → Secrets and variables → Actions**, delete the old secret, and re-add it.

---

Summary
-------

- 🔐 Add AWS credentials as **GitHub Secrets**  
- ⚙️ Run the **Deploy ChatLab** workflow once  
- 🌍 Get your own HTTPS site on AWS automatically  
- 🧹 Run "destroy" workflow anytime to remove everything

This approach lets non-technical researchers deploy ChatLab securely and reproducibly with **no coding or AWS console steps required**.
