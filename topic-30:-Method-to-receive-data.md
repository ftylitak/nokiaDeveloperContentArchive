Topic #30 - Method to receive data!
----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    03/17/11 23:28:02
Moderators:
----------------------------------------------------------------------------

Hi all! I'm developing a project for Heart care system. For that, I need to read data from a sports utility through bluetooth RFCOMM interface, but I do not find any functions like "receiveData" in either SerialPort server or SerialPort client class implementations. The connection with that device is in simplex mode alone, it neither accepts any incoming data.

So please help me. Any advice would be greatly appreciated.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 15:10:41
Moderators:
----------------------------------------------------------------------------

Since you are using the binaries provided, the .pro file that you posted is not correct.

This is how it should be:

{{{
#-------------------------------------------------
#
# Project created by QtCreator 2011-01-22T14:51:27
#
#-------------------------------------------------
QT       += core gui sql webkit network
TARGET = iHeartCare_SystemV2
TEMPLATE = app
RESOURCES = iHeartCare_SystemV2.qrc

SOURCES += main.cpp\
        mainwindow.cpp
HEADERS  += mainwindow.h
FORMS    += mainwindow.ui
CONFIG += mobility
MOBILITY =

symbian {
    TARGET.UID3 = 0xeac0c8a3

    # include this for Qt Creator to find the Symbian SDK  and QBluetooth headers
    INCLUDEPATH += $$EPOCROOT\epoc32\include
    INCLUDEPATH += $$EPOCROOT\epoc32\include\QBluetooth

    # this says that we are going to use the QBluetooth library
    LIBS += -lQBluetooth

    TARGET.CAPABILITY += LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData \
        WriteDeviceData \
        ReadDeviceData
    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 0x800000
    ICON = Icon.svg

    customrules.pkg_prerules  = \
    ";QBluetooth" \
    "@\"$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_selfsigned.sisx\",(0xA003328D)"\
    " "

    DEPLOYMENT += customrules
}
}}}
Make sure, before building your application, that you copied the binary files provided (bin\Symbian_S60_3rd_FP1\epoc32 folder) in your S60 SDK epoc32 root folder.

Now you should be ready.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 15:24:49
Moderators:
----------------------------------------------------------------------------

I forgot to answer about the receive data method...

There is no receive data method because in our implementation, the data transmission is asynchronous. So you don't have to call receiveData in order to get the incoming data. Instead you connect to the signal [http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html#aa5d793a84a4dde54de51a29b83dac546 dataReceived]and as soon as something is send to you, you will be informed.

This is a much easier way than calling receiveData whenever you want to handle the incoming data.

:)

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    03/18/11 19:18:16
Moderators:
----------------------------------------------------------------------------

Thank you all for the quick replies and help! Actually I have tried to embed the dll file provided in the project site but now I'm gonna try the method you said! Thanks!

Hi favoritas37, actually I don't think that the device will automatically write the data to the SerialPort connection, so I may not receive the data. But it will worth a try! Thanks again!

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    03/18/11 19:37:15
Moderators:
----------------------------------------------------------------------------

Sir! I have packaged the self signed file as you said, but I thin the problem is with the class QBtSingleDeviceSelectorUI. Because whenever I have built the application with these lines only I get the error. '''Unable to execute file for security reasons'''
{{{
QBtSingleDeviceSelectorUI *btSelectUi = new QBtSingleDeviceSelectorUI;
    btSelectUi->show();
}}}

But after building it without these lines, I can run the app normally.

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    03/18/11 21:49:01
Moderators:
----------------------------------------------------------------------------

Sir after removing some unwanted capabilities it works fine. Thanks you!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/19/11 12:09:50
Moderators:
----------------------------------------------------------------------------

Can you please say which are the unwanted capabilities in case someone experiences the same problem?

----------------------------------------------------------------------------
Author:     ravi4frenz
Created:    03/19/11 14:07:43
Moderators:
----------------------------------------------------------------------------

The unwanted capabilities are ReadDeviceData, WriteDeviceData. I have used only the capabilities used in QuteMessenger now and it works fine.

