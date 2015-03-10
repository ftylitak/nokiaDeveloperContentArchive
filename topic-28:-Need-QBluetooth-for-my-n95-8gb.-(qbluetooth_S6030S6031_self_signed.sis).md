Topic #28 - Need QBluetooth for my n95 8gb. (qbluetooth_S6030S6031_self_signed.sis)
----------------------------------------------------------------------------
Author:     erespia2
Created:    03/14/11 13:14:51
Moderators:
----------------------------------------------------------------------------

Hi,

   This message is for favoritas37 or anyone who can help:

  I need the sisx package for the OS of my teminal (N95 8gb). (SELF_SIGNED)

I have little idea of how to build it myself, I tried but I get a lot of mistakes, I'm terrible for this.

Can you help me?

----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    03/14/11 15:01:15
Moderators:
----------------------------------------------------------------------------

Why not use sis from https://projects.forum.nokia.com/qbluetooth/downloads/1 ? It is in epoc32/InstallToDevice/ folder

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/14/11 15:20:08
Moderators:
----------------------------------------------------------------------------

Ok, now that i looked it up i can see why it doesn't works for you. Your device is S60 3rd FP1 whereas the .sisx file provided by the library is only for S60 3rd FP2 and newer because of the bluetooth plugin used for some operations.

So next step is to compile the library for S60 3rd FP1 and give you the appropriate .sisx file. Unfortunately there is a small problem that i have to overcome. I saw that the Qt for Symbian installation package is gone and only Nokia Qt SDK can be downloaded (which doesn't work in ur case)...any way i will try to find an older installation package (Qt 4.7.1) from somewhere and i will be back later tonight.

----------------------------------------------------------------------------
Author:     erespia2
Created:    03/14/11 17:27:49
Moderators:
----------------------------------------------------------------------------

Ok, thank you very much.

I hope you can get and then work for S60 3rd FP1.

I will be attentive to its progress.

Greetings.

----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    03/14/11 19:01:07
Moderators:
----------------------------------------------------------------------------

favoritas37, I think you can find it here: ftp://ftp.qt.nokia.com/qt/source/

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/14/11 19:04:48
Moderators:
----------------------------------------------------------------------------

Thank you very much!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/14/11 20:41:23
Moderators:
----------------------------------------------------------------------------

So the binaries are ready. I tested them on an identical device like yours (N95 8GB) and it works fine. You can get the binaries from [https://projects.forum.nokia.com/qbluetooth/files/Symbian_S60_3rd_FP1_binaries.zip here].

Things you have to be careful:

Make sure you have all the necessary libraries installed to your device. That is the OpenC/C++ plugin .sis files and the Qt.sis file.
 * From OpenC
   * openc_glib_s60_1_7_SS.sis
   * openc_ssl_s60_1_7_SS.sis
   * pips_s60_1_7_SS.sis
   * stdioserver_s60_1_7_SS.sis
 * From OpenC++
   * STDCPP_s60_1_7_SS.sis
 * From Qt
   * One of the installation files contained in the Qt folder (qt.sisx for example)

I would recommend you to reinstall them all just to be sure.

Also the files that you will download from the previous link will be used according to the instructions [https://projects.forum.nokia.com/qbluetooth/wiki/binaries here].

In the ''!InstallToDevice'' you will find 2 .sisx files: '''QBluetooth_selfsigned_no_location.sisx''' and '''QBluetooth_selfsigned.sisx'''. As the name says the first is created without the Location capability. The application that i used to test the library used the one without the Location capability. By default it will use the other one ('''QBluetooth_selfsigned.sisx''') so if you want to use the first .sisx rename it from '''QBluetooth_selfsigned_no_location.sisx '''to '''QBluetooth_selfsigned.sisx. '''

Now as the instructions says, copy the entire epoc32 folder to your Symbian SDK epoc32 folder and you should be ready.

Please reply here when you test it.

PS: I also uploaded the application that i used for testing. It is ready to be installed. Download from [https://projects.forum.nokia.com/qbluetooth/files/QuteMessenger_FP1_no_location.sisx here].

Regards.

----------------------------------------------------------------------------
Author:     erespia2
Created:    03/14/11 21:18:33
Moderators:
----------------------------------------------------------------------------

Yeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeessssssssssssssssssssssssss!!!!!!!


It works fine!!!!!

Thank you so much!!!!
Thank you so much!!!!
Thank you so much!!!!
Thank you so much!!!!
Thank you so much!!!!
Thank you so much!!!!
Thank you so much!!!!

Regards!!!!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/14/11 21:47:21
Moderators:
----------------------------------------------------------------------------

Glad to hear it works!

