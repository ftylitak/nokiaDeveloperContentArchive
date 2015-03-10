Topic #40 -
----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    07/13/11 17:56:40
Moderators:
----------------------------------------------------------------------------

Hi,

I am facing some problem while receiving data using QBluetooth library. Actually the actual data gets converted at the time of receiving. This problem I have resolved for Desktop version by changing the QBluetooth Library in the following file:

'''QBtSerialPortClient_win32.cpp''' @ line 320


{{{
//QString newArray = QString::fromUtf8(buffer, size);   //      Original code commented by Me
                                        QString newArray(QByteArray(buffer, size));//   added by Me
                                        emit p_ptr->dataReceived(newArray);
}}}

After modifying this I am able to view the binary data properly, but this is in Desktop version.

I am trying the same thing for Symbian devices, infact I tried to change the following file:

'''QBtSerialPortClient_symbian.cpp''' @ line number 461
{{{
            //QString receivedString = QString::fromUtf8((char*)iBuffer.Ptr(), iBuffer.Size());// Original Code commented by Me
            QString receivedString(QByteArray((char*)iBuffer.Ptr(), iBuffer.Size()));//Added by Me

            // start expecting new incoming data
            ReceiveData();

            // notify
            QT_TRYCATCH_LEAVING (emit p_ptr->dataReceived (receivedString) );
            break;
}}}

But the capabilities used are not self signed, so it's not possible for me to use the Built dll in my Application to get the proper  received data.

How can I solve this problem.

Also i just want to know, if I can use this QBluetooth library for Meego platform?

Please let me know.

Thanks...

----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    07/14/11 08:01:47
Moderators:
----------------------------------------------------------------------------

Hey favoritas37,

If you look into the QBluetooth.pro file,  you will find the capabilities for Symbian build like,


{{{
TARGET.CAPABILITY = All -TCB -AllFiles -DRM
}}}

I am able to build the library with the changes mentioned in my first post and when I tried to install the generated sis file(qbluetooth.sis) alone, on my mobile device it is giving me error like:


{{{
unable to install a protected application from an untrusted supplier
}}}

Then i tried to use the generated sis file by putting the file in the folder "InstallToDevice",  in my Application pro file like:


{{{
symbian {
    INCLUDEPATH += /epoc32/include/QBluetooth

    LIBS += -lQBluetooth
    TARGET.UID3 = 0xe8e420c6

    TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData

    customrules.pkg_prerules = ";QBluetooth" \
    "@\"$$(EPOCROOT)Epoc32/InstallToDevice/qbluetooth.sis\",(0xA003328D)"\
    " "
    DEPLOYMENT += customrules
    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 \
        0x800000
}
}}}

Here when I tried to build the application it is giving the error like:


{{{
SIS creation failed
}}}

after little analysis, I found that it is due to the qbluetooth.sis file. Because when I tried to use the original QBluetooth_selfsigned.sisx file provided by you,  it is working fine:


{{{
customrules.pkg_prerules = ";QBluetooth" \
"@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_selfsigned.sisx\",(0xA003328D)"\
" "
}}}

I hope you understood my problem. Please provide your suggestions on this.

For Maemo you don't have any support, what about Meego. Can I use this library for Meego project development?

Thanks...

----------------------------------------------------------------------------
Author:     favoritas37
Created:    07/14/11 11:00:45
Moderators:
----------------------------------------------------------------------------

Meego is not supported also.

By default in our source code the UID of the library is '''0x2003328D'''. So if you compiled the project with its default UID, then in the following code you should use the UID that is in the qbluetooth.pro file: `customrules.pkg_prerules = ";QBluetooth" \ "@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_selfsigned.sisx\",(0x2003328D)"\ " "`

So be always sure that the UID of the project and the UID of the .sis that is defined when used are the same. Of course if the UID doesn't fits you can freely change it. For example in the selfsigned binaries provided by this site we used the 0xA003328D which a UID in the protected range meaning that it should be used only for testing purposes.

Regarding the version of the binaries that have the following capabilities: `TARGET.CAPABILITY = All -TCB -AllFiles -DRM` This can only be used if you properly sign the .sis file before its use. For the signing of the .sis file you can find plenty of information in the Developer Discussion Boards or from previous posts here.

So which are your 2 options:

 * If you are a registered developer, you have UIDs assigned to you. Replace the UID of the lirbary with one of yours, keep the capabilities as is and sign the .sis file. When using you must define the new UID that you used (as mentioned above).
 * If you want to use self signed capabilities, change the UID from '''0x2003328D '''to''' 0xA003328D''' and replace the capabilities with the following {{{[[BR]]
{{{
TARGET.CAPABILITY += Location
        LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData}}} Once again when using the .sis file be careful to use the correct UID.
}}}

----------------------------------------------------------------------------
Author:     favoritas37
Created:    07/14/11 11:11:41
Moderators:
----------------------------------------------------------------------------

You might also find useful the following wiki page: https://projects.developer.nokia.com/qbluetooth/wiki/binaries

----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    07/14/11 11:49:26
Moderators:
----------------------------------------------------------------------------

favoritas37,

I have just changed the code in the QBtSerialPortClient_symbian.cpp file, and everything else remains the same even the UUID. Please check the Bluetooth.pro and my Application that uses the Bluetooth library.

Bluetooth.pro:

{{{
uid3 = 0x2003328D

QT += xml

TEMPLATE = lib
#TARGET   = QBluetooth_0x2003328D
TARGET   = QBluetooth
DEFINES  += BLUETOOTH_LIB

PUBLIC_HEADERS += QBtGlobal.h \
    QBtAuxFunctions.h \
        DeviceDiscoverer/QBtDeviceDiscoverer.h \
    BTTypes/QBtAddress.h \
    BTTypes/QBtDevice.h \
    BTTypes/QBtService.h \
    BTTypes/QBtConstants.h \
    BTTypes/QBtTypes.h \
    BTTypes/QBtUuid.h \
    DeviceDiscoverer/QBtSingleDeviceSelectorUI.h \
    ServiceDiscoverer/QBtServiceDiscoverer.h \
    ServiceDiscoverer/QBtServiceDiscovererForAll.h \
    ServiceAdvertiser/QBtServiceAdvertiser.h \
    Connection/SerialPort/Server/QBtSerialPortServer.h \
    Connection/SerialPort/Client/QBtSerialPortClient.h \
    Connection/ObjectExchange/Client/QBtObjectExchangeClient.h \
    LocalDevice/QBtLocalDevice.h \
    Connection/ObjectExchange/Server/QBtObjectExchangeServer.h \
    QBluetooth.h

HEADERS += $$PUBLIC_HEADERS


SOURCES += Connection/ObjectExchange/Server/Impl/QBtObjectExchangeServer.cpp \
    LocalDevice/Impl/QBtLocalDevice.cpp \
    Connection/ObjectExchange/Client/Impl/QBtObjectExchangeClient.cpp \
    Connection/SerialPort/Client/Impl/QBtSerialPortClient.cpp \
    Connection/SerialPort/Server/Impl/QBtSerialPortServer.cpp \
    ServiceAdvertiser/Impl/QBtServiceAdvertiser.cpp \
    BTTypes/Impl/QBtService.cpp \
    ServiceDiscoverer/Impl/QBtServiceDiscoverer.cpp \
    ServiceDiscoverer/Impl/QBtServiceDiscovererForAll.cpp \
    DeviceDiscoverer/Impl/QBtSingleDeviceSelectorUI.cpp \
    DeviceDiscoverer/Impl/QBtDeviceDiscoverer.cpp \
    BTTypes/Impl/QBtAddress.cpp \
    BTTypes/Impl/QBtDevice.cpp \
    BTTypes/Impl/QBtUuid.cpp

symbian {
        # fix for Qt Creator to find the symbian headers
        INCLUDEPATH += $$EPOCROOT/epoc32/include

    deploy.path = $$EPOCROOT
    exportheaders.sources = $$PUBLIC_HEADERS
    exportheaders.path = epoc32/include

    INCLUDEPATH += $$deploy.path$$exportheaders.path/QBluetooth/

    INCLUDEPATH += $$deploy.path$$exportheaders.path


    TARGET.UID3 = 0x2003328D

    TARGET.EPOCALLOWDLLDATA = 1

    HEADERS += Connection/ObjectExchange/Server/QBtObjectExchangeServer_symbian.h \
        LocalDevice/QBtLocalDevice_symbian.h \
        Connection/ObjectExchange/Client/QBtObjectExchangeClient_symbian.h \
        Connection/SerialPort/Client/QBtSerialPortClient_symbian.h \
        Connection/SerialPort/Server/QBtSerialPortServer_symbian.h \
        ServiceAdvertiser/QBtServiceAdvertiser_symbian.h \
        ServiceDiscoverer/QBtServiceDiscoverer_symbian.h \
        DeviceDiscoverer/QBtDeviceDiscoverer_symbian.h \
        DeviceDiscoverer/QBtSingleDeviceSelectorUI_symbian.h \
        DeviceDiscoverer/BtDeviceFinderDlg_symbian.h

    SOURCES += Connection/ObjectExchange/Server/Impl/QBtObjectExchangeServer_symbian.cpp \
        LocalDevice/Impl/QBtLocalDevice_symbian.cpp \
        Connection/ObjectExchange/Client/Impl/QBtObjectExchangeClient_symbian.cpp \
        Connection/SerialPort/Client/Impl/QBtSerialPortClient_symbian.cpp \
        Connection/SerialPort/Server/Impl/QBtSerialPortServer_symbian.cpp \
        ServiceAdvertiser/Impl/QBtServiceAdvertiser_symbian.cpp \
        ServiceDiscoverer/Impl/QBtServiceDiscoverer_symbian.cpp \
        DeviceDiscoverer/Impl/QBtDeviceDiscoverer_symbian.cpp \
        DeviceDiscoverer/Impl/QBtSingleDeviceSelectorUI_symbian.cpp \
        DeviceDiscoverer/Impl/BtDeviceFinderDlg_symbian.cpp

    # more libs based on s60 version
    contains (S60_VERSION, 3.1):BT_PLUGIN_LIB = -lbteng
    else:BT_PLUGIN_LIB = -lbtengsettings

    #
    LIBS += -lbluetooth \
         -lbtextnotifiers \
        -lsdpdatabase \
        -lsdpagent \
        -lirobex \
        -lefsrv \
        -lfeatdiscovery \
        -lcentralrepository \
        -lbafl \
        -leikcore \
        -lcone \
        -lbtdevice \
        -lbtmanclient \
        -lesock \


    LIBS += $$BT_PLUGIN_LIB

    TARGET.CAPABILITY = All -TCB -AllFiles -DRM

    for(header, exportheaders.sources):BLD_INF_RULES.prj_exports += "$$header $$deploy.path$$exportheaders.path/QBluetooth/$$basename(header)"

        # add this for Qt Creator to generate a pkg file, it seems to be a bug in the current version (2.0.0)
        #addFiles.sources = QBluetooth_0x2003328D.dll
        addFiles.sources = QBluetooth.dll
        addFiles.path = /sys/bin
        DEPLOYMENT += addFiles
}


else {
    win32 {
        LIBS += -lBlueSoleil_SDK_2.0.5/bin/BsSDK

                INCLUDEPATH = ./ \
                        BTTypes \
                        BlueSoleil_SDK_2.0.5/include \
            Connection/ObjectExchange/Server \
            LocalDevice \
            Connection/ObjectExchange/Client \
            Connection/SerialPort/Client \
            Connection/SerialPort/Server \
            ServiceAdvertiser \
            ServiceDiscoverer \
            DeviceDiscoverer \
            WinSerialPort
        DESTDIR += $$(QTDIR)\QBluetooth\bin
        HEADERS += Connection/ObjectExchange/Server/QBtObjectExchangeServer_win32.h \
            LocalDevice/QBtLocalDevice_win32.h \
            Connection/ObjectExchange/Client/QBtObjectExchangeClient_win32.h \
            Connection/SerialPort/Client/QBtSerialPortClient_win32.h \
            Connection/SerialPort/Server/QBtSerialPortServer_win32.h \
            ServiceAdvertiser/QBtServiceAdvertiser_win32.h \
            ServiceDiscoverer/QBtServiceDiscoverer_win32.h \
            DeviceDiscoverer/QBtDeviceDiscoverer_win32.h \
                        DeviceDiscoverer/QBtSingleDeviceSelectorUI_stub.h \
            WinSerialPort/Tserial_event.h
        SOURCES += Connection/ObjectExchange/Server/Impl/QBtObjectExchangeServer_win32.cpp \
            LocalDevice/Impl/QBtLocalDevice_win32.cpp \
            Connection/ObjectExchange/Client/Impl/QBtObjectExchangeClient_win32.cpp \
            Connection/SerialPort/Client/Impl/QBtSerialPortClient_win32.cpp \
            Connection/SerialPort/Server/Impl/QBtSerialPortServer_win32.cpp \
            ServiceAdvertiser/Impl/QBtServiceAdvertiser_win32.cpp \
            ServiceDiscoverer/Impl/QBtServiceDiscoverer_win32.cpp \
                        DeviceDiscoverer/Impl/QBtSingleDeviceSelectorUI_stub.cpp \
            DeviceDiscoverer/Impl/QBtDeviceDiscoverer_win32.cpp \
            WinSerialPort/Tserial_event.cpp

                exportheaders.sources = $$PUBLIC_HEADERS


                for(header, exportheaders.sources){
                        DESTDIR += "$$header $$DESTDIR\include\$$basename(header)"
                }
    }
    else {
        HEADERS += Connection/ObjectExchange/Server/QBtObjectExchangeServer_stub.h \
            LocalDevice/QBtLocalDevice_stub.h \
            Connection/ObjectExchange/Client/QBtObjectExchangeClient_stub.h \
            Connection/SerialPort/Client/QBtSerialPortClient_stub.h \
            Connection/SerialPort/Server/QBtSerialPortServer_stub.h \
            ServiceAdvertiser/QBtServiceAdvertiser_stub.h \
            ServiceDiscoverer/QBtServiceDiscoverer_stub.h \
            DeviceDiscoverer/QBtDeviceDiscoverer_stub.h
        SOURCES += Connection/ObjectExchange/Server/Impl/QBtObjectExchangeServer_stub.cpp \
            LocalDevice/Impl/QBtLocalDevice_stub.cpp \
            Connection/ObjectExchange/Client/Impl/QBtObjectExchangeClient_stub.cpp \
            Connection/SerialPort/Client/Impl/QBtSerialPortClient_stub.cpp \
            Connection/SerialPort/Server/Impl/QBtSerialPortServer_stub.cpp \
            ServiceAdvertiser/Impl/QBtServiceAdvertiser_stub.cpp \
            ServiceDiscoverer/Impl/QBtServiceDiscoverer_stub.cpp \
            DeviceDiscoverer/Impl/QBtDeviceDiscoverer_stub.cpp
    }
}

}}}

My application pro file:


{{{
# -------------------------------------------------
# Project created by QtCreator 2011-06-16T14:35:19
# -------------------------------------------------
QT += core \
    gui \
    xml
TARGET = MyBluetoothApp
TEMPLATE = app
SOURCES += main.cpp \
    Home.cpp \
    Client.cpp
HEADERS += Home.h \
    Client.h
FORMS += Home.ui
CONFIG += mobility
MOBILITY =
symbian {
    INCLUDEPATH += /epoc32/include/QBluetooth

    LIBS += -lQBluetooth
    TARGET.UID3 = 0xe8e420c6

    TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData

    customrules.pkg_prerules = ";QBluetooth" \
    "@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_template.sisx\",(0x2003328D)"\
    " "
    DEPLOYMENT += customrules
    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 \
        0x800000
}
win32 {
    LIBS += -LC:\\Carbide\\QuteMessenger\\QBluetooth\\win32\\bin\\Debug \
        -lQBluetooth_0x2003328D
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/BTTypes
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/LocalDevice/
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/ServiceAdvertiser
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/ServiceDiscoverer
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/DeviceDiscoverer
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/Connection/ObjectExchange/Server/
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/Connection/ObjectExchange/Client/
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/Connection/SerialPort/Client/
    INCLUDEPATH += QBluetooth/win32/include/QBluetooth/Connection/SerialPort/Server/
    HEADERS += QBluetooth/win32/include/QBluetooth/QBluetooth.h
}
symbian::LIBS += -lapgrfx \
    -lws32 \
    -lcone \
    -leikcore \
    -lavkon
RESOURCES += MyBluetoothAppResource.qrc
}}}

Also I tried to build the Library using protected range UUID i.e. 0x20048E3F, 0x20048E41, which I got from symbian signed but result is same.

Any suggestions on this...

