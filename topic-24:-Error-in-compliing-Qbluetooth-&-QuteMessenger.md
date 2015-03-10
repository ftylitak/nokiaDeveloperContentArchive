Topic #24 - Error in compliing Qbluetooth & QuteMessenger
----------------------------------------------------------------------------
Author:     alex_hui
Created:    03/02/11 17:10:25
Moderators:
----------------------------------------------------------------------------

Hi all,

This is my first post here and I hope someone can help to to solve this problem cause I was stuck in this problem for so long, it really makes me frustrating in my developing process :(

BTW, I have problem in successfully compile both the QBluetooth library and  QuteMessenger application.
I tried many different methods and had follow  different instruction but still can't success in it, I would like to know if the link I posted below give the correct steps in compiling Qbluetooth & QuteMessenger

http://cacovsky.wordpress.com/2011/01/15/using-the-qbluetooth-library-with-symbian-s60-5th-edition/

Case 1. I tried uninstall all related software, application and follow the steps given in the above but still can't success. I tried it in windows Vista with both S60-5th SDK & symbian ^3 SDK installed, but the errors show "Unable to acquire a device for port ". It appears to be in use. What's wrong with it?

Case 2. I tried another time in win 7 with the same setting as the link I posted.
A. For compiling QBluetooth, the error shows

"WARNING: c:\Users\51177407\Documents\Project\QBluetooth\source\QBluetooth.pro:149: Unescaped backslashes are deprecated.

c:\....\Documents\Project\QBluetooth\source\QBluetooth.pro:174: Parse Error ('}*/')

c:\....\Documents\Project\QBluetooth\source\QBluetooth.pro:174: Unterminated conditional block at end of file

Error processing project file: C:/..../Documents/Project/QBluetooth/source/QBluetooth.pro

The process "c:/nokiaqtsdk/simulator/qt/mingw/bin/qmake.exe" exited with code %2.
Error while building project QBluetooth (target: Qt Simulator)
When executing build step 'qmake'"

B.  For compiling QuteMessenger , it shows lots of 118 errors like
"BtServicehas not been declared" (other errors are similar, all relating to bluetooth service)

In Case 2A&B, i use Qt Creator (Nokia Qt SDK) to compile it, is that the problem?

Looking forward to your help and really need that urgently. Thank you.

----------------------------------------------------------------------------
Author:     alex_hui
Created:    03/02/11 18:45:04
Moderators:
----------------------------------------------------------------------------

> Hi all, This is my first post here and I hope someone can help to to solve this problem cause I was stuck in this problem for so long, it really makes me frustrating in my developing process :( BTW, I have problem in successfully compile both the QBluetooth library and  QuteMessenger  application. I tried many different methods and had follow  different instruction but still can't success in it, I would like to know if the link I posted below give the correct steps in compiling Qbluetooth & QuteMessenger http://cacovsky.wordpress.com/2011/01/15/using-the-qbluetooth-library-with-symbian-s60-5th-edition/ Case 1. I tried uninstall all related software, application and follow the steps given in the above but still can't success. I tried it in windows Vista with both S60-5th SDK & symbian ^ 3 SDK installed, but the errors show "Unable to acquire a device for port ". It appears to be in use. What's wrong with it? Case 2. I tried another time in win 7 with the same setting as the link I posted.  A. For compiling QBluetooth, the error shows "WARNING: c:\Users\51177407\Documents\Project\QBluetooth\source\QBluetooth.pro:149 : Unescaped backslashes are deprecated. c:\....\Documents\Project\QBluetooth\source\QBluetooth.pro:174 : Parse Error ('}*/') c:\....\Documents\Project\QBluetooth\source\QBluetooth.pro:174 : Unterminated conditional block at end of file Error processing project file: C:/..../Documents/Project/QBluetooth/source/QBluetooth.pro The process "c:/nokiaqtsdk/simulator/qt/mingw/bin/qmake.exe " exited with code %2. Error while building project QBluetooth (target: Qt Simulator) When executing build step 'qmake'" B.  For compiling QuteMessenger  , it shows lots of 118 errors like  "BtServicehas  not been declared" (other errors are similar, all relating to bluetooth service) In Case 2A&B, i use Qt Creator (Nokia Qt SDK) to compile it, is that the problem? Looking forward to your help and really need that urgently. Thank you.^

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/02/11 18:53:52
Moderators:
----------------------------------------------------------------------------

What do you mean by reposting what you wrote above?

You mean that you followed the correct steps and you ended up here?

Also i will repeat that it is recommend to use the latest version of the code from the SVN. I underline that because the QBluetooth.pro file doesn't have any '}*/' string sequence in it.

Finally, is it mandatory for you to compile the library? If yes, you want to generate the .sis file which you will use, or you will use directly the generated files in the epoc root folder of the Symbian SDK?

Please clarify these for me to be able to help you.

----------------------------------------------------------------------------
Author:     alex_hui
Created:    03/03/11 08:40:50
Moderators:
----------------------------------------------------------------------------

I'm using Windows 7 ultimate 32 bits and still can't successfully compile both the QBluetooth library and QuteMessenger, the following is the list of software installed on my machine:

ActivePerl 5.12.2 Build 1202
MinGW-gcc440_1
S60 5th Edition SDK
Qt for Symbian by Nokia v4.7.1 (S60 Opensource)
Nokia Qt SDK(Qt Creator 2.1.0)
Extensions Plug-in for S60 3rd Edition SDK for Symbian OS, Feature Pack 2, v1.3

In fact, I'm using the Qt Creator in Nokia Qt SDK to compile the two applications, is that correct?
And is there any possibility my project put in a wrong location which make the compiling fail?
I currently put the two project in "My document" folder to compile, which is the same as other project(other project compile successfully). is there anything wrong of the location?

Thank for your help.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/03/11 15:16:35
Moderators:
----------------------------------------------------------------------------

First thing, i would suggest not to place the project folder to "My documents". Select a directory path which doesn't contains spaces (they are not identified by the !ActivePerl).

Secondly, download again the source code because i had made a small fix.

Thirdly download and install [http://www.forum.nokia.com/info/sw.nokia.com/id/91d89929-fb8c-4d66-bea0-227e42df9053/Open_C_SDK_Plug-In.html OpenC/C++].

Then copy the Bluetooth Plugin files in the Symbian SDK you are going to use (i would suggest not to use the one in Nokia Qt SDK because it has some missing files but the S60 5th Edition SDK instead).

Select the above SDK (in Qt Creator: Projects -> Qt Version -> S60_5th_Edition_SDK_v1.0(Qt4.7.1)) and build the library with Debug (or Release) configuration (in Qt Creator: Projects -> Edit build configuration -> select one ).

Now you should be fine.

Also if anything goes wrong, please post the console output as well or else i wont be able to help...

A small comment on "#include <QBluetooth>": it is logical it didn't wor, the library was not build successfully so how could it work?

Waiting for you answer...[[BR]]

----------------------------------------------------------------------------
Author:     alex_hui
Created:    03/04/11 04:44:58
Moderators:
----------------------------------------------------------------------------

In my windows 7 it seems I was finally successfully compile the QBluetooth library as the result of the compilation is a package called QBluetooth_0x2003328D.sis, and it  is in the folder QBluetooth\source\. Is that mean the QBluetooth library compile successfully?

But after that there is a message pop-up show "Could not find the executable, please specify one" and ask me to fill in some executable information. What is that mean?

Moreover, if this means the library compile success, what should I do next to merge the library to my own application so that I can use in my own way?

Thank you so much for your help again.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/05/11 16:01:33
Moderators:
----------------------------------------------------------------------------

To begin with, if the library was successully compiled and the .sis file was created then you are done from the part of compiling the library.

The message window "Could not find the executable, please specify one" is in fact very logical if you click the "Run" button instead of "Build". When hitting "Run" it builds the project and then tries to execute it. In our case, the compilation creates only the library (think of it as a DLL), there is no executable but only the library files ready to be used in an application.

The library can only be compiled on Symbian Device configuration, not Qt Simulator, because it is the only configuation that has all the necessary files as well as the Bluetooth hardware support.

Next step is how to use the produced library files. In the wiki there is plenty of information about how to do that. Check out [https://projects.forum.nokia.com/qbluetooth/wiki#Includingthelibrary here]. Also the following article could be usefull:

 * [http://wiki.forum.nokia.com/index.php/Discovering_Bluetooth_devices_with_the_QBluetooth_library Discovering Bluetooth devices with the QBluetooth library]
 * [http://wiki.forum.nokia.com/index.php/Discovering_nearby_Bluetooth_services_with_the_QBluetooth_library Discovering nearby Bluetooth services with the QBluetooth library]
 * [http://wiki.forum.nokia.com/index.php/QuteMessenger_-_Bluetooth_Chat QuteMessenger - Bluetooth Chat]

The first two links use the produced library files directly and the third uses the .sis file (Shared DLL). It's your choise...

----------------------------------------------------------------------------
Author:     alex_hui
Created:    03/18/11 05:35:31
Moderators:
----------------------------------------------------------------------------

Hi, it's me back again.

I found that there's an error with I use the library,
No rule to make target `\NokiaQtSDK\Symbian\SDK\epoc32\release\armv5\LIB\QBluetooth_0x2003328D.dso', needed by `\NokiaQtSDK\Symbian\SDK\epoc32\release\gcce\udeb\start.exe'.  Stop.

What's up with that? Can anyone help me on that? Thanks.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/18/11 15:47:14
Moderators:
----------------------------------------------------------------------------

As it seems you haven't compiled successfully the QBluetooth library, that is why the `QBluetooth_0x2003328D.dso is not found.`

The library can't be build for NokiaQtSDK\Symbian because the SDK provided by the Nokia Qt SDK package is missing some files.

The alternative, if you really really want to use Nokia Qt SDK, compile the library, see which files are missing and copy them from a normal S60 SDK (i thing they are: btextnotifiers.dso and irobex.dso but i am not sure).

