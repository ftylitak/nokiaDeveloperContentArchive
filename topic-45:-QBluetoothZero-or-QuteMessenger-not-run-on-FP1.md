Topic #45 - QBluetoothZero or QuteMessenger not run on FP1
----------------------------------------------------------------------------
Author:     nfrfab
Created:    02/05/12 21:27:42
Moderators:
----------------------------------------------------------------------------

Hi all. I have a problen and I can find the error. My problem is:
QBluetoothZero or QuteMessenger not run on n95-3 (S60 3rd FP1). The application not run (no show nothing, no error, no messages).
To solved the problem I did this:
- I compiled QuteMessenger with QBluetoothZero.sis, QBluetoothZero_selfsigned_S60_FP1.sis (changing the name on QuteMessenger.pro).
- I compiled QBluetoothZero.pro and use the .sis (recompiled the QuteMessenger.pro with the new QBluetoothZero.sis).
- I install the last version of the Qt for symbian (openc_glib_s60_1_7_SS.sis, openc_ssl_s60_1_7_SS.sis, pips_s60_1_7_SS.sis, stdioserver_s60_1_7_SS.sis, STDCPP_s60_1_7_SS.sis and Qt 4.7.1)
 I tried all, but the application not run.
I think I need a new version of qt sdk (or qt mobility), but I'm not sure which version I need (I am sure the last version do not work in S60 3rd FP1).
Do you have any idea? Thank you.

Note: I don't speak English.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/06/12 11:35:27
Moderators:
----------------------------------------------------------------------------

Hello, i saw your posts in the Discussion Boards yesterday but i didn't have time to reply.

I think i know where is the problem but i will take a look at it tomorrow.
Probably it is a small mistake in the compilation of the FP1 version of the library that we provide so i will take a look at it.

Also since you are targeting S60 FP1 you would better use Qt 4.6.3. So yes you need a new version of Qt, the 4.6.3.

Regards.

----------------------------------------------------------------------------
Author:     nfrfab
Created:    02/07/12 00:09:58
Moderators:
----------------------------------------------------------------------------

The version I use is: Nokia_Qt_SDK_Win_offline_v1_0_2_en.exe (Qt Creator version 2.0.1, Qt Simulator version 1.0.1, Qt for Symbian 4.6.3, Qt for Maemo 4.6.3, qt mobility 1.0.2). I think it work perfect with QBluetoothZero but I not sure (in Qt Creator Menu-> Help -> About Qt Creator... say : Based on Qt 4.7.0 (32 bit) Built on Sep 9 2010...
With this version of Qt I never had problems. But if you think it not work I'll download another.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/07/12 12:12:42
Moderators:
----------------------------------------------------------------------------

No the version you are using is fine. The "Based on Qt 4.7.0 (32 bit) Built on Sep 9 2010" corresponds to the Qt Creator itself. So you are using Qt 4.6.3 for your application which is fine.

----------------------------------------------------------------------------
Author:     nfrfab
Created:    02/08/12 23:53:31
Moderators:
----------------------------------------------------------------------------

Ok. If you find the problem of library, Please let me know. Thank.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/12/12 19:32:25
Moderators:
----------------------------------------------------------------------------

Hello once again, at some time i recompiled the library as it should be and  here is the produced binary: https://projects.developer.nokia.com/qbluetooth/files/QBluetoothZeroFP1.sis

I tried to test it to QuteMessenger on a S60 3rd FP1 device but i kept having the "Feature not supported" error when trying to run it, despite the fact that i was following the instructions here: https://bugreports.qt-project.org//browse/QTSDK-170

Any way, because i am running short on time i can't thoroughly test it so if you please, give the new binary a try and let me know if it works.

Regards.

----------------------------------------------------------------------------
Author:     nfrfab
Created:    02/13/12 04:03:28
Moderators:
----------------------------------------------------------------------------

I installed on two phones (n95-3), does not work (Feature not supported). Anyway, thank you.


----------------------------------------------------------------------------
Author:     fluxlinkage
Created:    08/20/12 09:34:57
Moderators:
----------------------------------------------------------------------------

I have the same problem on my N95.

https://projects.developer.nokia.com/qbluetooth/files/QBluetoothZeroFP1.sis

works well when my program only discovers devices and services. But if my program uses the QBtObjectExchangeClient class (to send a file), it does not work ("Feature not supported").

I recompiled QBlueToothZero, and when Qt Creator complains about missing files (btextnotifiers.dso, irobex.dso, ...), I used the files from the S60 3rd Edition FP1 SDK (not form the [https://projects.developer.nokia.com/qbluetooth/files/MissingS60BluetoothFiles.rar MissingS60BluetoothFiles.rar]).

This solved my problem! And I can run [http://www.developer.nokia.com/Community/Wiki/QuteMessenger_-_Bluetooth_Chat QuteMessenger] on my N95 now.

Note: I don't speak English, either.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    08/27/12 18:04:05
Moderators:
----------------------------------------------------------------------------

The binaries for the Symbian FP1 are now updated, following your suggestion.

As of now they are a separate download since they vary not only to the generated .sis but to the library files as well (.dso). They can be downloaded from the main project's page or from [https://projects.developer.nokia.com/qbluetooth/downloads/8 here]

I have tested it on a Nokia E51 and it worked but i would really appreciate if someone could confirm it as well.

----------------------------------------------------------------------------
Author:     fluxlinkage
Created:    08/28/12 05:58:19
Moderators:
----------------------------------------------------------------------------

It works on my Nokia N95.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    08/28/12 10:36:07
Moderators:
----------------------------------------------------------------------------

Glad to hear, thank you very much!

