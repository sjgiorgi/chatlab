Running Locally
===============

This guide walks you through setting up and running the ChatLab project locally on your machine.

Prerequisites
------------

Before you begin, ensure you have the following installed:

* Docker Engine
* Docker Compose
* Git

Getting Started
-------------

1. Clone the Repository
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   git clone https://github.com/wwbp/humanlike-chatbot.git
   cd humanlike-chatbot

2. Environment Setup
~~~~~~~~~~~~~~~~~~

Copy the sample environment file and configure it:

.. code-block:: bash

   cp sample.env .env

Edit the `.env` file to include your API keys and configuration settings.

3. Available Services
~~~~~~~~~~~~~~~~~~~

The project consists of several components that work together (validate docker-compose.yml):

* **Chat Interface** [Web Client]:
   - Port: 3000
   - URL: http://localhost:3000
   - Purpose: Where users interact with the chatbot

* **Control Center** [API Server]:
   - Port: 8000
   - Admin URL: http://localhost:8000/api/admin/
   - Purpose: Manages chatbots, conversations, and settings

* **Data Storage** [Database]:
   - Port: 3306
   - Purpose: Stores conversations and system data long-term

* **Quick Memory** [Cache]:
   - Port: 6379
   - Purpose: Temporarily stores data for faster access

* **Documentation** [Guides]:
   - Port: 8001
   - URL: http://localhost:8001
   - Purpose: Instructions and guides for using the system

4. Running the Application
~~~~~~~~~~~~~~~~~~~~~~~~

Start all services using Docker Compose:

.. code-block:: bash

   docker compose up --build

Or use the provided Makefile command:

.. code-block:: bash

   make start

To stop the services:

.. code-block:: bash

   docker compose down

To stop and remove all data (clean slate):

.. code-block:: bash

   docker compose down -v

5. First-time Setup
~~~~~~~~~~~~~~~~~

Create an admin user to access the Django admin interface:

.. code-block:: bash

   # Access the backend container
   docker exec -it humanlike-chatbot-backend-1 bash
   
   # Create superuser
   python manage.py createsuperuser

Follow the prompts to set up your admin username and password.

6. Accessing the Services
~~~~~~~~~~~~~~~~~~~~~~~

After starting the services, you can access:

* Chat Interface: http://localhost:3000
* Admin Panel: http://localhost:8000/api/admin/
* Documentation: http://localhost:8001

7. Development Commands
~~~~~~~~~~~~~~~~~~~~

Useful commands for development:

* Run backend tests:

  .. code-block:: bash

     make test

* View logs for a specific service:

  .. code-block:: bash

     docker compose logs -f backend
     docker compose logs -f frontend

8. Common Issues
~~~~~~~~~~~~~~

* If services fail to start, ensure no other applications are using the required ports (3000, 8000, 3306, 6379, 8001)
* For database connection issues, try removing volumes and rebuilding:

  .. code-block:: bash

     docker compose down -v
     docker compose up --build

* If the frontend shows connection errors, ensure the backend service is fully started and healthy