Topic #31 - QBtLocalDevice::setBluetoothPowerState(true)
----------------------------------------------------------------------------
Author:     erespia2
Created:    03/18/11 13:04:28
Moderators:
----------------------------------------------------------------------------

Hi!!!

I'm trying to enable the Bluetooth function
QBtLocalDevice: setBluetoothPowerState (true), but when I run the
application and use that function, my application dies, it does crash.

Instead if I use QBtLocalDevice: askUserTurnOnBtPower ();, if I
enable bluetooth, but I need it enabled automatically without
need to be asking the user.

Another issue, is there a class or function to return a list
Bluetooth devices that have stored as friends?

Thanks.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 15:01:30
Moderators:
----------------------------------------------------------------------------

To use the !QBtLocalDevice:setBluetoothPowerState (true) you need to use a binary other than those with the selfsigned capabilities.

I think it needs the '''!WriteDeviceData''' capability which can be used only when properly signed. So if i provide you a .sis file with all the available capabilities, will you be able to sign it?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 15:02:27
Moderators:
----------------------------------------------------------------------------

I forgot to mention that currently we have not function that returns the Bluetooth devices stored as friends.

----------------------------------------------------------------------------
Author:     erespia2
Created:    03/18/11 16:06:19
Moderators:
----------------------------------------------------------------------------

Yes, I can sign, you can pass me. sis file with all the available Capabilities?

I remember that my OS is S60 3rd FP1 (N95 8GB.)

Regarding the other issue, is there any way I can know that I have stored devices?. I need to when you discover any previously stored device, run another application.

One more question, When I use the code:

'''QBtDeviceDiscoverer * QBtDeviceDiscoverer deviceDiscoverer = new (this);
connect (deviceDiscoverer, SIGNAL (newDeviceFound (QBtDevice)), this, SLOT (handlerFunction (QBtDevice)));
deviceDiscoverer-> startDiscovery ();'''

Sometimes I find the bluetooth device from other mobile and sometimes not. For example, if I run my application only has the code to discover devices, I want this all the time looking for devices, when he has a running and active while the other cell phone bluetooth for you to discover, not discovered. In contrast, if the bluetooth is already on another terminal and run my application, if it discovers. What will happen?
I want to make my application is that in the background all the time looking for devices, and when you find one that is already defined in the phone, then run a particular application.

"I can get this in any way with your library?

Thank you very much!!

Message #144 - CRASH: QBtLocalDevice::setBluetoothPowerState(true)
----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 16:29:40
Moderators:
----------------------------------------------------------------------------

Firstly, i suppose the code that you posted should look like this (careful with the signal signature: it takes argument "const &QBtDevice" not "QBtDevice"):

{{{
QBtDeviceDiscoverer* deviceDiscoverer = new QBtDeviceDiscoverer(this);

connect(deviceDiscoverer, SIGNAL(newDeviceFound (const &QBtDevice)),
        this, SLOT (handlerFunction (const &QBtDevice)));

deviceDiscoverer-> startDiscovery ();
}}}
Now, how you could do the operation you asked...

a) create a file in the same directory as the application running.

b) start the discovery and write every device is reported as found in the file as well as in a QList<QBtDevice>

c) If discovery is stopped and restarted, at the discovery of every device you will check if it is already found from the file or from the QList

Maybe you will ask why to have both input file and QList. QList can be used as the main storage when the application is running. So every device reported can be stored there and can later on be recovered for you to check if it is a new device or not. The output file will be used when the application starts or terminates. On termination you will write all the data from the QList (in any format is convenient to you) to the output file. In that way, when the application starts again you first load the data from the file in the QList and you are ready to continue as if the application was never closed.

Mind that you don't have to store the whole data from each QBtDevice. It name and address will be enough.

----------------------------------------------------------------------------
Author:     erespia2
Created:    03/18/11 17:07:44
Moderators:
----------------------------------------------------------------------------

Ok, I understand what you mean more or less, is a good idea. Thanks

With regard to the library with all the necessary permissions to run QBtLocalDevice: setBluetoothPowerState (true), You Can pass me. with all the file system available Capabilities?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 18:06:26
Moderators:
----------------------------------------------------------------------------

In the slot that you will connect with the [http://www.csd.uoc.gr/%7Eftylitak/QBluetooth_repo_doc/class_q_bt_device_discoverer.html#ae5005eee0f36cab45bb63460ba30a5ac discoveryStarted] () signal, you will call startDiscovery(). So whenever the discovery stops, you restart it.

And once more,  the following can't be valid C++ code
{{{
QBtDeviceDiscoverer * QBtDeviceDiscoverer deviceDiscoverer = new (this);
}}}

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 18:37:00
Moderators:
----------------------------------------------------------------------------

TARGET.CAPABILITY = All -TCB -[https://projects.forum.nokia.com/qbluetooth/wiki/AllFiles AllFiles?] -DRM

----------------------------------------------------------------------------
Author:     erespia2
Created:    03/18/11 19:29:55
Moderators:
----------------------------------------------------------------------------

I can not use the debugger, because my certificate can not open the
QtCreator compiler.

I have a certificate of OPDA (www.opda.cn), and all applications
I've done with a capabilities as Location, etc, has signed to me
perfectly.

Maybe my signing certificate that is not used?

Thanks

----------------------------------------------------------------------------
Author:     erespia2
Created:    03/24/11 19:46:00
Moderators:
----------------------------------------------------------------------------

Hello.

I still have the problem that does not work for the library QBluetooth
S60 3rd FP1, you gave me, that I signed with my
certificate. If it can not be a problem with my certificate (OPDA.cn), but always
I worked well with permissions as WriteDeviceData, Location,
ReadDeviceData, etc.

Any ideas?, what if he signed with open signed work?

Thanks.

