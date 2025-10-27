MTurk Integration
=================

Overview
--------

ChatLab can also be deployed via MTurk by linking an embedded Qualtrics or
REDCap survey as the HIT URL.

Setup
-----

1. Create a new HIT in your MTurk requester account.
2. Use your survey URL (with embedded ChatLab iframe) as the task link.
3. Add URL parameters to track worker IDs and conditions:

   ``?workerId=${workerid}&assignmentId=${assignmentid}``

Tracking and Completion
-----------------------

Worker IDs and task completion status are logged automatically in ChatLab's
database alongside conversation data.
