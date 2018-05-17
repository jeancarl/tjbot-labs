# TJBot Labs

The following labs showcase the capabilities of the TJBot open-source robot. Each lab shows step-by-step instructions to use a combination of Watson services and hardware to make the robot come alive.

You can choose to either use Node-RED or Node.js to run these labs on a physical TJBot, or use the browser-based TJBot simulator to run the Node.js labs. An free IBM Cloud account is required to use the IBM Watson capabilities.

## Getting Started 

### Node-RED

For instructions on setting up the Raspberry Pi, upgrading Node-RED, and installing the Node-RED nodes needed for these labs, please refer to this [Medium post](https://medium.com/@jeancarlbisson/how-to-train-your-tjbot-in-node-red-88bfb3bbe0ab).

### Node.js

These labs use the [tjbot](https://www.npmjs.com/package/tjbot) NPM library. To install the library, run the command:

```
npm install tjbot
```

Note: you may need to run the code as root so that TJBot can access the hardware.

```
sudo node app.js
```

### TJBot Simulator

These labs can also be run using the online TJBot Simulator. A web-browser (Chrome, Firefox) with access to the camera, microphone, and speaker is all that's needed. 

Access the simulator at [ibm.biz/meet-tjbot](https://ibm.biz/meet-tjbot).


## Labs 

### Conversational Robot

*Lab Resources*: [Node.js](conversational-robot/nodejs)

*Uses*: Microphone, Speaker, Watson Speech to Text, Watson Conversation, Watson Text to Speech

Train TJBot to listen to phrases, understand natural language intents and entities, and speak out responses. Uses an example Conversation workspace that explains what the TJBot is and some of the components of the project.

---

### Emotional Light

*Lab Resources*: [Node-RED](emotional-light/node-red) | [Node.js](emotional-light/nodejs)

*Uses*: Microphone, LED, Watson Speech to Text, Watson Tone Analyzer

Train TJBot to listen to phrases and analyze the emotional tone using Watson Tone Analyzer. Depending on which emotion is most prevalent in the phrase, the LED will change to represent that emotion.

---

### Say What I See

*Lab Resources*: [Node-RED](say-what-i-see/node-red) | [Node.js](say-what-i-see/nodejs)

_Uses_: Camera, Speaker, Watson Visual Recognition, Watson Text to Speech

Train TJBot to take a photo with the Raspberry Pi, classify it with Watson Visual Recognition, and speak what objects and colors are seen with Watson Text to Speech and the speaker.

---

### Translate What I Hear

*Lab Resources*: [Node.js](translate-what-i-hear/nodejs)

*Uses*: Microphone, Speaker, Watson Speech to Text, Watson Language Translator, Watson Text to Speech

Train TJBot to listen to phrases, translate to French, and speak out the translation.

---

## License

This code is licensed under Apache 2.0. Full license text is available in [LICENSE](LICENSE).
