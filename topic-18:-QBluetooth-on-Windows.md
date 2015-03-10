Topic #18 - QBluetooth on Windows
----------------------------------------------------------------------------
Author:     dmartino
Created:    02/07/11 17:33:12
Moderators:
----------------------------------------------------------------------------

Hi!

I'm working to realize a system on Windows where the desktop application is a coordinator of a sensors set. Particularly, the coordinator needs to connect to the sensors via bluetooth, for data transfer. 

I want to realize this with Qt on Windows. Will I manage to do it with QBluetooth plus Bluesoleil SDK?  

Another question: Will I need to use QBtSerialPortServer on coordinator side to establish a channel with the sensors for data transfer?

Many thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/07/11 18:02:23
Moderators:
----------------------------------------------------------------------------

Hello again!

It has been long since we last took a look at the windows part of the library. Tomorrow you will have your answer because i will need some time to remember the limitations that you might have ( today i am quite busy).

Till then, the only issue that i can report is this one: https://projects.forum.nokia.com/qbluetooth/ticket/3

Also there might be a limitation from the Bluesoleil part of the number of concurent Bluetooth Serial Port connections that you can have.

:)

----------------------------------------------------------------------------
Author:     dmartino
Created:    02/09/11 17:39:58
Moderators:
----------------------------------------------------------------------------

Hi favoritas37,

thank you very much for your replies. These helped me, very much!

Nevertheless, I'm working to create the .dll file of QBluetooth library on Windows and I got some errors :(

I opened the QBluetooth.pro file in Visual Studio and the result of debugging is the follow:

c:\users\...\documents\qbluetooth_lib\qbluetooth\source\bttypes\impl\../../QBtAuxFunctions.h(162) : error C2065: 'QBtAddress' : undeclared identifier

1>c:\users\...\documents\qbluetooth_lib\qbluetooth\source\bttypes\impl\../../QBtAuxFunctions.h(162) : error C2065: 'address' : undeclared identifier

1>c:\users\...\documents\qbluetooth_lib\qbluetooth\source\bttypes\impl\../../QBtAuxFunctions.h(163) : error C2448: '!GetDeviceHandle' : function-style initializer appears to be a function definition

1>.\BTTypes\Impl\QBtUuid.cpp(184) : error C3861: 'BT_ASSERT_MSG2': identifier not found

1>QBtSingleDeviceSelectorUI.cpp

1>.\!DeviceDiscoverer\Impl\QBtSingleDeviceSelectorUI.cpp(18) : error C2065: '_implPtr' : undeclared identifier

1>QBtServiceDiscoverer_win32.cpp

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscoverer_win32.cpp(153) : error C2664: 'QBtService::setClass' : cannot convert parameter 1 from 'QBtConstants::!ServiceClass' to 'const QBtUuid &'

1>        Reason: cannot convert from 'QBtConstants::!ServiceClass' to 'const QBtUuid'

1>        No user-defined-conversion operator available that can perform this conversion, or the operator cannot be called

1>QBtServiceDiscovererForAll.cpp

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscovererForAll.cpp(158) : error C3861: 'BT_DEBUG_MSG': identifier not found

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscovererForAll.cpp(248) : error C3861: 'BT_DEBUG_MSG': identifier not found

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscovererForAll.cpp(259) : error C3861: 'BT_DEBUG_MSG': identifier not found

1>QBtServiceDiscoverer.cpp

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscoverer.cpp(56) : error C2039: '!DiscoverServices' : is not a member of 'QBtServiceDiscovererPrivate'

1> c:\users\...\documents\qbluetooth_lib\qbluetooth\source\servicediscoverer\impl\../QBtServiceDiscoverer_win32.h(28) : see declaration of 'QBtServiceDiscovererPrivate'

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscoverer.cpp(65) : error C2039: '!DiscoverObexServices' : is not a member of 'QBtServiceDiscovererPrivate'

1> c:\users\...\documents\qbluetooth_lib\qbluetooth\source\servicediscoverer\impl\../QBtServiceDiscoverer_win32.h(28) : see declaration of 'QBtServiceDiscovererPrivate'

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscoverer.cpp(73) : error C2039: '!DiscoverRfcommServices' : is not a member of 'QBtServiceDiscovererPrivate'

1> c:\users\...\documents\qbluetooth_lib\qbluetooth\source\servicediscoverer\impl\../QBtServiceDiscoverer_win32.h(28) : see declaration of 'QBtServiceDiscovererPrivate'

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscoverer.cpp(82) : error C2664: 'QBtServiceDiscovererPrivate::!DiscoverSpecificClass' : cannot convert parameter 2 from 'const QBtUuid' to 'QBtConstants::!ServiceClass'

1>        No user-defined-conversion operator available that can perform this conversion, or the operator cannot be called

1>.\!ServiceDiscoverer\Impl\QBtServiceDiscoverer.cpp(91) : error C2039: '!DiscoverSpecificClasses' : is not a member of 'QBtServiceDiscovererPrivate'

1>c:\users\...\documents\qbluetooth_lib\qbluetooth\source\servicediscoverer\impl\../QBtServiceDiscoverer_win32.h(28) : see declaration of 'QBtServiceDiscovererPrivate'

.\Connection\!SerialPort\Client\Impl\QBtSerialPortClient_win32.cpp(53) : error C2678: binary '==' : no operator found which takes a left-hand operand of type 'QBtUuid' (or there is no acceptable conversion)

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/global/qglobal.h(1874): could be 'bool operator ==(QBool,bool)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/global/qglobal.h(1875): or       'bool operator ==(bool,QBool)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/global/qglobal.h(1876): or       'bool operator ==(QBool,QBool)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qchar.h(386): or       'bool operator ==(QChar,QChar)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qbytearray.h(509): or       'bool operator ==(const QByteArray &,const QByteArray &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qbytearray.h(511): or       'bool operator ==(const QByteArray &,const char *)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qbytearray.h(513): or       'bool operator ==(const char *,const QByteArray &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(908): or       'bool operator ==(QString::Null,QString::Null)'

1>        c:\qt\commercial\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(909): or       'bool operator ==(QString::Null,const QString &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(910): or       'bool operator ==(const QString &,QString::Null)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(936): or       'bool operator ==(const char *,const QString &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(949): or       'bool operator ==(const char *,const QLatin1String &)'

1>        c:\qt\commercial\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(962): or       'bool operator ==(const QLatin1String &,const QLatin1String &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1170): or       'bool operator ==(const QStringRef &,const QStringRef &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1173): or       'bool operator ==(const QString &,const QStringRef &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1176): or       'bool operator ==(const QStringRef &,const QString &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1180): or       'bool operator ==(const QLatin1String &,const QStringRef &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1183): or       'bool operator ==(const QStringRef &,const QLatin1String &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1204): or       'bool operator ==(const char *,const QStringRef &)'

1>        c:\qt\4.7.0\include\qtcore\../../src/corelib/tools/qstring.h(1206): or       'bool operator ==(const QStringRef &,const char *)'

1>        C:\Program Files\Microsoft SDKs\Windows\v6.0A\include\guiddef.h(192): or       'int operator ==(const GUID &,const GUID &)'

1>        c:\Users\...\Documents\QBluetooth_lib\QBluetooth\source\BTTypes\QBtUuid.h(101): or       'bool QBtUuid::operator ==(const QBtUuid &) const'

1>        while trying to match the argument list '(QBtUuid, QBtConstants::!ServiceClass)'

1>.\Connection\!SerialPort\Client\Impl\QBtSerialPortClient_win32.cpp(182) : error C2664: 'Btsdk_ConnectEx' : cannot convert parameter 2 from 'QBtUuid' to 'BTUINT16'

1>        No user-defined-conversion operator available that can perform this conversion, or the operator cannot be called

1>QBtSerialPortClient.cpp

I have no idea to resolve these problems ... Can you help me?

Many thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/09/11 22:03:28
Moderators:
----------------------------------------------------------------------------

Well the windows part of the library was not updated during a last change of the library API so it is logical that it can't compile.

I would suggest to get an older version of the library (before some specific additions where made) which will work just fine.

Here is a link with the repository version 25 which was last checked to be working http://www.forum.nokia.com/piazza/wiki/images/archive/3/36/20101228164758%21QBluetooth_lib.zip

The Bluesoleil SDK can be found here: http://www.bluesoleil.com/support/Intro.aspx?topic=Download_SDK

----------------------------------------------------------------------------
Author:     dmartino
Created:    02/10/11 00:48:11
Moderators:
----------------------------------------------------------------------------

I finally managed to make the dll! 

Thank you very much!

:)

