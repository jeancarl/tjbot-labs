# Conversational Robot

![](assets/tjbot.png)

## Requirements

In this lab, we'll use the listen, converse, and speak nodes to train TJBot to listen to utterances, understand the natural language intents and entities, and respond by speaking out the response.

You will need a microphone and speaker connected to the TJBot for this lab. 

## Train TJBot to Listen, Converse, and Speak

1. In the Node-RED editor running on the Raspberry Pi, drag two ![](assets/nodes/inject.png) nodes onto the canvas. Double click on each node and configure as shown below.

    ![](assets/1.1.png)
    ![](assets/1.2.png)    
 
2. Add a ![](assets/nodes/change.png) node as shown below. This node will take the payload from the inject nodes and set the `msg.mode` property, which the listen node in the next step will use.

    ![](assets/1.3.png)
3. Add a ![](assets/nodes/listen.png) node as shown below. The listen node has several modes, start and stop, that can be configured programmatically using the `msg.mode` property to start and stop listening. When listening is enabled, the listen node produces messages as TJBot hears and transcribes words, with the text being passed in the `msg.payload` property. 

    The listen node uses the Watson Speech to Text service, which requires service credentials from IBM Cloud. Click on the pencil icon to the right of the **Bot** dropdown menu.

    ![](assets/1.4.png)

4. Click on the link icon next to the **Speech to Text** heading to launch into the IBM Cloud console and create a Watson Speech to Text service instance.

    ![](assets/1.5.png)

5. If you don't have an IBM Cloud account, sign up at [https://bluemix.net](https://bluemix.net). Sign into your account if prompted. Leave the service name as is and click **Create**.

    ![](assets/1.6.png)

6. Click on **Service Credentials** in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.7.png)
    ![](assets/1.8.png)    

7. Copy the username and password into the fields back in the Node-RED editor under the **Speech to Text** section.

    ![](assets/1.9.png)
    ![](assets/1.10.png)    

8. At the top of the configuration window, select **US English (US dialect)** from the **Listen** dropdown menu. Enable the microphone by ticking the checkbox labeled **Microphone**.

    ![](assets/1.11.png)
    
9. Add a change node to stop listening.

10. Next, we'll use the Watson Conversation service to analyze the text and respond, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog and create a **Watson Conversation** service.

    ![](assets/1.12.png)

11.	Leave the service name as is and click **Create**.

    ![](assets/1.13.png)

12. Click on the green button labeled **Launch Tool** to launch into the Conversation training tool.

    ![](assets/1.14.png)

13. We'll use a pretrained workspace. Download the file [workspace.json](../workspace.json) to your computer. Click on the up arrow to the right of the **Create** button.

    ![](assets/1.15.png)

14. Click **Choose a file** and select the `workspace.json` file you downloaded. Click **Import** to create the new workspace.

    ![](assets/1.16.png)    

15. The workspace has been trained with Intents, Entities, and a Dialog of responses that provide a simple conversation that the user can ask TJBot. Explore the tabs and see how an example chatbot is designed in the Watson Conversation service.

    ![](assets/1.17.png)    

    In order for TJBot to use the Watson Conversation service, we first need to get the service credentials and workspace ID. Click on the deploy button in the left sidebar.

    ![](assets/1.18.png)    

16. Click on the **Credentials** tab. 
    ![](assets/1.19.png)
    
17. Copy the username, password and workspace ID values into the fields under the **Conversation** heading in the Node-RED editor.

    ![](assets/1.20.png)
    ![](assets/1.21.png)    

18. Add a converse node.

    ![](assets/1.22.png)

    Here's an example of what the `response` object looks like.

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

19. Add a change node.


20. Finally, we'll use the Watson Text to Speech service to speak out the response, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog and create a **Watson Text to Speech** service.

    ![](assets/1.23.png)    

21. Leave the service name as is and click **Create**.

    ![](assets/1.24.png)

22. Click on **Service Credentials** in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.25.png)	    
    ![](assets/1.26.png)	        

23. Replace the placeholder `/* Step #22 */` with the following code. Use your own username and password credentials from the previous step.

    ```
        text_to_speech: {
          username: "dec28251-d359-4f88-a714-9f36694c4218",
          password: "5ZwSwrciqoHG"
        }
    ```

    ![](assets/1.27.png)

24. In order for TJBot to play audio, we need to configure it with a speaker. Add `"speaker"` to the array of the first argument to the TJBot constructor. If you're using a physical TJBot, please refer to the section below titled **Running on the Raspberry Pi** for more information about the speaker device ID.

    ```
    var tj = new TJBot(
      ["microphone","speaker"],
    ```

25. Replace the placeholder `/* Step #24 */` with the following code. Configure TJBot with the gender of the voice (`male` or `female`) and what language to use (`en-US` is for the US English dialect).

    ```
        robot: {
          gender: "male"
        },
        speak: {
          language: "en-US"
        }
    ```

26. Replace the placeholder `/* Step #25 */` with the following code. This code will combine the response from the Watson Conversation service and use the Watson Text to Speech service to speak out the response. When the text has been spoken out, TJBot will resume listening for another utterance, and repeat the cycle.

    ```
        tj.speak(response.object.output.text.join(" ")).then(() => {
          tj.listen(processText);
        });
    ```

27. Connect the nodes together as shown below.

    ![](assets/1.26.png) 
   
   
    
28. Click on the ![](assets/nodes/deploy.png) button in the top-right corner of the Node-RED editor to save and deploy the changes.

29. Click on the tab to the left of the inject node labeled **Start Listening** to activate the microphone. Speak into the microphone and wait for the LED to turn the color that represents the emotion that's most prevalent. Click on the tab to the left of the inject node labeled **Stop Listening** to deactivate the microphone.

## Notes

This lab uses the TJBot Node.js library which uses an older version of the Tone Analyzer. As of September 25th 2017 (service version: 3.4.1, interface version: 2017-09-21), the Tone Analyzer service no longer returns the emotion disgust. 