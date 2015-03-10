== Abstract ==
A Qt application using [[QBluetooth - A Qt bluetooth library | QBluetoothZero]] library to chat and send files to nearby devices.

The idea of creating this program was to have a cheap and easy to handle way to communicate with other nearby devices. So, people in a classroom, in a house and so on can chat for free. Also in the same time users can send files to the other devices.

== How to run ==
First download source files from [[File:QuteMessenger.zip]].

*''For Symbian (before compile):'' there is a folder named '''QBluetoothZero'''. In there, a folder exists by the name '''symbian'''. Copy its contents (the '''epoc32''' folder) to the root folder of the Symbian SDK that you are using (e.x. C:\QtSDK\Symbian\SDKs\Symbian1Qt473). This will lead to merge the already existing ''epoc32'' folder in there, with the new one that has only the binaries of the QBluetoothZero library. 

Next step, compile as Phone Debug/Release configuration and if all goes well it will be successful.

*''For Windows (before first run):'' make sure you have the QBluetoothZero.dll file in the same directory that the executable of your application is placed.

== Details ==
From now on we assume that every device that we want to connect to in order to chat, has the same application installed and running.

As soon as the application starts, a QBtSerialPortServer is created and is set to publish the service by the name "Messenger Protocol " + the name of the device. So any other device that finds this device through device discovery (QBtDeviceDiscoverer) then it already knows which service to look for. 

An other device will act as client (QBtSerialPortClient), will search for the above server through device discovery, select it and press ''Options -> Start Chat''. On the creation of the new view where the chat board will be placed, at the same time a QBtSerialPortClient is created and tries to connect to the selected server. If all this is successful then those two devices will be able to exchange messages.

On the server side, if the connection with the client is lost then the QBtSerialPortServer instance previously created is deleted and re-instantiated to be able accept future connection requests by other client.<br>
On both server and client side, if the connection is lost the view containing the chat board is useless so it should be closed manually. An other view will be created in case of new connection (on server side it is automatically created on successful client connection whereas the client creates the view when user presses ''Start chat'' as mentioned above).

Also at the main view (where the devices found are shown), selecting a device and pressing ''Options'' softkey provides and an other operation. User can send s selected file to the remote device. If selected, a QFileDialog is show for the user to select the file to send. Then a User's device instantiates a QBtObjectExchangeClient which connects to the remote device (doesn't require a QBtObjectExchangeServer to be created, the default OBEX server is just fine!). Upon successful connection, the QBtObjectExchangeClient sends the file by calling PutFile(''filename'').

[[File:QuteMessenger_home.png]]

[[File:QuteMessenger_deviceDiscovery.png]]

[[File:QuteMessenger_menu.png]] 

[[File:QuteMessenger_chat.png]]

== Links ==
* '''''[[File:QuteMessenger.zip | Download source code]]'''''
* [[Media:QBluetooth_lib.zip|QBluetoothZero.zip]]

[[User:Favoritas37|Favoritas37]] 00:47, 5 January 2012 (EET)
