Crowdworking Integration
========================

Overview
--------

ChatLab supports seamless deployment of surveys and chat-based experiments
through major crowdworking platforms such as **Prolific** and **Amazon
Mechanical Turk (MTurk)**. These integrations allow you to collect rich,
naturalistic conversational data from diverse online participants at scale.

Participants interact with an embedded ChatLab bot within your survey
platform (e.g., Qualtrics or REDCap). The survey is then linked to the
crowdworking platform via URL parameters, ensuring that participant metadata
such as worker IDs, session IDs, and study identifiers are automatically
captured and stored in ChatLab's backend.

Key Features
------------

- **Unified Data Capture:** All Prolific or MTurk identifiers (e.g.,
  participant ID, study ID, session ID) are automatically logged alongside
  survey and chat data.

- **Automatic Linking:** Worker metadata is tied directly to the conversation
  record, allowing precise data merges across survey exports, ChatLab logs,
  and recruitment platform records.

- **Data Resilience:** Metadata redundancy across systems (ChatLab, survey
  platform, and Prolific/MTurk) provides protection against data loss.

- **Scalable Recruitment:** Combine ChatLab's experimental control and
  conversation features with Prolific or MTurk's large, diverse participant
  pools.

Available Integrations
----------------------

The following guides describe how to configure ChatLab with specific
crowdworking platforms:

.. toctree::
   :maxdepth: 1

   prolific
   mturk

Typical Workflow
----------------

1. **Create a ChatLab-enabled survey**  
   Build your study in Qualtrics or REDCap and embed a ChatLab chatbot.

2. **Launch the survey on a crowdworking platform**  
   Use either Prolific or MTurk to recruit participants.  
   - Set the external study link to your survey URL.  
   - Ensure required identifiers (e.g., Prolific PID or MTurk Worker ID) are
     passed as URL parameters.

3. **ChatLab automatically logs participant metadata**  
   All parameters are stored in ChatLab's database, linked to each
   conversation session.

4. **Merge and analyze data**  
   Export your ChatLab and survey data to merge by common IDs for downstream
   analysis (e.g., conversation quality, engagement, or LLM behavior).

Integration Diagram
-------------------

The flow below illustrates how ChatLab connects with crowdworking platforms
and survey systems:

.. code-block:: text

   [Prolific / MTurk]
            │
            ▼
      [Survey Platform]
        (Qualtrics / REDCap)
            │
            ▼
         [ChatLab]
            │
            ▼
       [MySQL Database]
            │
            ▼
      [Export & Analysis]


