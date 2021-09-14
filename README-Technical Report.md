# Technical documentation - C301 Individual Capstone Project: Internet of Things: Connections, Data exchange and security
The project is about the connections and data exchange happening on Internet of Things. Flowwing in the document there are the requirementsneeded in order to run the project. There are in total three differents parts that have to be executed: 
* Server/Client connection and communication without MQTT
* Subscriber/Publisher communication using MQTT
* Set up of Mosquitto broker and configuring of files to add username and paswword authentication

## Project features: 
1)	Make connection with client 
2)	Start conversation with client
3)	Allow conversation between clients one at each time 
4)	MQTT implementation


## Hardware requirements:
* 2 Raspberry Pi 3 Model B 
* 2 Raspberry Pi power supplies
* 2 monitors with HDMI input (one can be used for both devices althoug is suggested to use two monitor)
* 2 HDMI cables 
* 2 mouse 
* 2 keyboard
* 2 MicroSD cards at least 16Gb each

## Software requirements/installations needed 
* Raspbian OS on Raspberry Pi
* balenaEtcher (to install the images on the two Raspberry Pi)
* paho.mqtt library
* Mosquitto broker 
* Dekstop/Laptop computer
* Pythons



## First of all the Raspbian OS must be installed into the two devices. 

* Raspbian OS must  to be downloaded from the following link in zip format including Desktop and recommended software: https://www.raspberrypi.org/downloads/raspbian/, after it is downloaded, the image is loaded into the Etcher software, MicroSD is entered into the PC and Etcher software unpacks the image and uploads it to the MicroSD, after this procedure is completed, Micro SD is entered into a Raspbian Pi, some easy configurations are done by the user and the device is ready to be used. Python is already installed through Raspbian installation.

## Server/Client connection and communication without MQTT

Moving on to the first feature, first of all a library named 'sockets' have to be installed in the two Raspberry Pi, in order to have IP/TCP connection between the devices. This done by going into the terminal of the device and typing: 

 `sudo apt-get install socket` 

After this installation, two python program files are required and can be downloaded from the following links: 
* server.py Link
* client.py  Link 

After you download those two files you have to transfer the server.py file to the one Pi and the client.py to the other Pi. Then you have to open the server.py and client.py and change their 'host' with the IP of the Raspberry PI choosen to be the server. 

→ To find the IP of the server Pi you can type in terminal: `ifconfig`


→ Mention that the two devices have to be conencted to the same network, moreover a WiFi hotspot can be created from a computer, so somehow a specific 'network' is created for the IoT system.  the terminal window from both devices and go to the directory(folder) the code is eg.  '`cd /youfolder `'.  


### Now everything is ready to run the two codes: 
1) first run the server.py file from terminal window of server's Pi by typing the following command: `python3 server.py`
2) then run the client.py code from terminal windows of client's Pi by typing the following command: `python3 client.py`
3) After this on server's Pi a message will displayed stating the IP address of client's Pi.

4) Now the communication and message exchange will start

5) From client's Pi type one of the following commands "HELLO", "REPEAT", "BYE" , "CLOSE" to start the conversation. 

   * HELLO comand is to send a message to the server and receive back a message 
   * REPEAT is to send a message to server 
   * BYE is to leave the connection 
   * CLOSE is to shut down the conenction with the server


6) Each time a message is sent the server prints 'Data has been sent'

7) If you want to test that only one client is allowed to communicate with the server, you can first conenct from one device running the clent.py code, start the command communication and at the same time execute the program from another device, you will notice that the second device is connected although when commands are asked, the devices stays in a standby mode. 


## Subscriber/Publisher communication using MQTT

This is the first part of the fourth feature where the implementation of MQTT is applied and the codes are running from terminal window again. Here the subscriber/publisher model used in MQTT is presented. A subscriber is filtering the data. 

Two python files have to be downloaded:
* subscriber.py Link: 
* publisher.py Link: 

Before running the files another python MQTT library has to be installed. Go to terminal window and type the following command to install the library: 

`sudo pip install paho-mqtt`

After this run these files from terminal window be sure you are in the correct directory. As and the first part above the two Raspberry Pi must be conencted to the same network. As and before first the subscriber.py program have to be executed first and then the publisher.py file. 

The programs should be executed from termianl like this: 

`python MQTT_subscriber.py`
`python MQTT_publisher.py`


In this part the two codes can be executes in the same Pi, first the subscriber.py and then the publisher.py. 



After running this you will see the outcome and undestand how the topic is working using MQTT.

### How data is filtered by subscriber.py code: 
* There are two topics 'welcome' and 'hello'
* The message for 'welcome' should be 'Welcome'
* The message for 'hello' should be 'Hello'
* If message and topic is correct a message is outputted
* If message is correct and topic is wrong, the subscriber is displaying the related message
* If the content of message is wrong, a message is displayed notifying about the wrong content
* If message is empty, a message is displayed saying that message is empty


Publisher's code is publishing in two topics and is sending two messages to each topic. After sending the messages the program is terminated, printing 'Messages published'.  Publisher's code can be sending messages iven if the client/subscriber to those topics is offline. 


## Set up of Mosquitto broker and configuring of files to add username and paswword authentication

This is the second part of the fourth feature. In this part the user learns how to set up a MQTT broker through some Mosquitto libraries. The user have to follow some instruction described bellow, in order to set up a broker. Basically the user needs to set up manually a broker, edit the configuration files and add authentication using username and password. The configurations are saved in broker device. 

Devices have to be connected to the same network in order to subscribe to a topic and get data. 

Before doing anything is good to update and upgrade (although it may take up to 30-40 minutes) the Raspbian OS this can be done by typing the following commands in terminal window: 

`sudo apt update`

`sufo apt full-upgrade`


## Follow instructions/commands from terminal:
1) install Mosquitto broker --> `sudo apt-get install mosquitto`
2) install a client library from Mosquitto --> `sudo apt-get install mosquitto-clients`

After doing the steps above you will start configuring through nano the broker: 

3) type the following command to start configuring the broker --> `sudo nano /etc/mosquitto.conf`

Now a nano window will open and you have to edit the configuration file, to add authentication, and assigning a port for the MQTT connection. You should find the following line: `incude_dir /etc/mosquitto/conf.d` and delete it. Then you have to add the following commands in order to ask for authentication:

* `allow_anonymous false` --> it means to block anyone without authentication 
* `password_file /etc/mosquito/pwfile ` -->it specified where to save the password/username file
* `listener 1883` --> port used to do the MQTT connections
* press ctrl+X to save the nano file and press Y to confirm changes, after this you will go back to terminal window

4) Now you have to set up the username and password used for the authentication 

5) Type into terminal window the following command, replace USERNAME with your username: 
* `sudo mosquitto_passwd -c /etc/mosquitto/pwfile USERNAME`
* after typing this you will be ask to type in a password two times 
* in order to set up the authentication you will have to reboot the system by typing: `sudo reboot` 

Now you can choose if you want to publish in the broker through the same Pi or through the other, it is recommended to publish from the other Raspberry Pi, so stepts 1&2 have to be applied and to the second Pi. 

After doing this, you can type the following commands in order to receive the data sent by the publisher: 

### To subscribe to a topic type into the terminal: 

* `mosquitto_sub -h YOUR IP ADDRESS -u YOUR USERNAME -P YOUR PASSWORD -t "test/topic"` by this command you subscribe to a topic 

### To publish to a topic type into the terminal: 

* `mosquitto_pub -d  -u YOUR USERNAME -P YOUR PASSWORD -t "test/topic" -m "your message"`by this command you are sending the messages to the subscriber, this commands must be executed from the device initialy set up the broker. 


