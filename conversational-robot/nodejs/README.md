# Conversational Robot

![](assets/tjbot.png)

## Requirements

In this lab, you'll use the listen, converse, and speak methods to train TJBot to listen to utterances, understand the natural language intents and entities, and respond by speaking out loud the response.

You can run this lab on a physical TJBot or use the [TJBot simulator](https://ibm.biz/meet-tjbot).

If you run this lab on a physical TJBot, you will need to connect a microphone and speaker to the TJBot for this lab. There is also an extra step to configure the speaker in the section titled **Running on the Raspberry Pi** at the end of this lab.

## Train TJBot to Listen, Converse, and Speak

1. Create a file named `app.js`. Copy the following code. In the following steps, replace the `/* Step # */` placeholders with the code provided.

    ```
    var TJBot = require("tjbot");
    
    var tj = new TJBot(
      [/* Step #2 *//* Step #25 */],
      {
        /* Step #26 */
      },
      {
        /* Step #8 */
        /* Step #18 */
        /* Step #24 */
      }
    );

    /* Step #19 */

    /* Step #9 */
    ```

2. For TJBot to listen and transcribe audio, we first need to configure it with a microphone. The first argument to the TJBot constructor is an array of hardware available. Add `"microphone"` to this array.

    ```
    var tj = new TJBot(
      ["microphone"/* Step #25 */],
    ```

3. TJBot uses the Watson Speech to Text service from IBM Cloud to transcribe the audio. Sign into your IBM Cloud account.

4. Click the **Catalog** link in the top menu of the IBM Cloud dashboard.

    ![](assets/1.1.png)

5. Click the **AI** category on the left. Click the **Speech to Text** tile.

    ![](assets/1.2.png)

6. Leave the service name as is. Click **Create**.

    ![](assets/1.3.png)

7. In the **Manage** tab, click **Show Credentials**.

    ![](assets/1.4.png)       

8. Replace the placeholder `/* Step #8 */` with the following code, using the username and password credentials from the previous step:

    ```
        speech_to_text: {
          apikey: "T7XjA4ZpXVb8Zw4YZ9w6c7xpvqgSa5zmnpY-1g0u1Y7f"
        },
    ```

    ![](assets/1.5.png)    

9. Replace the placeholder `/* Step #9 */` with the following code:

    ```    
    function processText(text) {
      console.log(text);
      tj.stopListening();      

      /* Step #20 */
    }

    tj.listen(processText);    
    ```

    This will instruct TJBot to start transcribing what is heard, calling the processText function after each chunk of audio is transcribed. We also call the method `stopListening` to stop any further audio capturing.

10. Next, we'll use the Watson Assistant (formerly Watson Conversation) service to analyze the text and respond, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog and create a **Watson Assistant (formerly Conversation)** service.

    ![](assets/1.6.png)

11.	Leave the service name as is. Click **Create**.

    ![](assets/1.7.png)

12. Click **Launch Tool** to launch into the Watson Assistant training tool.

    ![](assets/1.8.png)

13. Click **Assistants**. Click **Create new**.

    ![](assets/1.9.png)

14. Give the assistant a name `Chat With TJ`. Click **Create**.

    ![](assets/1.10.png)    

15. Click **Add Dialog Skill**.

    ![](assets/1.11.png)

16. Click **Import Skill**. We'll use a pretrained skill. Download the file [workspace.json](../workspace.json) to your computer. Click **Choose JSON file** and select this file. Click **Import**.

    ![](assets/1.12.png)
    
17. Click the vertical elipse. Select **View API Details**. 

    ![](assets/1.13.png)

18. Copy the API Key (password) and workspace ID values into the fields under the **Watson Assistant** heading in the Node-RED editor.

    ![](assets/1.14.png)
    

    Replace the placeholder `/* Step #18 */` with the following code, using the **API Key** from the previous step:


    ```
        assistant: {
          apikey: "DV9sJ-1GZ_sW8oGEOEd0tZT46peLo9QZc_dE9VbHgCOI"
        },
    ```

19. Replace the placeholder `/* Step #19 */` with the following code, using the Workspace ID credential:

    ```
    var workspaceId = "c9641780-a360-4e1a-a74c-c9ee3d697f7e";
    ```

20. Replace the placeholder `/* Step #20 */` with the following code:

    ```  
      tj.converse(workspaceId, text, response => {
        console.log(response);

        /* Step #27 */
      });
    ```
    
    The transcribed text from the Watson Speech to Text service is sent to the Watson Assistant service to understand and be analyzed. Here's an example of what the `response` object looks like:

    ```
    {
      "object": {
        "intents": [
          {
            "intent": "what_am_i",
            "confidence": 0.8959802150726319
          }
        ],
        "entities": [],
        "input": {
          "text": "who are you?"
        },
        "output": {
          "text": [
            "I'm T J Bot. I'm an open-source project at IBM that shows you how to enable beautiful objects like me with cognitive capabilities using IBM Watson."
          ],
          "nodes_visited": [
            "node_2_1520404296380"
          ],
          "log_messages": []
        },
        "context": {
          "conversation_id": "7cca5aa8-6906-480c-846b-0c40c35e79a8",
          "system": {
            "dialog_stack": [
              {
                "dialog_node": "root"
              }
            ],
            "dialog_turn_counter": 1,
            "dialog_request_counter": 1,
            "_node_output_map": {
              "node_2_1520404296380": [
                0
              ]
            },
            "branch_exited": true,
            "branch_exited_reason": "completed"
          }
        }
      },
      "description": "I'm T J Bot. I'm a open-source project at IBM that shows you how to enable beautiful objects like me with cognitive capabilities using IBM Watson."
    }
    ```    

21. Return to the IBM Cloud dashboard catalog and create a **Text to Speech** service, which you will use to speak out the response.

    ![](assets/1.16.png)    

22. Leave the service name as is. Click **Create**.

    ![](assets/1.17.png)

23. In the **Manage** tab, click **Show Credentials**.

    ![](assets/1.18.png)    

24. Replace the placeholder `/* Step #24 */` with the following code, using the username and password credentials from the previous step:

    ```
        text_to_speech: {
          apikey: "q5wt6VlBi5IuljwK-vbI8sCwkQrHCaAYNXeftnwCJilc"
        }
    ```

    ![](assets/1.19.png)

25. To configure TJBot with a speaker to play audio, add `"speaker"` to the array of the first argument to the TJBot constructor. If you're using a physical TJBot, refer to the "[Running on the Raspberry Pi](link)" section for more information about the speaker device ID.

    ```
    var tj = new TJBot(
      ["microphone","speaker"],
    ```

26. Replace the placeholder `/* Step #26 */` with the following code to configure TJBot with the gender of the voice (`male` or `female`) and what language to use (`en-US` is for the US English dialect).

    ```
        robot: {
          gender: "male"
        },
        speak: {
          language: "en-US"
        }
    ```

27. Replace the placeholder `/* Step #27 */` with the following code, which combines the response from the Watson Assistant service and uses the Watson Text to Speech service to speak out the response:

    ```
        tj.speak(response.object.output.text.join(" ")).then(() => {
          tj.listen(processText);
        });
    ```
    
    When the text has been spoken out, TJBot will resume listening for another utterance, and repeat the cycle.

28. Run the code. Speak a phrase. TJBot will transcribe the audio with the Watson Speech to Text service, analyze the utterance with the Watson Assistant service, and speak out the response using the Watson Text to Speech service.    

## Running on the Raspberry Pi

Depending on the speaker you use, you may need to specify the speaker device ID. Determine the Speaker Device ID by running the command `aplay -l` on the Raspberry Pi. In the example output shown below, the USB speaker attached is accessible on card `2`, device `0`.

![](assets/1.20.png)

In the TJBot configuration, use the applicable speaker device ID, with the format `plughw:<card>,<device>`.

```
    speak: {
      language: "en-US",
      speakerDeviceId: "plughw:2,0"
    }
```

## Completed Code

The completed Node.js program is shown below.

```
var TJBot = require("tjbot");

var tj = new TJBot(
  ["microphone","speaker"],
  {
    robot: {
      gender: "male"
    },
    speak: {
      language: "en-US"
    }
  },
  {
    speech_to_text: {
      apikey: ""
    },
    assistant: {
      apikey: ""
    },
    text_to_speech: {
      apikey: ""
    }    
  }
);

var workspaceId = "";

function processText(text) {
  console.log(text);
  tj.stopListening();

  tj.converse(workspaceId, text, response => {
    console.log(response);

    tj.speak(response.object.output.text.join(" ")).then(() => {
      tj.listen(processText);
    });
  });
}

tj.listen(processText);
```
