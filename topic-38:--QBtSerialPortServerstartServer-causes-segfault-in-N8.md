Topic #38 - QBtSerialPortServer::startServer causes segfault in N8
----------------------------------------------------------------------------
Author:     TapaniS
Created:    05/17/11 16:56:13
Moderators:
----------------------------------------------------------------------------

Hi!

I'm trying to develop a bluetooth service to Nokia N8 device. However, when running my program on the device it crashes. When running application in debug mode from Qtcreator, I get segmentation fault message when program enters QBtSerialPortServer::startServer().

This behaviour is present with my own code and with QuteMessenger (http://wiki.forum.nokia.com/index.php/QuteMessenger_-_Bluetooth_Chat).

Any experience on the issue?

BR,

Tapani

----------------------------------------------------------------------------
Author:     favoritas37
Created:    05/17/11 17:10:05
Moderators:
----------------------------------------------------------------------------

Hello TapaniS.

I haven't experienced the issue that you are saying but i would be glad to take a look at it.

My problems are first that i don't have a Symbian ^3 device to test it but only S60 3rd FP2 and last but most important, no free time for a few days.

So, can't promise that i will reply quickly but i will try as soon as possible.

----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    05/22/11 23:23:34
Moderators:
----------------------------------------------------------------------------

It is no longer needed. I added sendData('''QByteArray''') method by myself an everything works fine.

