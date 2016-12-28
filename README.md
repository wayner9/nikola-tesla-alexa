# Nikola

Nikola is a python application for monitoring and managing
a Tesla connected automobile from an Amazon Alexa natural language device

Authors: Michael Kolowich, Andrew Payne, Wayne Kozun;
October/November/December, 2016

Requires:
* teslajson.py by Greg Glockner (on GitHub);
* flask_ask by John Wheeler (on GitHub);
* geocoder by Denis Carriere (on GitHub)

## Notes
The Python program nikola.py contains the intent handlers required to
accept an Alexa Intent, query and post to the Tesla API for a specific car,
and return a text response to be spoken on Alexa devices such as the Echo and Dot.

This is a fork of mekolowich's Alexa skill.  One of the main differences is that this
is designed to be run on a PC in your home or wherever rather than in the cloud.
We will then use a program called ngrok to open a tunnel to your PC.

I have tried this out on Windows and on a Raspberry Pi running Raspbian- Jessie.

The following need to be entered as arguments when running the program because they contain
private information:
* TESLA_USER: Tesla.com username for the Tesla automobile to be monitored and managed;
* TESLA_PASSWORD: Tesla.com password;

The function DataDump() creates a file named tesladata.txt, which contains a
complete dump of the data provided by the Tesla API.  An example of this file is
provided in this repository.  (Note: location and vehicle name data is deleted
for privacy reasons.)

## Key Files in this repository
Here are the most important files and what they do:
* <b>nikola.py</b> is the main Python program to handle the incoming Alexa intents, query the Tesla API, issue commands through the API, formulate and return responses to be spoken by an Alexa device;
* <b>teslajson.py</b> is the third-party application, written by Greg Glockner, that actually handles the communications to and from the Tesla through the API;
* <b>intents.txt</b> is the most current schema of intents, in JSON format, that defines the inquiries and commands that Alexa can pass to Nikola.  This can be copied and pasted into the "Intent Schema" section of the "Interaction Model" tab for a Nikola Skill that can be defined and managed in the Amazon Developer Console.
* <b>utterances</b> is a list of "Sample Utterances" that Alexa uses to decide which intent to send to the application.  Like the Intent Schema, this can be copied and pasted into the "Sample Utterances" section of the Interaction Model for an Alexa Skill;
* <b>tesladata.txt</b> is, for reference purposes, a dump of API data from a Tesla Model X running v8.0 of the Tesla firmware as of November, 2016.  This list may be generated for any Tesla automobile at any time by invoking the "DataDump" intent by saying: "Alexa, ask Nikola to dump my car's data to a file."

## Setup and Installation
Prereqs - you should have Python installed an running on your PC.  I am using Python 2.7.

Sign up for an Amazon Developer Account - you can likely use your existing Amazon credentials.

Download all of the files from this repository and place them in a folder you create on your PC.
I am assuming that you are using a folder called nikola but that doesn't really matter.
You may have to download and install the following python packages:  flask, flask-ask, geocoder, 
isodate and geopy.  That is done by typing 'pip install flask' or whatever.
Run the main python file by typing 'python nikola.py fred@flinstone.com ILuvWilma'.
Of course you replace the parameters above with the email and password that you have used for
your Tesla app.  If that doesn't work then you may have an authentication issue. As an alternate
authentication method you can take a token that you have and paste it into line 30 of the
teslajson.py file.  That should keep using that token until it expires - but you will have to deal
with replacing it upon retirement.

Assuming it is working you now have your file waiting for commands on port 5000.

The next step is to setup ngrok.  Download and run the version of ngrok for your
PC - on windows you will need the command 'ngrok.exe http 5000' and on Linux it is 
'./ngrok http 5000'.  You will need some info from this screen in a little while.

Sign in to the Amazon Developer Console.  Click on Alexa on the top of the screen.  Then click
on the Get Started button of the Alexa Skills Kit.  Next click on the Add a New Skill button.

On the first screen use Nikola in both boxes.  Click Next.
On the next page copy everything from the intents.txt file and paste into the intent schema.
Copy everything from the utterances.txt file and paste into the Sample Utterances. Click next.
On the next screen for Endpoint click on HTTPS.  Pick your region.  In the box enter the https
URL from ngrok which should be the last Forwarding line.  For example if the line reads:
"Forwarding    https://ab12cd34.ngrok.io --> localost:5000" then enter this in the box.
Press Save or Next and you should now be working.

Ask Alexa for some info as in "Alexa ask Nikola what is the status of my car".

## Credits
Much credit goes to [Tim Dorr](http://timdorr.com) for documenting the Tesla JSON API.
Also to Greg Glockner for his teslajson.py approach to unlocking that API's power.
And Michael Kolowich and Andrew Payne for doing the original work on Nikola

## Disclaimer
This software is provided as-is.  This software is not supported by or
endorsed by Tesla Motors.  Tesla Motors does not publicly support the
underlying JSON API, so this software may stop working at any time.  The
author makes no guarantee to release an updated version to fix any
incompatibilities.
