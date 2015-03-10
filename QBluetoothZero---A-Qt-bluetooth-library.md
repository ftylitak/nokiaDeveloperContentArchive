== Abstract ==
QBluetoothZero is a '''3rd party''' bluetooth library written on Qt supported on Symbian and Windows. Its architecture is based on [http://www.developer.nokia.com/info/sw.nokia.com/id/89261a1d-ed6d-4cc5-8855-8250a33de328/S60_Platform_Bluetooth_API_Developers_Guide.html S60 Platform: Bluetooth API Developer's Guide].

; Warning : The project has been renamed from {{Icode|QBluetooth}} to {{Icode|QBluetoothZero}} to avoid conflicts with Bluetooth module of Qt Mobility. Users that have already used this library are advised to have a look at the following wiki page which states the changes that need to be made to comply with the new version of the library: [http://sourceforge.net/projects/qbluetoothzero/ QBluetoothZero])

The idea is to create a library implementation using Qt (a DLL) of the S60 bluetooth API easy to use by anyone with basic knowledge of Qt. 

So the architecture was extracted from S60 bluetooth API, and also source code was used from the following examples: 
* [http://www.developer.nokia.com/info/sw.nokia.com/id/385f811a-2940-444f-b906-e5d3444e121c/S60_Platform_Bluetooth_OBEX_Example_v1_0_en.zip.html S60 Platform Bluetooth OBEX Example] 
* [http://www.developer.nokia.com/info/sw.nokia.com/id/e56fccb6-2d70-4a02-9008-7b3e97927057/S60_Platform_Bluetooth_Point_to_Multipoint_Example.html S60 Platform Bluetooth Point to Multipoint Example] 
* [http://library.developer.nokia.com/index.jsp?topic=/S60_5th_Edition_Cpp_Developers_Library/GUID-441D327D-D737-42A2-BCEA-FE89FBCA2F35/Chat/doc/index.html S60 blueooth Serial Port Chat] 

and the basic guide for combining all these was [[Using Qt and Symbian C++ Together]]

NOTE: for more detailed description of the the library's components please advise the documentation. Documentation can be found in this site [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/index.html QBluetoothZero Documentation]. To '''download''' source code visit  [http://sourceforge.net/projects/qbluetoothzero/ project page].

An application using QBluetoothZero can be found here [[QuteMessenger - Bluetooth Chat|QuteMessenger - A Qt application using QBluetoothZero]]

Some code examples from the Nokia Wiki: 
* [[Discovering nearby Bluetooth services with the QBluetooth library]] 
* [[Discovering Bluetooth devices with the QBluetooth library]]

= Basic Information Container Classes =

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_address.html QBtAddress]'' : that represents the device's bluetooth address 
* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service.html QBtService]'' : all the necessary information about a bluetooth service 
* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_device.html QBtDevice]'' : all the information needed about any remote bluetooth device 
* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_local_device.html QBtLocalDevice]'' : implemented only with static functions through which user can access information of the local device

= Operational Classes =

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_device_discoverer.html QBtDeviceDiscoverer]'': the mechanism of the device discovery

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_single_device_selector_u_i.html QBtSingleDeviceSelectorUI]'': a simple widget to select a bluetooth device. Device discovery is initiated when widget is shown. Created for convenience if a user simply wants to select a discovered device to proceed to other operations

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html QBtServiceDiscoverer]'': provides the mechanism to inquire a given device for the bluetooth services it supports

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_advertiser.html QBtServiceAdvertiser]'': class responsible to advertise a given bluetooth service

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_object_exchange_client.html QBtObjectExchangeClient]'' : OBEX client, connects to a remote OBEX server to send/retrieve data and files

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_object_exchange_server.html QBtObjectExchangeServer]'' : OBEX server, waits for clients to connect and ready to serve any OBEX request

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html QBtSerialPortClient]'' : Serial Port (SP) client, connect to SP server to send/receive raw data through serial port

* ''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_server.html QBtSerialPortServer]'' : Serial Port server, ready to accept client connections to also send/receive raw data

= Building the library =

#. You will need a standalone SDK, like the S60 5th Edition or 3rd Edition FP2, for example. Download them from [http://www.developer.nokia.com/info/sw.nokia.com/id/ec866fab-4b76-49f6-b5a5-af0631419e9c/S60_All_in_One_SDKs.html Nokia Developer].
#. The library requires the BluetoothEngineAPI. This API is available as a SDK plugin. Please see [[SDK API Plug-in]] to see how to get it. 
#. Download and install [http://qt.nokia.com/downloads Qt].

Use QBluetoothZero.pro file to open the project. Next step is create the DLL. 

If compiled for Symbian and Carbide is used as IDE then we Freeze Exports (Project->Freeze Exports) and it is ready.
If compiled for Windows (in case of Visual Studio 2008 as IDE) just compile and the .dll and .lib files will be generated.

It's also possible to build the library with Nokia Qt SDK 1.0. However, the Symbian SDK that comes with Nokia Qt SDK 1.0 does not have all the libraries required to build the library (eg. BluetoothEngineAPI). The options are 1) copy the missing files from the 5th Edition SDK, or 2) choose the standalone SDKs as the Qt Version when configuring a project (Symbian device target).

=== Possible build errors ===

This section provides answers to some questions that might arise when building the library.

==== Qt Creator doesn't find the Symbian SDK header files ====
Add this to the QBluetoothZero .pro file, just before the HEADERS declaration at the beginning of the file.

<code cpp-qt>
# fix for Qt Creator to find the symbian headers
INCLUDEPATH += $$EPOCROOT\epoc32\include
</code>

==== Compiler error in <obexbase.h>, line 205: extra qualification 'CObex::' on member "!CancelObexConnection". ====

Edit the obexbase.h file and remove the CObex:: qualifier.

====Qt Creator complains about missing libraries====

The solution found so far for S60 5th Edition SDK and Nokia Qt SDK 1.0 is to install the BluetoothEngineAPI files from the S60 3rd Edition FP2 plugin pack. For the S60 3rd Edition FP1 SDK, please search for the plugin pack specific for this SDK.

If Qt Creator complains about {{{-lbtextnotifiers}}} or {{{-lirobex}}}, when using as target Qt devices (Nokia Qt SDK 1.0): The solution so far is to either use another SDK (as the 5th Edition SDK), or to copy the missing files from the 5th Edition SDK). The files are:

<code>
btextnotifiers.dso 
btextnotifiers.lib 
btextnotifiers{000a0000}.dso 
btextnotifiers{000a0000}.lib 

irobex.dso
irobex.lib
irobex{000a0000}.dso
irobex{000a0000}.lib
</code>

These files are located in '''\epoc32\release\armv5\lib''' of the SDK installation directory.

==== Qt Creator doesn't generate the .sis file for the library ====

No known solution so far. Even if having configured the build target to sign the file with a developer certificate, Qt Creator (Nokia Qt SDK 1.0) does not generate the sis file.  It seems to be a bug in Qt Creator.

To make Qt Creator generate the .pkg file, add this to the QBluetoothZero.pro file at the end of the file:

<code>
addFiles.sources = QBluetoothZero.dll
addFiles.path = !:\sys\bin
DEPLOYMENT += addFiles
</code>

==== Compiler errors when modifying the public API (any public method of QBluetoothZero classes), with Qt Creator ====

When modifying the public API in Symbian DLLs, it's necessary to freeze the API. Currently, Qt Creator does not do this, and there's no way to instruct it to make this step. This is a known Qt Creator bug  (https://bugreports.qt-project.org/browse/QTCREATORBUG-772). 

To freeze the API it's necessary to run it from the command line. Create a .bat file and place it in the same directory where the QBluetoothZero .pro file resides.  Write the text below:
<code>
set EPOCROOT=\Dev\Symbian\S60_3rd_FP1\
del eabi\*.def
abld freeze gcce
</code>

Set the '''EPOCROOT''' variable to the root of the Symbian SDK that is being used by the project (without the drive letter as in the example).  For Nokia Qt SDK 1.0, this is '''sdk instalation folder"\Symbian\SDK'''. Then, proceed as this:

#. Clean the project with Qt Creator
#. Build the project with Qt Creator
#. Run the .bat file
#. Build the project again with Qt Creator

= Including the library =

=== Use the output of the previous build ===

The following lines must be inserted in the .pro file of the project that is going to use the QBluetoothZero library.

If compiling for Symbian, add in ''symbian'' scope:

<code>
    
LIBS += -lQBluetoothZero

symbian {

    INCLUDEPATH += /epoc32/include/QBluetoothZero

    TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData 

    addFiles.sources = QBluetoothZero.dll
    addFiles.path = !:\sys\bin
    DEPLOYMENT += addFiles
}

</code>

If compiling for Windows, add in ''win32'' scope (supposing that the folder QBluetoothZero containing all the headers is in the same directory as the project that is going to use it):

<code>
    LIBS += -lQBluetoothZero \
        -LQBluetoothZero\bin\release
    INCLUDEPATH = QBluetoothZero
    HEADERS += QBluetoothZero.h
</code>

The LIBS label sets the location of the file containing the .lib file, the INCLUDEPATH sets the location of the header file and HEADERS is used to specify the exact header to be included.

At any case, whenever one of the library classes is to be used, the developer must include <QBluetoothZero>.

=== Use the binaries ===

In '''bin''' folder you can find the library already compiled and ready to be used.

The binary is a Shared Dll Library. It is installed once and can be used from multiple client programs without conflicts.

It looks good BUT it has some catches.

- the binaries are ''self signed ''so only the basic Capabiliites are supported: !LocalServices, !NetworkServices, !ReadUserData, !UserEnvironment, !WriteUserData, Location.

- The UID of the binary comes from the protected range. This means that it is only for testing and personal use. There is no guaranty that the UID will not confict with programs from other developers.

- In case someone wants to use the binary for an application that will be placed in Ovi Store, the binary has to be rebuild using a UID that the developer got a publisher.

* Step 1 
Copy the contents of bin file ("epoc32" folder) in the "epoc32" folder of your Symbian SDK.

* Step 2
Put the following lines in your .pro file:

<code>

symbian{
       INCLUDEPATH += /epoc32/include/QBluetoothZero
       LIBS += -lQBluetoothZero

       customrules.pkg_prerules  = \
	";QBluetoothZero" \
	"@\"$(EPOCROOT)Epoc32/InstallToDevice/QBluetoothZero_selfsigned.sis\",(0xA003328D)"\
	" "	
	
	DEPLOYMENT += customrules
}
</code>

In case you use Qt Creator, the following line:
<code>
"@\"$(EPOCROOT)Epoc32/InstallToDevice/QBluetoothZero_selfsigned.sis\",(0xA003328D)"\
</code>

must be changed into (just add a '$" character):
<code>
"@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetoothZero_selfsigned.sis\",(0xA003328D)"\
</code>

* Step 3
Compile your appllication.

= Code examples =

== Discovering devices in range ==
[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_device_discoverer.html QBtDeviceDiscoverer] is used to inquire for bluetooth devices in range.
- create new instance
- call [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_device_discoverer.html#a76e9223d3b2330c1c26c94bee5a1ae5f startDiscovery()] to initiate inquiry
- call [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_device_discoverer.html#a74851bd623fc4d333791d6a295c10478 getInquiredDevices()] to retrieve the devices found so far or connect to SIGNAL '''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_device_discoverer.html#aa88fadfe4eabe2b5ad1d7b0eb92653d3 newDeviceFound (QBtDevice)]''' to be reported each time a device is found

<code cpp-qt>

QBtDeviceDiscoverer* deviceDiscoverer = new QBtDeviceDiscoverer(this);
connect(deviceDiscoverer , SIGNAL(newDeviceFound (QBtDevice)), 
        this, SLOT(handlerFunction(QBtDevice)));
deviceDiscoverer ->startDiscovery();

</code>


== Discovering services on a device ==
[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html QBtServiceDiscoverer] is used to inquire a remote device for the services it supports. 
After instantiation, user can call one of the 3 overloads of startDiscovery()
- [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html#a3477976884356670db43a5ab337976f7 startDiscovery(QBtDevice*)] : get all the device's supported services
- [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html#a50a50d9dcd06d0aa19077f0f07646149 startDiscovery(QBtDevice*,QBtConstants::!ServiceClass)] : get all the device's supported services whose UUID equals to the given
- [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html#a1471b3ffba854e197e4c105fdc884c89 startDiscovery(QBtDevice*,QBtConstants::!ServiceProtocol)] : get all the device's supported services whose mechanism contains the service protocol given as argument

Use [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html#af9d1e5edcf33c98916c0e0f19f6f860a getInquiredServices()] to get the services found so far or connect to the SIGNAL [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_discoverer.html#a2ef2eb229485582e224d91aec67d7773 newServiceFound(const QBtDevice&, QBtService)] to be reported each time a service is found.

Example: inquire a device (variable name ''remoteDevice'') for all the Serial Port Protocols (SPP) it supports.

<code cpp-qt>

	QBtServiceDiscoverer* serviceDiscoverer = new QBtServiceDiscoverer(this);
	connect (serviceDiscoverer, SIGNAL(newServiceFound(const QBtDevice&, const QBtService&)), 
	        this, SLOT (handlerFunction(const QBtDevice&, const QBtService& )));

	serviceDiscoverer->startDiscovery (remoteDevice, QBtConstants::SerialPort);

</code>

== Publish a service ==
[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_advertiser.html QBtServiceAdvertiser] is responsible to advertise a given bluetooth service.

After the instantiation of the class, [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_advertiser.html#a52bc79594aefff3eb687688b4fdb4fad startAdvertising(const QBtService&)] can be called to start advertising the given bluetooth service. If successful then [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_advertiser.html#ad3b82bfa197b6ee4454082b4ebe7de6b advertisingStarted(const QBtService&)] signal is emitted.

NOTE: Currently there is no implementation on the Windows platform so after the instantiation of this object, any call to any function will emit an [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_service_advertiser.html#a11393d220f3013ab21b310253330d892 error(QBtServiceAdvertiser::FeatureNotSupported)] signal.

<code cpp-qt>

	QBtService newService;
	newService.setName(p_ptr->trService->getName());
	newService.setClass(QBtConstants::SerialPort);
	newService.setPort(aChannel);
	newService.addProtocol(QBtConstants::L2CAP);
	newService.addProtocol(QBtConstants::RFCOMM);
	
	QBtServiceAdvertiser* advertiser = new QBtServiceAdvertiser(this);
	advertiser->startAdvertising(newService);	
	
</code>

== Send/receive a file ==
[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_object_exchange_client.html QBtObjectExchangeClient] is used to connect to a remote OBEX server and send or recieve files or raw data.

Example connecting to a remote OBEX server. At successful connection user send a file to the server.

Header

<code cpp-qt>

 #include <QBluetoothZero>

class FileSender
{
Q_OBJECT

public:
    FileSender();

protected:
    void Setup();

private slots:
    void SendFile();

private:
    QBtObjectExchangeClient* client;
}

</code>

Source

<code cpp-qt>

FileSender::FileSender()
{
    Setup();
}

void FileSender::Setup()
{
    client = new QBtObjectExchangeClient(this);
    connect(client, SIGNAL(connectedToServer()), this,SLOT(SendFile()));
    client->connectToServer(remoteDevice, remoteService); // remoteDevice & remoteService are acquired from the previous examples
}

void FileSender::SendFile()
{
    client->putFile("C:\\picture.jpg");
}
</code>

== OBEX server ==
[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_object_exchange_server.html QBtObjectExchangeServer] is used to create an OBEX server. After instantiation user can call [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_object_exchange_server.html#a0eb4d3b5d82b34d109cc7433b34d48c8 startServer(const QString&)] to start the server. The feedback from the server's operations is given through SIGNALS.
<code cpp-qt>

    QBtObjectExchangeServer* server = new QBtObjectExchangeServer(this);
    server->startServer("MyObexServer");
</code>

 * NOTE: not implemented for Windows.

== Connecting to a Serial Port Server ==
In this case we use [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html QBtSerialPortClient]. Call [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html#ad15d5078a06b111ba81452f0a4edf1ad connect(const QBtDevice&, const QBtService&)] to connect to a remote Serial Port (SPP) Server. If successfull client can send data using [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html#afd7f72738c717e1eb0a42b5f64a5d69c sendData(const QString &)] function and reports received data through SIGNAL [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html#aa5d793a84a4dde54de51a29b83dac546 dataReceived(const QString)].

== Creating a Serial Port Server ==
[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_server.html QBtSerialPortServer] is used. Call [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_server.html#a01abd43a827da150d87911c3dfa81763 startServer(const QString&)] to start server and set the name of the publishing service and then wait for a client to connect. When a client is connected, it is reported through [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_server.html#a86c9f24f68a8fbd18f1653a598fc207d clientConnected(QBtAddress)] SIGNAL. At this point server is able to send data through sendData(const QString) and the data received are reported through [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_server.html#abee23ef5ea738a22652f8c9b2ba14fc9 dataReceived(QString)].

* NOTE: not implemented for Windows.


= Other useful information =
On the Windows implementation side, the bluetooth functions are provided by the Bluesoleil SDK. So in order to use the Windows implementation user must download the Bluesoleil SDK and Bluesoleil Drivers 6.4 and above. I know that this restriction is a drawback but I had a couple of months of experience on that SDK so it was the only chance for me to make a Windows implementation at this moment...but it works. 

* NOTE: if someone wants to use this implementation, consider to update the path of the folder containing the SDK in the QBluetoothZero.pro in the ''win32'' scope.

The Win Serial port functions are provided by Thierry Schneider through Tserial_event: 
  Copyright @ 2001-2002 Thierry Schneider 
                            thierry@tetraedre.com 

= Licensing =
This project is licensed under the Apache License, Version 2.0.

A few code portions are used from other Nokia code examples thus in the root directory of the project there are these two files:stating the license of each.
* License_OBEX_example.txt
* License_Point-to-Multipoint_Example.txt


= Future work =
<u>The project is further developed to its [http://sourceforge.net/projects/qbluetoothzero/ project page].</u>

= Links =
* [http://sourceforge.net/projects/qbluetoothzero/ Project's page]
* '''[http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/index.html The QBluetoothZero library documentation]''' <br>
* '''[[QuteMessenger - Bluetooth Chat | QuteMessenger - A Qt application using QBluetoothZero]]'''<br>
* [http://www.developer.nokia.com/info/sw.nokia.com/id/89261a1d-ed6d-4cc5-8855-8250a33de328/S60_Platform_Bluetooth_API_Developers_Guide.html S60 Platform: Bluetooth API Developer's Guide] <br>
* [http://www.developer.nokia.com/info/sw.nokia.com/id/385f811a-2940-444f-b906-e5d3444e121c/S60_Platform_Bluetooth_OBEX_Example_v1_0_en.zip.html S60 Platform Bluetooth OBEX Example] <br>
* [http://www.developer.nokia.com/info/sw.nokia.com/id/e56fccb6-2d70-4a02-9008-7b3e97927057/S60_Platform_Bluetooth_Point_to_Multipoint_Example.html S60 Platform Bluetooth Point to Multipoint Example] <br>
* [http://library.developer.nokia.com/index.jsp?topic=/S60_5th_Edition_Cpp_Developers_Library/GUID-441D327D-D737-42A2-BCEA-FE89FBCA2F35/Chat/doc/index.html S60 blueooth Serial Port Chat] <br>
* [[Bluetooth Protocol]]
* [[Service Discovery in Bluetooth]]

[[User:Favoritas37|Favoritas37]] 00:29, 5 January 2012 (EET)
