What Gets Collected
===================

ChatLab automatically logs all key elements of each conversation.

Data Types
-----------

**Conversation metadata**
   - Conversation ID
   - Bot name and model used
   - Participant identifiers (from Qualtrics, REDCap, Prolific, or MTurk)
   - Start and end timestamps

**Utterances**
   - Message text
   - Speaker (bot or participant)
   - Sent and received timestamps
   - Token usage and latency

**Prompts and Instructions**
   - System and persona prompts used during the conversation
   - Chat history included in each model call

**Participant metadata**
   - Response IDs from survey platforms
   - Recruitment IDs from crowdsourcing platforms
   - Optional demographic or session data

**Behavioral telemetry**
   - Typing delay, keystroke count, and page dwell time (if enabled)
