Topic #36 - "No rule to make target" error
----------------------------------------------------------------------------
Author:     zstapic
Created:    04/06/11 12:52:29
Moderators:
----------------------------------------------------------------------------

Hi everyone,

i have spent a days now trying to integrate QBluetooth capability in my Qt for Symbian application.
As I have had lots of problems, i firstly wanted to try to build your source code and the source code of the example ([http://wiki.forum.nokia.com/index.php/QuteMessenger_-_Bluetooth_Chat]), but I failed :(

So this is what I have done:
1. I installed everything that is written here [https://projects.forum.nokia.com/qbluetooth/wiki/buildIncludeSymbian]
    - this includes the environment (Perl, JRE), SDK S60 5th Edition, Open C/C++ Plugin, BluetoothEngineApi, Nokia Qt SDK 1.0
2. I changed the SDK in Qt creator as shown here:
 [[Image(http://dl.dropbox.com/u/242610/3.png)]]

But after running the build I get the "No rule to make target" error. I am pretty much sure I have done some wrong configuration, but I can't figure it out what.
I also tried to build the example QuteMessenger and I got the same error!

Does anyone have a clue of what is wrong with my configuration and why do I get the error shown here:
[[Image(http://dl.dropbox.com/u/242610/4.png)]]

Also, why does it say that there is no Qt installed here:
[[Image(http://dl.dropbox.com/u/242610/2.png)]]

Thank you very much in advance!
Zlatko

----------------------------------------------------------------------------
Author:     favoritas37
Created:    04/07/11 13:39:22
Moderators:
----------------------------------------------------------------------------

This is your mistake -> Nokia Qt SDK 1.0.2.......well not entirely yours.....

The page from the wiki describing the build steps (https://projects.forum.nokia.com/qbluetooth/wiki/buildIncludeSymbian) mentions that you must donwload __Qt for Symbian__, but as of Qt 4.7.2, it seems that Nokia stopped providing the Qt for Symbian as a separate package but you have to download Nokia Qt Sdk instead. Here there is a catch. The Nokia Qt SDK doesn't have some necessary files for the bluetooth thus making QBluetooth unable to compile to Nokia Qt SDK (at least without some tweaking).

Any way, the reason why you can't build it is also shown to your last image "'''No Qt Installed'''". For this to be working, after installing Symbian SDK, you have to install the Qt for Symbian which seats on top of the Symbian SDK.

Now, what's the solution...

Use the binaries. Why do you need to build the library on your own?

If you need help on which binaries to use just provide us with the model of your deployment mobile device and which capabilities your program will use. Will you sign it or not?

