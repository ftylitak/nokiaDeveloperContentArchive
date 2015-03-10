Topic #41 - Qbluetooth : "undefined reference to"
----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    08/28/11 23:50:49
Moderators:
----------------------------------------------------------------------------

Hi,
sorry for my english, i'm italian.
I'm trying to use this library but i have some problems, on symbian all works fine but when i try to build the same project for desktop i get the undefined reference for all the function of the library i used.
i putted the headers in C:\QBluetooth ad the QBluetooth_0x2003328D.dll and QBluetooth_0x2003328D.lib in C:\QBluetooth\Libs\
so i insert the following lines in the pro file :

INCLUDEPATH += C:\QBluetooth
LIBS += C:\QBluetooth\Libs\QBluetooth_0x2003328D.dll

it seams is a problem white the dll... i downloaded already compiled but i think it doesn't matter...
thanks to anyone who will help me :)

----------------------------------------------------------------------------
Author:     favoritas37
Created:    08/29/11 09:46:11
Moderators:
----------------------------------------------------------------------------

The problem is this line:
LIBS += C:\QBluetooth\Libs\QBluetooth_0x2003328D.dll

change it with this one:
LIBS += -lC:\QBluetooth\Libs\QBluetooth_0x2003328D.dll

:)

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    08/29/11 13:33:22
Moderators:
----------------------------------------------------------------------------

thank you for the response.

i don't know exactly what the .lib file is , so i included the .dll :D

i replace that line but now i get this error  :-1: error: cannot find -lC:\QBluetooth\Libs\QBluetooth_0x2003328D

if i understood what you say in the second PS i putted the .dll also here : C:\Programmi\OnlyASymbyanTest-build-desktop\debug (onlyASymbyanTest is only the name of the project)

i'm working on this problem from several weeks my mind is melting :P

----------------------------------------------------------------------------
Author:     favoritas37
Created:    08/29/11 14:52:53
Moderators:
----------------------------------------------------------------------------

[Option number 1:]

Replace with this one:

{{{
LIBS += -LC:/QBluetooth/Libs \
              -lQBluetooth_0x2003328D
}}}
[Option number 2:]

The simplest thing you could is to put the QBluetooth folder in the folder of your project.

So you will place the QBluetooth folder in C:\Programmi\OnlyASymbyanTest.

Then in the .pro file you will write:

{{{
INCLUDEPATH += QBluetooth
LIBS += -lQBluetooth/Libs/QBluetooth_0x2003328D
}}}
Generally speaking it would be good to read this [https://projects.developer.nokia.com/qbluetooth/wiki/buildIncludeWindows#Includingthelibrary wiki page].

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    08/29/11 15:31:16
Moderators:
----------------------------------------------------------------------------

i said a wrong thing but i fixed it (i fixed the paths) ... the project name is : WillWork

however with the firs option i get the undefined reference error.
whit the second i get :-1: error: cannot find -lQBluetooth/Libs/QBluetooth_0x2003328D.

i already read the wiki page you suggest a lot of times  but i didn't managed to get i working so i write on this forum.

probably i'm doing something wrong... i try to post the pro file... perhaps you may see something wrong..


{{{
symbian:TARGET.UID3 = 0xE552E1A7

symbian:TARGET.CAPABILITY += NetworkServices LocalServices UserEnvironment

win32{

INCLUDEPATH += QBluetooth
LIBS += -lQBluetooth/Libs/QBluetooth_0x2003328D

}

symbian {
LIBS += -lQBluetooth
INCLUDEPATH += $$EPOCROOT\epoc32\include\QBluetooth
}

HEADERS += \
    mainwindow.h

SOURCES += \
    mainwindow.cpp \
    main.cpp


}}}

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    08/29/11 16:22:16
Moderators:
----------------------------------------------------------------------------

it seams it's already installed... the box is checked so it is installed...

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    08/30/11 14:20:31
Moderators:
----------------------------------------------------------------------------

I also tried on another pc even if the MSVC 2008 is installed the tool chain is not set... maybe i have to install something manually?

----------------------------------------------------------------------------
Author:     pablomtz
Created:    09/09/11 01:00:32
Moderators:
----------------------------------------------------------------------------

Hi Rajeevsahu!


Could you solved this? ..I think i'm having the same problem!


I need to receive hexa data, but I'm receive "3f" if the data is bigger than "7f" !

thanks!

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    09/18/11 20:31:28
Moderators:
----------------------------------------------------------------------------

Hi favoritas37 i finally managed to fix the problem, i installed Visual C++ 2008 Express and now i have "Microsoft Visual c++ Compiler 9.0(x86) as toolchain.
Now if i build the project all work fine, but if i run the project it exit with code -1073741515.
if i remove all the bluetooth code from the project (like object declaration and other stuf) it works.

thank you in advance

----------------------------------------------------------------------------
Author:     favoritas37
Created:    09/19/11 13:59:51
Moderators:
----------------------------------------------------------------------------

Where exactly are you having errors. Can you run the project step by step to see which exact command gives you that error?

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    09/19/11 15:25:48
Moderators:
----------------------------------------------------------------------------

if i try to use the debug i get 2 messages...
the first say Warning : the preferred debugger engine for debugging binaries of type 'x86-windows-mvsc2008-pe-32bit' is not aviable, the debugger 'gdb engine' will be used as fallback
the second is an error : An exception was triggered : exception at 0x7c964ed1,code: 0x0000135:Dll not found,flags=0x0. During srartup program exited with code 0x0000135.

however i get error only if a try to create any object of the  Qbluetoot library...

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    09/19/11 15:59:40
Moderators:
----------------------------------------------------------------------------

i tried to run the application both on windows7 64bit  and xp 32bit(on a vm) but the error is the same
maybe the Visual C++ 2008 Express  i installed in not correct to compile and link the program... i don't know...  i tryed for 2 week to add that damn tool chain :)

----------------------------------------------------------------------------
Author:     fatshadyDEV
Created:    09/29/11 01:00:40
Moderators:
----------------------------------------------------------------------------

Thank you favoritas37 !
i finally managed to get it working!
i compile the library but i had the same problems... finally i realized that i haven't installed the bluesoleil drivers! so i downloaded the program bluesoleil 8.0, the program installed the drivers and the needed library(the error code i had : -1073741515 means that some dll are missing in my case the bluesolei dlls) now it works!!!
now i have a second question... the program is a 15day trial... but i don't understand if also the drivers are under the trial license... i will use my program for non commercial use... i have to buy the bluesoleil program even if i don't use the program but only the drivers?

thank you!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    09/29/11 12:11:18
Moderators:
----------------------------------------------------------------------------

Unfortunately that is the catch with windows implementation. When the trial ends you can use the bluetooth to transfer at most 2MB per day at the free version.

FYI: The supported Bluesoleil drivers are from version 6.x and on.

