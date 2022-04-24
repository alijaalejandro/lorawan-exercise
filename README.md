# IIoT LoRaWAN project (a practical exercise) 

This repo is about a practical exercise (in summary) on:

1. Connecting an analog sensor (temperature and rel. humidity) to a LoRaWAN transmitter node 
2. Capturing these data by using a LoRaWAN gateway
3. Configuring the gateway as a network server to route the LoRa traffic to a MQTT broker


![gateway/node/sensor](/images/IMG_6021.png)
![gateway/node/sensor](/images/IMG_6019.png)
![gateway/node/sensor](/images/IMG_6020.png)

# What do we need to try this?

To develop this lab we have a set of LoRaWAN transmitter node and gateway. Both are commercial equipment from Advantech.

- The gateway Advantech [WISE 6610](https://www.advantech.com/products/dc06386d-75f7-438c-879e-979ce8cabfda/wise-6610-a100/mod_db167821-a8fa-458d-be9a-c1f730682da4)
- The transmitter node [Wzzard LRPv2 Node](https://www.advantech.com/products/dc06386d-75f7-438c-879e-979ce8cabfda/bb-wsw2c42100/mod_3a8e8c8e-fb19-49b6-886f-fe5348d47afd)
- The sensor is a commercial sensor manufactured by Methron-Dropsens company.

_Please check all the documents related in folder [Documents](/documents)_


## Step 1

- Configure the LoRaWAN node
	- To do this, download and install the [utility](/tools). Have in mind this utility is windows only so you must have a Windows machine available to perform this operation. It's quite straightforward to follow the tutorial and configure the node.
	- By doing this we must take a note of ```Devaddr```	and a couple of ```keys``` codes that appears at the first screen in the utility. Then, in the sensor tab, you must check you get signals from Analog input ports and don't forget configuring the sampling rate (15'' in our case)
	After this operation, unplug the micro-usb and power up the node by using the batteries.

## Step 2

- Configure the LoRaWAN gateway
	- Power up the gateway and connect the Ethernet cable.
	- For a fresh installation, connect the Ethernet cable directly to your machine and configure the network adapter in the range 192.168.1.XX
	- Access to the http://192.168.1.1:8080 and use root/root as standard user/passwd.
	- Change the default passwd if you want.
	- Depending of your configuration you may want to configure the LAN section to enabling DHCP. For a domestic configuration you may want to connect the gateway to your router. In that case you must enable DHCP to get an IP in your local network.
	- After this, find the new IP assigned to the LoRaWAN gateway and repeat the login.

	![node-red](/images/general.png)

## Step 3. The funny part

- Here is where the things become more interesting.
- The gateway has custom modules to act as a gateway from LoRaWAN network to an IP network.
- You must access to ```User Modules -> LoRaWAN Gateway```
- You can explore hundreds of menus and configurations there, but basically, you must check the node is configured and connected to the gateway.
- Be sure you enable the MQTT server/broker and standard config must be ```127.0.0.1:1883```
- Access to ```http://192.168.68.112:8080/```
	- Here is the Server Admin configuration.
	- Go to Devices and Activated Nodes and configure the node with the corresponding data (the previous keys)
	- Use the EU868 handler
	- ```VERY IMPORTANT``` in order to get the sensor data type ```Advantech``` in the field ```App Arguments```
	- As soon as you place this word there you start to see a new topic at the MQTT broker called Advantech and there, (in the payload) the value of the AI and DI/DO
	- In section handlers you can add more data (more fields) to the uplink payload

	![node-red](/images/handler.png)
	![node-red](/images/appargument.png)
	![node-red](/images/receivedframes.png)

## Step 4. Get the data at the MQTT broker

- Open MQTT explorer or any other MQTT client.
- Connect to the IP assigned by the router to your gateway.
- Standard port 1883 with no authentication

![node-red](/images/mqttexplorer.png)

## Bonustrack. Node-red module

- Node red can be installed as module inside of the gateway.
- By doing this, we can program our own data transformation directly into the gateway.
- We can connect to the uplink or Advantech topic and retrieve the LoRa data. 
- We can also transform this data and send it (re-publish) to an IoT platform like IoT hub o the Thingsboard by using again an MQTT external broker.

![node-red](/images/noderedmodule.png)







