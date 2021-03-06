#**Wit.aiUnityLightsAndMove**

It's a very very simple game with where you can move a cube on a plane or change the ambient light with your voice.

The game is binded with a Wit.ai App: Wit.ai is an online service that takes a natural language sentence, ie. 'I have a meeting tomorrow', and sends back data that can be easily interpreted by software, ie. 'intent: appointment, datetime: 2014-03-02T00:00:00.000+01:00'.

The scripts send sound data from a ADPCM file to the Wit.ai server by an HTTP request.
The Data from the server are formatted in JSON format, so the scripts convert them in the respective classes.

Then scripts realize what you said.

#What can you say??

The game is made to respond at 3 sentences:
- turn the light on
- turn the light off
- move forward

but you can change the speech file as you wish.
i.e. you can say "light on/off" and the behavior is the same. (this is the power of Wit.ai) 

#**What can you change??**

If you want to use this project as a template you must know some little things.

###1. Main Folder
Here you must put the audio file with your command. That file must be in PCM or ADPCM (better) format.
    
###2. /Assets/Script/PluginController.cs
This script control the processing of Speech files in response to user keyDown 			input. (i.e. if you press P, the script process "lightsoff" tu turn off the light)
    
In this calss you also have to configure the Wit.ai access Token with your app code,	the processing class need this code to comunicate with your application on Wit.ai 		servers.

###3. /Assets/Script/NLP
This folder is the core of the Game, it contains all the real class that implement		the command behavior.
    
To implement and realize a Wit.ai command you need 3 elements:
    
O_NLP classes: this file contains all the info to convert the JSON data packet to real C# object.
In particual you must add your custom Entities in the Entities class.
To know how you must format your C# classes just see the JSON data packet (Preview API Output) in che Wit.Ai Enitites Console. 
    
    i.e. 
    
	**[JSON]**    

     {
      "msg_id": "0769e3b6-8bf5-4e49-bb0c-c487d662adee",
      "_text": "turn lights on",
      "outcomes": [
        {
          "_text": "turn lights on",
          "intent": "lights",
          "entities": {
            "on_off": [
              {
                "value": "on"
              }
            ]
          },
          "confidence": 0.999
        }
      ]
    }
    
	**[C# classes]**  
    
      	public class _Entities
        {
            //....
            
            [JsonConverter(typeof(JsonToArrayConverter<_On_Off))]
            public _On_Off[] on_off { get; set; }
            
            //...
        }
        
        public class _On_Off
        {
        	public string value { get; set; }
        }

Intent Classes: Classes with the SAME name of the Wit.ai App Intent, this class is istanziated during the process, it opens the o_NLP object created by the parsing of the JSON data packet and realizes the command. It must offer the method "DoSomethings".
In Unity there is a little problem: only MonoBehavior Class can interact with the game object, but you can't istanziate an object of a MonoBehaviour Class. So we needs an other class to interact with the game world. (i.e. lights or move)
    
Execution Classes: Each Intent Class as an Execution Class with static methods that act on the game world to realize the user commands. (i.e. lights and LightsActuator)
   
###4. /Assets/Script/Processing
This folder contains the algorithm classes that control the comunication with Wit.ai	servers

###5. /Assets/Script/HTTP
This folder contains the script that the realize the POST HTTP Request from the client to the Wit.ai servers



