REDCap Integration
==================

Overview
--------

REDCap users can embed ChatLab's conversational interface inside a project
form using the Descriptive Text field and HTML iframe.

Embedding Steps
---------------

1. Deploy ChatLab on AWS.
2. Copy your webview URL, e.g., ``https://chatlab.yourdomain.org/conversation``.
3. In your REDCap project, create a Descriptive Text field and paste:

   .. code-block:: html

      <iframe
        src="https://chatlab.yourdomain.org/conversation?record_id=[record-id]"
        width="100%" height="600" frameborder="0"></iframe>

Parameter Linking
-----------------

- ``record_id`` is passed automatically by REDCap.
- ChatLab logs each conversation with matching participant metadata.

Data Flow
---------

REDCap record ⇄ ChatLab conversation metadata ⇄ AWS backend
