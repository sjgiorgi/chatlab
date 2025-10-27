Qualtrics Integration
=====================

Overview
--------

Use ChatLab to embed conversational tasks into your Qualtrics survey pages.


Data Needed to Embed Bot
------------------------

- **Survey ID**: 
- **Bot Name**:
- **Study Name**:  

Embedding ChatLab
-----------------

1. Deploy ChatLab on AWS following the :doc:`/deployment/index` guide.
2. Obtain your webview URL (e.g., ``https://chatlab.yourdomain.org/conversation``).
3. In Qualtrics, add a **Text / Graphic** question.
4. Open the JavaScript editor. This is typically on the left under Edit Question -> Question Behavior
5. Add the following code in the editor under Edit Question JavaScript:

   .. code-block:: php

      Qualtrics.SurveyEngine.addOnload(function() {
         var studyName = "Anthropomorphism Study";  // Match expected value
         var botName = "${e://Field/model}";  
         var surveyID = "SV_cBajyfQsVTKqZRY";  //unique survey ID
         var participantID = "${e://Field/pid}";  // Unique prolific participant ID
         var conversationID = "${e://Field/ResponseID}";  // qualtrics session Id

         window.totalTimeOnPage = 0;
         window.totalTimeAwayFromPage = 0;
         window.pageStartTime = new Date();
         window.awayStartTime = null;

         // Construct chatbot URL with encoded parameters
         var botURL = "https://chatlab.yourdomain.org/conversation";
         botURL += "?bot_name=" + encodeURIComponent(botName);
         botURL += "&conversation_id=" + encodeURIComponent(conversationID);
         botURL += "&participant_id=" + encodeURIComponent(participantID);
         botURL += "&study_name=" + encodeURIComponent(studyName);
         botURL += "&user_group=" + encodeURIComponent(userGroup);
         botURL += "&survey_id=" + encodeURIComponent(surveyID);

         console.log("Generated botURL:", botURL);  // Debugging

         var container = this.getQuestionTextContainer();  // Ensure valid container
         if (container) {
            var iframe = jQuery("<iframe>", {
                  src: botURL,
                  width: "100%",
                  height: "100vh",
                  frameborder: "0"
            });
            jQuery(container).append(iframe);  // Insert iframe
         } else {
            alert("Error: No valid container found.");
         }

         function handleVisibilityChange() {
            var currentTime = new Date();
            if (document.hidden) {
                  // User switched to a different tab
                  window.awayStartTime = currentTime;
                  window.totalTimeOnPage += (currentTime - window.pageStartTime);
                  console.log("User is not looking at the page");
            } else {
                  // User returned to the tab
                  if (window.awayStartTime) {
                     window.totalTimeAwayFromPage += (currentTime - window.awayStartTime);
                     window.awayStartTime = null;
                  }
                  window.pageStartTime = currentTime;
                  console.log("User is looking at the page");
            }
         }         

         // Set initial state
         handleVisibilityChange();

         // Listen for visibility change events
         document.addEventListener("visibilitychange", handleVisibilityChange, false);

      });

      
   

Passing Data
------------

Use Qualtrics embedded data fields (e.g. ``ResponseID``)
to pass participant metadata through URL parameters. ChatLab logs these automatically.

Data Linking
------------

- Each conversation is stored with a `participant_id`
- Survey responses remain linked to conversation logs
- You can merge data via Qualtrics exports or the ChatLab database

Keystrokes
----------

   .. code-block:: php

      // Function to update time counters and send keystroke data
      function handlePageExit() {
         var currentTime = new Date();
         
         // Ensure we account for time on page before the event
         if (!document.hidden) {
            window.totalTimeOnPage += (currentTime - window.pageStartTime);
         } else if (window.awayStartTime) {
            window.totalTimeAwayFromPage += (currentTime - window.awayStartTime);
         }
         
         sendKeystrokeData();
      }

      // Attach both unload and pagehide events
      Qualtrics.SurveyEngine.addOnUnload(handlePageExit);
      window.addEventListener("pagehide", handlePageExit, false);

      // Function to send keystroke data to external API using both window and sessionStorage flags to avoid duplicate sends
      function sendKeystrokeData() {
         // Check both window and sessionStorage flags
         if (window._keystrokeDataSent || sessionStorage.getItem("keystrokeDataSent") === "true") {
            console.log("Keystroke data already sent.");
            return;
         }
         // Set both flags so that duplicate calls are ignored
         window._keystrokeDataSent = true;
         sessionStorage.setItem("keystrokeDataSent", "true");
         
         var conversationID = "${e://Field/ResponseID}"; // Embedded data from Qualtrics
         var payload = JSON.stringify({
            conversation_id: conversationID,
            total_time_on_page: window.totalTimeOnPage,
            total_time_away_from_page: window.totalTimeAwayFromPage,
            keystroke_count: window.keystrokeCount
         });
         
         if (navigator.sendBeacon) {
            navigator.sendBeacon("https://bot.wwbp.org/api/update_keystrokes/", payload);
            console.log("Keystroke data sent using sendBeacon.");
         } else {
            fetch("https://bot.wwbp.org/api/update_keystrokes/", {  
                  method: "POST",
                  headers: {
                     "Content-Type": "application/json"
                  },
                  body: payload
            })
            .then(response => response.json())
            .then(data => console.log("Keystroke data successfully sent:", data))
            .catch(error => console.error("Error sending keystroke data:", error));
         }
      }