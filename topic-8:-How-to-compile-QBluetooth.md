----------------------------------------------------------------------------
Author:     fkizewski
Created:    12/15/10 14:00:59
Moderators:
----------------------------------------------------------------------------

Hi,

I've just following your tutorial to compile the QBluetooth library, but in Carbide when building, i've some error like:
{{{
string: No such file or directory
expected init-declaration before '<' token
...
}}}
I've install this on my Windows Vista or Windows 7:
- 3rd Edition, FP2 v1.1 (455 MB)
- Bluetooth Engine API for 3rd Edition FP2
- Qt for symbian 4.6.3 (ftp://ftp.qt.nokia.com/qt/source/qt-...urce-4.6.3.exe)
- Carbide++ (http://www.forum.nokia.com/info/sw.n...119a2b4cb.html)

Can you please explain me where i've make a mistakes? And why you do not purpose to download directly the library (it's so much simply for all user want to use this library) ?
Have you a new tutorial for compile it?

I've try to compile it on Qt creator 2.0.1, and i've another errors.

Thanks a lot, Fabrice

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 02:17:43
Moderators:
----------------------------------------------------------------------------

Hello fkizewski.

Every error comes with a line number, where it occurred.
Can you please send some more information like for example the console output of the compiler?

Till then, i see you haven't installed the OpenC/C++ plug-in. Download and install it from here.
http://www.forum.nokia.com/info/sw.nokia.com/id/91d89929-fb8c-4d66-bea0-227e42df9053/Open_C_SDK_Plug-In.html
May be, after installing OpenC/C++ it would be wise of you to reinstall the Qt for Symbian and then give the compiling an other try.

Regards.

----------------------------------------------------------------------------
Author:     tech.spvn
Created:    12/16/10 02:51:48
Moderators:
----------------------------------------------------------------------------

Hey guy! I've tried compiled bluetooth library by Carbide, I've follow a member of forum, but in carbide when building, I've some error: "BLDMAKE ERROR: Can't find "\Symbian....\BLD.INF" And, this error at follow image:
[[Image(http://i1039.photobucket.com/albums/a477/Go_Ahead/error1.jpg)]]
Can you please explain me where i've make a mistakes?
thanks alot!

----------------------------------------------------------------------------
Author:     fkizewski
Created:    12/16/10 13:41:11
Moderators:
----------------------------------------------------------------------------

Hi favoritas37,

Thanks a lot for your help, now the library is successfull compiled.
For anyone needs to compile them, there is the processus i make (On Windows Vista):
{{{
Install 3rd Edition, FP2 v1.1 (455 MB)
Install the OpenC/C++ plug-in
Add Bluetooth Engine API for 3rd Edition FP2
Qt for symbian 4.6.3
Carbide++
}}}

But now, for include it in my project, i make this? Can you confirm it!
I put the file: QBluetooth_0x2003328D.dso in my SDK folder (in my case: \NokiaQtSDK\Symbian\SDK\epoc32\release\armv5\lib\, because it's on another computer on Windows 7)
In my .pro file i add this line:

{{{
LIBS += -lQBluetooth_0x2003328D

symbian {

    INCLUDEPATH += /epoc32/include/QBluetooth

    TARGET.CAPABILITY = LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData \
        WriteDeviceData

    addFiles.sources = QBluetooth_0x2003328D.dll
    addFiles.path = !:\sys\bin
    DEPLOYMENT += addFiles
}
}}}

And in my class that need to use QBluetooth, i put: #include <QBluetooth/QBluetooth.h>

That's all?

Thanks in advance, regards Fabrice

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 13:50:16
Moderators:
----------------------------------------------------------------------------

No you don't only need QBluetooth_0x2003328D.dso.

You also need

 * epoc32\include\QBluetooth folder that contains all the header files.
 * epoc32\release\gcce\udeb (or urel, debug / release respectively) QBluetooth_0x2003328D.dll and maybe QBluetooth_0x2003328D.dll.map but for that i am not sure.

Generally speaking, the best thing you can do is, start a search in the epoc32 root folder of the Sdk that you are using "\NokiaQtSDK\Symbian\SDK\epoc32\" and search for QBluetooth. Copy all the results that you will have...its the easies way.

Furthermore, in the program that will use the library you have to put #include <QBluetooth.h> since you have previously added the path for the headers in the include path.

----------------------------------------------------------------------------
Author:     fkizewski
Created:    12/21/10 12:52:29
Moderators:
----------------------------------------------------------------------------

Hi

I'm just posting here, for make a thanks to '''favoritas37''', to help me to compile the library.
I'm now using the library in my project. If i've any question i'm coming into discussions of this project.

Thanks again for your disponibilty, regards Fabrice

