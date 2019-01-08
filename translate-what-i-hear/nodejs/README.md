# Translate What I Hear

![](assets/tjbot.png)

## Requirements

In this lab, you'll use the listen, translate, and speak methods to train TJBot to listen to utterances, translate to French, and speak out loud the translation.

You can run this lab on a physical TJBot or use the [TJBot simulator](https://ibm.biz/meet-tjbot).

If you run this lab on a physical TJBot, you will need to connect a microphone and speaker to the TJBot for this lab. 

## IBM Cloud account

You will need a IBM Cloud account to create the IBM Watson services used in this lab. See [ibm.biz/start-tjbot-lab](https://ibm.biz/start-tjbot-lab) to register for and log into IBM Cloud.

## Train TJBot to Listen and Translate

1. Create a file named app.js. Copy the following code. In the following steps, replace the `/* Step # */` placeholders with the code provided. 

    ```
    var TJBot = require("tjbot");
    
    var tj = new TJBot(
      [/* Step #2 *//* Step #20 */],
      {
        /* Step #18 */
      },
      {
        /* Step #8 */
        /* Step #13 */
        /* Step #19 */
      }
    );
    
    /* Step #9 */
    ```
    
2. For TJBot to listen and transcribe audio, you first need to configure it with a microphone. The first argument to the TJBot constructor is an array of hardware available. Add `"microphone"` to this array.

    ```
    var tj = new TJBot(
      ["microphone"],
    ```

3. TJBot uses the Watson Speech to Text service from IBM Cloud to transcribe the audio. Sign into your IBM Cloud account.

4. Click the **Catalog** link in the top menu of the IBM Cloud dashboard.

    ![](assets/1.1.png)

5. Click the **AI** category on the left. Click the **Speech to Text** tile.

    ![](assets/1.2.png)

6. Leave the service name as is. Click **Create**.

    ![](assets/1.3.png)

7. Click **Show Credentials**.

    ![](assets/1.4.png)	    

8. Replace the placeholder `/* Step #8 */` with the following code. Use your own API key from the previous step. 

    ```
        speech_to_text: { 
          apikey: "T7XjA4ZpXVb8Zw4YZ9w6c7xpvqgSa5zmnpY-1g0u1Y7f"
        }, 
    ```

    ![](assets/1.5.png)	

9. Replace the placeholder `/* Step #9 */` with the following code. This code instructs TJBot to start transcribing what is heard and this function is called after the first chunk of audio is transcribed.
    
    ```    
    tj.listen(text => {
      tj.stopListening();
      /* Step #14 */
    });
    ```

10. Next, you will train TJBot to translate the text using the Watson Language Translator service, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog. Click the **AI** category. Click the **Language Translator** tile.

    ![](assets/1.6.png)

11. Leave the service name as is. Click **Create**.

    ![](assets/1.7.png)

12. Click **Show Credentials**.

    ![](assets/1.8.png)	

13. Replace the placeholder `/* Step #13 */` with the following code. Use your own API key from the previous step. 

    ```
      language_translator: {
        "apikey": "C0ykGIGuzXTjDsb9muJXUK47O4jH5osQ1bDDSr3JBJlf"
      },
    ```

    ![](assets/1.9.png)   

14. Replace the placeholder `/* Step #14 */` with the following code. This code translates the text from English (en) to French (fr).

    ```
      tj.translate(text, "en", "fr")
        .then((response) => {
          /* Step #21 */
        });
    ```     

15. Next, you will train TJBot to speak this translation by using the Watson Text to Speech service, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog. Click the **AI** category. Click the **Text to Speech** tile.

    ![](assets/1.10.png)
    
16. Leave the service name as is. Click **Create**.

    ![](assets/1.11.png)

17. Click **Show Credentials**.

    ![](assets/1.12.png)	

18. Replace the placeholder `/* Step #18 */` with the following code. Use your own API key from the previous step. 

    ```
    text_to_speech: {
      apikey: "q5wt6VlBi5IuljwK-vbI8sCwkQrHCaAYNXeftnwCJilc"
    }
    ```

    ![](assets/1.13.png)   

19. Replace the placeholder `/* Step #19 */` with the following code. Configure TJBot with the gender of the voice (`male` or `female`) and what language to use (`fr-FR` is the language code for French). In this example, you'll use the French feminine voice model to synthesize the translation.

    ```
        robot: {
          gender: "female"
        },
        speak: {
          language: "fr-FR"
        }
    ```     

20. In order for TJBot to play audio, you need to configure it with a speaker.  Add `"speaker"` to the array of the first argument to the TJBot constructor. If you're using a physical TJBot, see the section below "Running on the Raspberry Pi" for more information about the speaker device ID.

    ```
    var tj = new TJBot(
      ["microphone","speaker"], 
    ```
    
21. Replace the placeholder `/* Step #21 */` with the following code. This will instruct TJBot to speak out loud the translation returned from the Watson Language Translator service.

    ```
          tj.speak(response.translations[0].translation);
    ```

22. Run the code. Speak a phrase, such as `Hello`. TJBot will transcribe the utterance captured via the microphone by using the Watson Speech to Text service, translate the phrase by using the Watson Language Translator service, and then speak out the translation by using the Watson Text to Speech service through the speaker.

    An example translation is:

    `Bonjour`

## Running on the Raspberry Pi

Depending on the speaker you use, you might need to specify the speaker device ID. Determine the Speaker Device ID by running the command `aplay -l` on the Raspberry Pi. In the following example output, the USB speaker attached is accessible on card `2`, device `0`.

![](assets/1.14.png)

In the TJBot configuration, use the applicable speaker device ID, with the format `plughw:<card>,<device>`

```
    speak: {
      language: "fr-FR",
      speakerDeviceId: "plughw:2,0" 
    }
```

## Complete Code

See the following complete Node.js program. The Watson service credentials are not shown. 

```
var TJBot = require("tjbot");

var tj = new TJBot(
  ["microphone","speaker"],
  {
    robot: {
      gender: "female"
    },
    speak: {
      language: "fr-FR"
    }
  },
  {
    speech_to_text: {
      apikey: ""
    },
    language_translator: {
      apikey: ""
    },
    text_to_speech: {
      apikey: ""
    }    
  }
);

tj.listen(text => {
  tj.stopListening();
  tj.translate(text, "en", "fr")
    .then((response) => {
      tj.speak(response.translations[0].translation);
    });
});
```
