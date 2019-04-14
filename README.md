
# Google Assistant for Choregraphe

<img align="right" width="200px" src="https://ozrobotics.com/wp-content/uploads/2018/09/Zora-Robot-1.jpg">

This repository contains the source  of a [Choregraphe](https://community.ald.softbankrobotics.com/en/resources/faq/developer/what-choregraphe) project that allows robots by [Softbank Robotics](https://www.softbankrobotics.com/) to behave like a Google Assistant, refereed as *GA* from now, it responds to normal voice command/questions (eg. "Who is Obama?", "What time is it?", "Turn off the light", etc...) like a normal Google Home would do. 

The script provides visual feedback to the user, eyes change colors according to the robot states: red indicates an error, blue indicates that it's listening, white indicate when it's idle.
This was tested on a real robot, named [Zora](http://www.zorarobotics.be/index.php/en/zorabot-zora), that is based on the Nao robot by Softbank.

This Choregraphe script could be plugged in any other existing projects like a plug-in, it uses only default Python functions, the two only limit is that the robot should have [ALSA](https://en.wikipedia.org/wiki/Advanced_Linux_Sound_Architecture) installed, note that in Zora it's already installed, and a server should be present for communicate with GA (see *How it works* section).

## Download
The last version of the project can be found in the [Releases tab](https://github.com/conema/Choregraphe-GA/releases), where you can download *choregraphe-ga.crg*

## How to use it
<img align="right" width="400px" src="https://user-images.githubusercontent.com/12801153/56098651-b1425b00-5f02-11e9-9e9a-d8ae378f2534.png">

To use this script you'll need to open the downloaded project with Choregraphe, connect it to a real robot and open the "GA" box where you should search and change **TCP_IP** and **TCP_PORT** with your server IP and port. Press the play button, say "Hey Zora" and start speaking, if all is going normally the robot should answer without problem.

## How it works
For a very general approach, the **script works as a client**, it sends microphone data to a server and waits for a JSON that contains the GA answer. The server could be installed into the robot (not tested) or in another pc, in any cases an internet connections is needed. It can be schematized as follows:

 1. The **robot** hears "Hey, Zora" (that triggers the speech recognition box)
 2. **GA box** starts recording microphone's audio with *arecord* and every bits it's sent to the server trough a TCP stream
 3. The **server** start a connection with GA and it forwards microphone's audio sent by the robot
 4. **GA** sends the response packets to the server 
 5. When GA sends an "END_OF_UTTERANCE", the **server** send a JSON to the client that contains: the transcription of the user input, the text response, the microphone mode and the conversation state (see the [documentation](https://developers.google.com/assistant/sdk/reference/rpc/google.assistant.embedded.v1alpha2) for more information) 
 6. The **robot** receive the JSON response and it read the text response that is into it.

**Note:** an implementation in NodeJS of this server can be found in the [GA-Server](https://github.com/conema/GA-Server) repository.

## Demonstration videos
[Play chess with Zora](https://www.youtube.com/watch?v=OePbvELNJ54) (Google Assistant + custom Google Actions)

[Zora controllig light](https://www.youtube.com/watch?v=rrvY-f9Vz58)  (Google Assistant + smart socket)
