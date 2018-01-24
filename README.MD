# Telecomm Relay
this project is a raspberry pi relay service.
that allows someone to wire a rj12 based telecomm relay phone to a raspberry pi.
the raspberry pi reads the rj12 audio in/audio out turns them to digital and vice versa then relays them to a HTML mobile interface.

## purpose
Basically I want to be able to:
-   answer calls from my buildings relay on my phone.
-   speak back to the other end (using webRTC) to have a two way conversation
-   press a button to unlock the front door (via raspberry pi GPIO)
-   run the whole thing as a web service that can be interfaces to alexa.

## v1 goal
raspberry pi runnning node/php server and GPIO listener.
buzzer triggers gpio wire to run service script.
activates audio capture (live stream) to webserver on the PI eg. http://myraspberry:8000/audiostream
listen to audio at interface http://myraspberry/buzzer
button press on page triggers GPIO to open door.

## v2 goal
service is API driven http://myraspberry/buzzer/api/opendoor
utilise alexa's call capabilities to open the stream on alexa.
buzzer triggers api http://myraspberry/buzzer/api/trigger
trigger sends signal to alexa <> alexa says "someone is at the door" (wait for input)
person: "ask who it is"
alexa sends audio recording to http://myraspberry/buzzer/api/sendvoice?response='who+is+there'
reponse listen. capture playback.
stream to alexa.


# components

raspberry pi: (v2 or higher)
GPIO break out. bread board.
rj11 to audio passthrough (http://www.dx.com/en/p/rj11-telephone-line-pass-thru-to-3-5mm-mini-jack-conversation-recording-line-out-adapter-18869#.Wmh7H5P1VYg)
usb audio in/out https://www.adafruit.com/product/1475


# basic process.

*hardware*
telephone comms system works over rj11. in and out.
fairly sure buzzer sends a signal over the in (microphone) pair of the telephone wiring  (hence why when you press the button you cant hear yourself... could be the other way around)

so rj11 would go to gpio wires.
gpio wires would go to audio in on usb in. (this is to recieve voice)
gpio wires would go to audio out on usb out (thisd is to send voice back)
both are analogue, the usb components would create the ability to record audio and transmit audio.

whichever wire pair sends the buzzer would need to be double upped in to a seperate gpio pin pair and marked.
most likely a switch is closed to send the signal to open the lock.

*software*

core process probably written in C++ or python would run as an abstrated service
with a basic non web based API, cli based eg. 
    bin/buzzer -send audiofile
    bin/buzzer -stream http://stream:8000
    bin/buzzer -unlock
    bin/buzzer -live

where live mode would run dumping the audio recording to /rec/file.str
and picking up from any audio broadcasted on http://stream:8000

then the innterface would run over the top in php or node
interacting with this process allowing a UI to talk and unlock the door.
as well as the api allowing alexa to send audio and recieve audio
