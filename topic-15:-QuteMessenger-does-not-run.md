Topic #15 - QuteMessenger does not run
----------------------------------------------------------------------------
Author:     zfreq
Created:    01/27/11 12:03:20
Moderators:
----------------------------------------------------------------------------

I am able to compile (GCCE) QBluetooth library with QtCreator, there are a lot of warnings (_STLP_NO_BAD_ALLOC redefined), but it compiles okay (i think) because no errors. I also tried installing it on device and it installs okay.

However, I am not able to compile (GCCE) QuteMessenger, there is one error:

:: error: No rule to make target `\S60\devices\S60_5th_Edition_SDK_v1.0\epoc32\release\armv5\LIB\QBluetooth.dso', needed by `\S60\devices\S60_5th_Edition_SDK_v1.0\epoc32\release\gcce\urel\QuteMessenger.exe'.  Stop.

So, apparently the QBluetooth compile does not create the QBluetooth.dso??

If i copy the QBluetooth.dso from the folder that comes with QuteMessenger, i am able to compile the QuteMessenger and install it on the device, but nothing happens when i try to start the application - nothing. It does not run.

Can you help me??

Here are my .pro files:

QBluetooth:

{{{
uid3 = 0xE000D7CC

TEMPLATE = lib
TARGET   = QBluetooth
DEFINES  += BLUETOOTH_LIB

QT += core

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

FORMS +=

RESOURCES +=

symbian {
        # fix for Qt Creator to find the symbian headers
        INCLUDEPATH += $$EPOCROOT\epoc32\include

    deploy.path = $$EPOCROOT
    exportheaders.sources = $$PUBLIC_HEADERS
    exportheaders.path = epoc32/include

    INCLUDEPATH += $$deploy.path$$exportheaders.path/QBluetooth/

    INCLUDEPATH += $$deploy.path$$exportheaders.path

    TARGET.UID3 = 0xE000D7CC

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
        -lapmime \
        -lcommonui \
        -lplatformenv \
        -lcharconv

    LIBS += $$BT_PLUGIN_LIB

    TARGET.CAPABILITY = LocalServices NetworkServices ReadUserData UserEnvironment WriteUserData WriteDeviceData

    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 0x800000

    for(header, exportheaders.sources):BLD_INF_RULES.prj_exports += "$$header $$deploy.path$$exportheaders.path/QBluetooth/$$basename(header)"

        # add this for Qt Creator to generate a pkg file, it seems to be a bug in the current version (2.0.0)
        addFiles.sources = QBluetooth.dll
        addFiles.path = !:\sys\bin
        DEPLOYMENT += addFiles
}


else {
    win32 {
        LIBS += -lBlueSoleil_SDK_2.0.5/bin/BsSDK

                //deploy.path = $$EPOCROOT
                /*INCLUDEPATH += $$deploy.path$$exportheaders.path/QBluetooth/
                INCLUDEPATH += $$deploy.path$$exportheaders.path*/

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
            WinSerialPort/Tserial_event.h
        SOURCES += Connection/ObjectExchange/Server/Impl/QBtObjectExchangeServer_win32.cpp \
            LocalDevice/Impl/QBtLocalDevice_win32.cpp \
            Connection/ObjectExchange/Client/Impl/QBtObjectExchangeClient_win32.cpp \
            Connection/SerialPort/Client/Impl/QBtSerialPortClient_win32.cpp \
            Connection/SerialPort/Server/Impl/QBtSerialPortServer_win32.cpp \
            ServiceAdvertiser/Impl/QBtServiceAdvertiser_win32.cpp \
            ServiceDiscoverer/Impl/QBtServiceDiscoverer_win32.cpp \
            DeviceDiscoverer/Impl/QBtDeviceDiscoverer_win32.cpp \
            WinSerialPort/Tserial_event.cpp

                /*exportheaders.sources = $$PUBLIC_HEADERS


                for(header, exportheaders.sources){
                        DESTDIR += "$$header $$DESTDIR\include\$$basename(header)"
                }*/
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

QuteMessenger:

{{{
TEMPLATE = app

TARGET = QuteMessenger

QT += core \
    gui

HEADERS += QChatWidgetClient.h \
    QChatWidgetServer.h \
    QChatWidget.h \
    QuteMessenger.h \
    QInputBox.h

SOURCES += QChatWidgetClient.cpp \
    QChatWidgetServer.cpp \
    QChatWidget.cpp \
    main.cpp \
    QuteMessenger.cpp \
    QInputBox.cpp

FORMS += chatWidget.ui \
    QuteMessenger.ui

RESOURCES += imageResources.qrc

CONFIG   += mobility

LIBS += -lQBluetooth

symbian {

        INCLUDEPATH += /epoc32/include/QBluetooth

    TARGET.UID3 = 0xE8D3B2DA

    LIBS += -lQBluetooth \
        -leikcoctl \
        -lavkon \
        -leikcore \
        -leiksrv \
        -lcone

    TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData \
        WriteDeviceData

    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 0x800000

    addFiles.sources = $$(EPOCROOT)Epoc32/release/$(PLATFORM)/$(CFG)/QBluetooth.dll
    addFiles.path = /sys/bin
    DEPLOYMENT += addFiles

  #customrules.pkg_prerules  = \
        #";QBluetooth" \
        #"@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth.sisx\",(0xA003328D)"\
       #" "
        #DEPLOYMENT += customrules
}

win32 {
    LIBS += -l$$(QTDIR)/QBluetooth/bin/QBluetooth \
        -L$$(QTDIR)/QBluetooth/bin
    INCLUDEPATH += ../QBluetooth/
        INCLUDEPATH += ../QBluetooth/BTTypes
        INCLUDEPATH += ../QBluetooth/LocalDevice/
        INCLUDEPATH += ../QBluetooth/ServiceAdvertiser
        INCLUDEPATH += ../QBluetooth/ServiceDiscoverer
        INCLUDEPATH += ../QBluetooth/DeviceDiscoverer
        INCLUDEPATH += ../QBluetooth/Connection/ObjectExchange/Server/
        INCLUDEPATH += ../QBluetooth/Connection/ObjectExchange/Client/
        INCLUDEPATH += ../QBluetooth/Connection/SerialPort/Client/
        INCLUDEPATH += ../QBluetooth/Connection/SerialPort/Server/
    HEADERS += ../QBluetooth/QBluetooth.h
}

ICON = val1.svg
}}}

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/27/11 12:23:03
Moderators:
----------------------------------------------------------------------------

The thing is that, using Qt Creator you can build the library but you can't freeze the project. This step is needed so the .dso file is created.

If you take a look at the [https://projects.forum.nokia.com/qbluetooth/wiki wiki page]under "Possible build errors" -> "Compiler errors when modifying the public API (any public method of QBluetooth classes), with Qt Creator" you will find what needs to be done in order to freeze the project.

Now its should work.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/27/11 12:37:18
Moderators:
----------------------------------------------------------------------------

Also, what do you mean by saying "''If i copy the QBluetooth.dso from the folder that comes with QuteMessenger, i am able to compile the QuteMessenger''".

As the QuteMessenger's wiki page says, you have to copy the entire epoc32 folder and paste in the root folder of the Symbian SDK that you use. All the files in there are needed.

----------------------------------------------------------------------------
Author:     zfreq
Created:    01/27/11 12:52:03
Moderators:
----------------------------------------------------------------------------

Yes! It works now!

i hadn't freeze the library so the .bat file thing helps with Qt creator! thank you!

i just meant i copied the .dso file because that was the only one missing but yeah, nevermind that, it works now because i used the .bat thing!

