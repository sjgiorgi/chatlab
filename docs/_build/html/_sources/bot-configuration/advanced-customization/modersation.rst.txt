Content Moderation
==================

ChatLab includes built-in moderation features to detect and handle inappropriate
content during chat sessions. Moderation ensures that participants are not exposed
to harmful model outputs and that study data remain safe for analysis.

Moderation Levels
-----------------

ChatLab uses model-based content moderation APIs (e.g., OpenAI's Moderation API)
to evaluate each message.

Each message receives **category scores** (e.g., sexual, violent, hate, harassment)
and a **confidence threshold**.

Configuration
-------------

In the ChatLab admin panel or `.env` file:

.. code-block:: bash

   MODERATION_ENABLED=True
   MODERATION_THRESHOLD=0.7

Behavior
--------

- **Below threshold:** message is delivered normally  
- **Above threshold:** message is blocked or replaced with a safe fallback  
- **Flagged:** conversation entry marked in logs for review

Fallback Responses
------------------

When a response is blocked, the bot sends a neutral message such as:

> "I'm sorry, I can't discuss that topic."

Logging and Review
------------------

All flagged messages are logged with timestamps and moderation category scores.
Administrators can review them in the backend admin panel.

Ethical Considerations
----------------------

Researchers should set moderation levels consistent with IRB and ins
