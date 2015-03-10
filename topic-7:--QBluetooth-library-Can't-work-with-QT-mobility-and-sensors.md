[initial messages not restored]

----------------------------------------------------------------------------
Author:     franky_75
Created:    12/29/10 11:32:47
Moderators:
----------------------------------------------------------------------------

> Do you have a developer certificate?

No, I'm using the Open Signed Online tool: https://www.symbiansigned.com/app/page/public/openSignedOnline.do [[BR]]Isn't it good for using QBluetooth?

> Also, your app needs to include the same capabilities of QBluetooth in the "symbian" part of the app .pro file ` symbian { # the capabilities must match the ones used to build the QBluetooth library.      TARGET.CAPABILITY +=  ... } `

So, you mean I have to put

{{{
    TARGET.CAPABILITY = All -TCB -AllFiles -DRM

}}}
inside my .pro file? Or changing the QBluetooth.pro file to match my app .pro file?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/29/10 16:10:24
Moderators:
----------------------------------------------------------------------------

Using Open Signed Online is just fine for the Capabilities of the library. But you have to remember that Open Signed is for '''test purposes only'''!

Now about the capabilities of the program using QBluetooth...i thing that it can have ANY subset of QBluetooth's capabilities. You are not obliged to have full match of the library's capabilities.

----------------------------------------------------------------------------
Author:     franky_75
Created:    12/29/10 17:19:33
Moderators:
----------------------------------------------------------------------------

> Using Open Signed Online is just fine for the Capabilities of the library. But you have to remember that Open Signed is for '''  test purposes only'''  ! Now about the capabilities of the program using QBluetooth...i thing that it can have ANY subset of QBluetooth's capabilities. You are not obliged to have full match of the library's capabilities.

Yes, I know that Open Signed is for test only: I'm testing the application only on my phone.

This is my .pro file but when I run the application nothing happens

{{{
QT       += core gui

TARGET = BluetoothDiscovery
TEMPLATE = app

INCLUDEPATH += $$EPOCROOT\epoc32\include
INCLUDEPATH += $$EPOCROOT\epoc32\include\QBluetooth

SOURCES += main.cpp\
        widget.cpp

HEADERS  += widget.h

FORMS    += widget.ui

LIBS += -lQBluetooth_0x20037F7A

symbian {
    TARGET.UID3 = 0x20037F7A
    TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData
    addFiles.sources = QBluetooth_0x20037F7A.dll
    addFiles.path = \sys\bin
    DEPLOYMENT += addFiles
}
}}}

----------------------------------------------------------------------------
Author:     lpvalente
Created:    01/04/11 15:07:38
Moderators:
----------------------------------------------------------------------------

Are you building your own version of QBluetooth, or using the binaries?

----------------------------------------------------------------------------
Author:     franky_75
Created:    01/04/11 17:01:47
Moderators:
----------------------------------------------------------------------------

> Are you building your own version of QBluetooth, or using the binaries?
>

I've built the library with Carbide and then linked it into the Qt project.

----------------------------------------------------------------------------
Author:     franky_75
Created:    01/11/11 20:13:04
Moderators:
----------------------------------------------------------------------------

I tested the application using your QBluetooth self signed binaries and it works. So, it's definitely a problem in my library build: any ideas where to look for?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/11/11 21:08:58
Moderators:
----------------------------------------------------------------------------

I have attached a .pro file for you to try.

Download the latest version of the code, use the new .pro file and do the steps necessary for building the library...who knows...

----------------------------------------------------------------------------
Author:     franky_75
Created:    01/12/11 12:36:27
Moderators:
----------------------------------------------------------------------------

Thank you, I'll try. But, where is the attachment?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/12/11 12:40:26
Moderators:
----------------------------------------------------------------------------

It is at the bottom of this page, above the RSS Feed.

----------------------------------------------------------------------------
Author:     franky_75
Created:    01/12/11 13:11:30
Moderators:
----------------------------------------------------------------------------

I'm logged but can't see the attachments, when I click on you link I see this error:

{{{
ATTACHMENT_VIEW privileges are required to perform this operation on Attachment 'QBluetooth.pro' in Topic #5

}}}
I think it's related to my status of Not registered user, but I'm registered to Forum Nokia...I don't understand.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/12/11 13:27:05
Moderators:
----------------------------------------------------------------------------

Ok i will fix that later.

For now, you can get it from here:

https://projects.forum.nokia.com/qbluetooth/files/QBluetooth.pro

I suppose it works.

----------------------------------------------------------------------------
Author:     franky_75
Created:    01/12/11 20:13:47
Moderators:
----------------------------------------------------------------------------

Thank you! It works now.

I also had to change the capabilities in QBluetooth.pro to


{{{
    TARGET.CAPABILITY = All -TCB -AllFiles -DRM -CommDD -MultimediaDD -NetworkControl -DiskAdmin

}}}
because the Open Signed Online tool cannot sign those capabilities.