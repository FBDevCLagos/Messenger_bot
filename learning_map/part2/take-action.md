# Overview 
Now that you know more about bots, let us dive in a little deeper on how to create a simple bot.

# the following languages can be use to build a custom bot.
1. PHP
2. Javascript
3. Python
4. Ruby on Rails

For quick setup i will start with PHP, example are also available with the metion programming languages listed above.

# Create a blue prints of your bot.
# Custom Bot Template engine.

# Using Lizards blueprints
--> Products and Purpose
	--> Type of Services/Product base on QA. 
	--> Documentation of Bots details.
	--> Flexibility and User Friendly.
	--> Compactibility
	--> Guide user expectations.

--> Structure and Layout

	--> products samples
		--> images
		--> data contents
		--> functionality

	--> Custom Testing
		--> How to use ngrok to expose your local development server and setup a webhook.
		--> How to custom test your bot while on development server.
		--> Using Git and Heroku for Tracking Development Stage.
		--> Adding Documentations and Committing Changes
		--> Test Bot Functionality with Heroku.

	--> Test Functionality
		--> Initilizing App ID. (Facebook App ID)
		--> programming Bot functionality
		--> Deploy to Hosted Server
		--> Creating a webhook URL
		--> Initilize token with server
		--> Select Bot Functionality and Services
		--> Test.

# see documentation on ngrok and heroku at the official website
www.ngrok.com
www.heroku.com

# visit github for VCS 
www.github.com

# Start Development
Step 1.
For PHP developer building facebook bot. create an index file with .php extension

---------------------------index.php------------------------
<?php

# Report all errors except E_NOTICE
# This is the default value set in php.ini
error_reporting(E_ALL & ~E_NOTICE);

# App Auth Token.
$VERIFY_TOKEN = '<YOUR_VERIFY_TOKEN>';
$PAGE_ACCESS_TOKEN = '<YOUR_PAGE_ACCESS_TOKEN>';

# Initilizing Request
$challenge = $_REQUEST['hub_challenge'];
$verify_token = $_REQUEST['hub_verify_token'];
if ($verify_token === $VERIFY_TOKEN) {
  	//If the Verify token matches, return the challenge.
  	echo $challenge;
}else {
	
	// *********************************
	// Programming bot codes goes here..
	// *********************************

	print "Hello World!"; // delete after confirming "hello world"

}

?>
---------------------------end------------------------------


Step 2.
Note: Request are handle by Javascript Json Array Structure.
Below are example of how javascript Json structure query bot messages and pinging..

curl -X POST -H "Content-Type: application/json" -d '{"recipient":{"id":"<YOUR PSID>"}, "message":{"text":"Hello PSID User!"}}' "https://graph.facebook.com/v2.6/me/messages?access_token=EAAMs03NJ13wBAFJTOA0ItNR6GaP7DBkNZBJBxa2sl8gSh8faZCMzxZCWLZCgMUSf74NdKI7kVgSAHp4ZBO7vmyaME2B9Ha3K5LcwxzyLGnZBOPzZCjS20TizeGmBBiy01XbaZCIV2rUTX8wZBX0ynRKDFjNamhZAYOuqF4Gwde0NEzpgZDZD"


# replace ID with APP-ID
# run on a cmd terminal or console.
# check the the messenger page for respond.

*** JSON PAYLOAD - Un-minified ***
{
	"recipient": {
		"id": "1216138421756978."
	},
	"message": {
		"text": "Hello World!"
	}
}

Note: the above lines of codes show how Data are sent to your Bot Messenger via Json Payloads.
So we have to convert Incoming and Outgoing Json Data Request into PHP via Json Array.
This can be accomplished like this.

A simple text data 
*** PHP converted JSON PAYLOAD - Un-minified ***
	$json = {
				"recipient": {
				"id": "1216138421756978."
			},
				"message": {
				"text": "Hello World!"
			}
	};

	# then we return the data in Json Format
	return $json;

So the following function has been build to provide easy functionality.

# custom function for building json payloads
function build_text_message_payload($sender, $message){
	// Build the json payload data
	$jsonData = '{
	    "recipient":{
	        "id":"' . $sender . '"
	    }, 
	    "message":{
	        "text": "'. $message .'"
	    }

	}';
	return $jsonData;
}


step 2. 
Understanding the core structure after the Json blending with PHP function to trigger respond feedback.
The function 'send_messages' send the build messages from the Json Payload Converted Data.

function send_message($access_token, $payload) {
	// Send/Recieve API
	$url = 'https://graph.facebook.com/v2.6/me/messages?access_token=' . $access_token;
	// Initiate the curl
	$ch = curl_init($url);
	// Set the curl to POST
	curl_setopt($ch, CURLOPT_POST, 1);
	// Add the json payload
	curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);
	// Set the header type to application/json
	curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json'));
	// SSL Settings
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
	// Send the request
	$result  = curl_exec($ch);
	//Return the result
	return $result;
}


Step 3.
Now that you can convert and return your own build message. The function below show how you can return messsage send request 
back to your bot.

$input = json_decode(file_get_contents('php://input'), true);
	// Get the Senders Page Scoped ID
	$sender = $input['entry'][0]['messaging'][0]['sender']['id'];
	// Get the message text sent
	$message = $input['entry'][0]['messaging'][0]['message']['text'];

	if(!empty($message)){
		if($message == 'hello'){
			// set the response
			$response = 'hi';
			// return message response
			send_message($sender, $response, $PAGE_ACCESS_TOKEN);
		}
	}

Step 4. 
Now that you have setup a simple callback function for your bot response we can move to hosting and deployment of your app.
Deploy your app using heroku and change your webhook url to the hosted scripts on Heroku, back to your application and run a quick test by typing hello.
This allow you to recieve the response from the Bot Messenger making it avaliable online.

for more details on Templating and Custom bot rendering interface visit the facebook documentation on Bot.
https://developers.facebook.com/docs/messenger-platform/guides/quick-start

you can post your question on the comment area on our github repo or facebook developer's page we will be glad to respond