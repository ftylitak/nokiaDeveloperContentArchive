Topic #34 - "Fearute not supported error" with own built QBluetooth library!
----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    04/03/11 18:07:26
Moderators:
----------------------------------------------------------------------------

I have developed an application using QBluetooth and QMobility, with Location capability! But when I run it it shows '''Unable to execute file for security reasons!''', thus I came to know that QBluetooth self signed (for S60 3rd FP1) does not have '''Location''' capabilities. It can be found when installing the library it does not shows Location capability either.

''I don't know why there is another sisx package named '''QBluetooth_no_location.sisx'''''

Thus I have tried to build it by myself '''with Location capabilities''' and I successfully built it and installed it to my phone (E63 S60 V3 FP1). But when I try to open my application it shows '''Feature not supported error'''.

I have successfully used and developed another application with the library without any problem. The problem comes only when the application needs '''Location capabilities'''.

I request someone here to help. I have read a discussion with an identical condition but it didn't work in my case.


{{{
uid3 = 0x8003328D

QT += core

TEMPLATE = lib
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


    TARGET.UID3 = 0x8003328D

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

    TARGET.CAPABILITY = All -TCB -AllFiles -DRM #LocalServices NetworkServices ReadUserData UserEnvironment WriteUserData Location ReadDeviceData WriteDeviceData


    for(header, exportheaders.sources):BLD_INF_RULES.prj_exports += "$$header $$deploy.path$$exportheaders.path/QBluetooth/$$basename(header)"

        # add this for Qt Creator to generate a pkg file, it seems to be a bug in the current version (2.0.0)
        addFiles.sources = QBluetooth.dll
        addFiles.path = !:/sys/bin
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


This is my pro file.

I have even changed that UID something out of protected range, but still no use.

help me!!

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    04/04/11 12:33:08
Moderators:
----------------------------------------------------------------------------

Sir, thanks for the reply. Actually my Device supports GPS but only through a external GPS module. I have already done some Location based applications using Qt successfully. Whenever I try to use the GPS functions the mobile will ask us to select an external device. So I see no problem with the device.

Sir,  Can you please provide me a QBluetooth library with '''Location capabilities''' please. I think I have done something wrong with the compilation. May be, because I have used the .dso files from S60 V5 SDK. I don't know how to build it particularly for S60 V3 FP1.

So please provide me package with almost all Capabilities, If you need I have one certificate from OPDA for my device. Please help me. I have to submit my project within April 6th.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    04/04/11 13:00:49
Moderators:
----------------------------------------------------------------------------

The package you request already exists under the folder ''bin/Symbian_S60_3rd_FP1/epoc32/!InstallToDevice/QBluetooth.sis''. Its capabilities are:

{{{
TARGET.CAPABILITY = All -TCB -AllFiles -DRM
}}}
To get it, checkout the SVN repository and you are ready. Just remember that you have to sign it.

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    04/04/11 13:46:42
Moderators:
----------------------------------------------------------------------------

Sir, so far I have used that file only but whenever I try to use an application it shows unable to execute file for security reasons. This is that application's pro file. Another application using that same DLL works fine but that capabilities are

TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData

.
My applications shows the '''Unable to execute file for security reasons''' only when Location capability is added to the profile. But when I remove that it does not shows any error and the application crashes too (since I use '''Location related functions like GPS''''.





{{{
#-------------------------------------------------
#
# Project created by QtCreator 2011-03-31T18:49:16
#
#-------------------------------------------------

QT       += core gui network

TARGET = HeartMonitor
TEMPLATE = app


SOURCES += main.cpp\
        mainwindow.cpp

HEADERS  += mainwindow.h \
    MakeCall.h

FORMS    += mainwindow.ui

CONFIG += mobility
MOBILITY = systeminfo location

LIBS += -lQBluetooth

symbian {
    TARGET.UID3 = 0x89756940
    TARGET.CAPABILITY =
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData \
        Location

    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 0x800000
    LIBS += -letel3rdparty \
            -lsendas2

    INCLUDEPATH += $$EPOCROOT\epoc32\include
    INCLUDEPATH += $$EPOCROOT\epoc32\include\QBluetooth
    ICON = Icon.svg

}

RESOURCES += \
    ImageResource.qrc

}}}


[Sir please note that, '''When installing the QBluetooth_selfsigned.sisx, the phone will show the capabilities that the installing application has. In that list also "The Location capabilities are not shown."''']
Look at the installation screen shot below, It does not shows Location capabilities.

[[Image(http://img811.imageshack.us/img811/8199/scr000002f.jpg)]]

[[Image(http://img14.imageshack.us/img14/7905/scr000001g.jpg)]]


----------------------------------------------------------------------------
Author:     favoritas37
Created:    04/04/11 14:27:01
Moderators:
----------------------------------------------------------------------------

Be careful, i didn't mention you should use '''QBluetooth_selfsigned.sisx''' but '''QBluetooth.sis''' instead.

These are two different things. The '''QBluetooth.sis''' needs signing in order to be used and has all the possible capabilities.

To include the following lines to your .pro file to use it.

{{{
symbian
{
        customrules.pkg_prerules  = \
        ";QBluetooth" \
        "@\"$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth.sis\",(0xA003328D)"\
        " "

        DEPLOYMENT += customrules
}
}}}

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    04/04/11 19:09:53
Moderators:
----------------------------------------------------------------------------

Sorry sir, I forgot to notice that. Thanks for the help. I will try that and post here.

