# Emotional LED Light

![](assets/tjbot.png)

## Requirements

In this lab, you'll use the listen and analyze tone nodes to train TJBot to listen to utterances and analyze the emotion, lighting up an LED light based on which emotion is most prevalent. You will need a microphone and LED connected to the TJBot for this lab.

## Train TJBot to Listen and React to Emotions

1. In the Node-RED editor running on the Raspberry Pi, drag two ![](assets/nodes/inject.png) nodes onto the canvas. Double click each node and configure as shown below.

    ![](assets/1.1.png)
    
    ![](assets/1.2.png)
 
2. Add a ![](assets/nodes/change.png) node as shown below. This node will take the payload from the inject nodes and set the `msg.mode` property, which the listen node in the next step will use.

    ![](assets/1.3.png)
3. Add a ![](assets/nodes/listen.png) node as shown below. The listen node has several modes, start and stop, that can be configured programmatically using the `msg.mode` property to start and stop listening. When listening is enabled, the listen node produces messages as TJBot hears and transcribes words, with the text being passed in the `msg.payload` property. 

    The listen node uses the Watson Speech to Text service, which requires service credentials from IBM Cloud. Click the pencil icon to the right of the **Bot** dropdown menu.

    ![](assets/1.4.png)

4. Click the link icon next to the **Speech to Text** heading to launch into the IBM Cloud console. Sign into your IBM Cloud account if prompted. 

    ![](assets/1.5.png)

5. Leave the service name as is. Click **Create**.

    ![](assets/1.6.png)

6. Click **Show Credentials**.

    ![](assets/1.7.png)

7. Copy the API Key into the field back in the Node-RED editor under the **Speech to Text** section.

    ![](assets/1.8.png)
    ![](assets/1.9.png)    

8. At the top of the configuration window, select **US English** from the **Listen** dropdown menu. Enable the microphone by ticking the checkbox labeled **Microphone**.

    ![](assets/1.10.png)

9. Add a ![](assets/nodes/analyzetone.png) node as shown below. Select **Emotion** from the **Tones** dropdown menu.

    The analyze node uses the Watson Tone Analyzer service, which requires service credentials from IBM Cloud. Click the pencil icon to the right of the **Bot** dropdown menu. 

    ![](assets/1.11.png)

10. Click the link icon next to the **Tone Analyzer** heading to launch into the IBM Cloud console and create a Watson Tone Analyzer service instance.

    ![](assets/1.12.png)

11. Leave the service name as is. Click **Create**.

    ![](assets/1.13.png)

12. Click **Show Credentials**.

    ![](assets/1.14.png)

13. Copy the API Key into the field back in the Node-RED editor under the **Tone Analyzer** section.

    ![](assets/1.15.png)
    ![](assets/1.16.png)    
14. Watson Tone Analyzer returns scores for five emotions: anger, disgust, fear, joy, and sadness. Use a ![](assets/nodes/function.png) node to find the emotion that scores the highest.

    ![](assets/1.17.png)

15. Add a ![](assets/nodes/switch.png) node to test which emotion scored highest as shown below.

    ![](assets/1.18.png)
16. Add five shine nodes, each with a color representing one of the emotions: red (anger), green (disgust), magenta (fear), yellow (joy), and blue (sadness).

    ![](assets/1.19.png) 
    ![](assets/1.20.png) 
    ![](assets/1.21.png) 
    ![](assets/1.22.png)
    ![](assets/1.23.png)

17. The shine node uses the LED. Click the pencil icon to the right of the **Bot** dropdown menu. At the top of the configuration window, enable the LED by ticking the checkbox labeled **LED**.

    ![](assets/1.24.png) 
    
18. Connect the nodes together as shown below.

    ![](assets/1.25.png) 
    
19. Click the ![](assets/nodes/deploy.png) button in the top-right corner of the Node-RED editor to save and deploy the changes.

20. Click the button to the left of the inject node labeled **Start Listening** to activate the microphone. Speak into the microphone and wait for the LED to turn the color that represents the emotion that's most prevalent. Click the button to the left of the inject node labeled **Stop Listening** to deactivate the microphone.

## Notes

This lab uses the TJBot Node.js library which uses an older version of the Tone Analyzer. As of September 25th 2017 (service version: 3.4.1, interface version: 2017-09-21), the Tone Analyzer service no longer returns the emotion disgust. 