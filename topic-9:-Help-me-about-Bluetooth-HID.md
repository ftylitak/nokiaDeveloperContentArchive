[initial message not found]

----------------------------------------------------------------------------
Author:     tms.turs
Created:    12/16/10 12:19:12
Moderators:
----------------------------------------------------------------------------

Thanks you! I understand your idea!
I've using example at here: [http://wiki.forum.nokia.com/index.php/QuteMessenger_-_Bluetooth_Chat] but I can't complier project. Qt creator make too error.
I thinks my project the same this project:
[http://www.electro-tech-online.com/electronic-projects-design-ideas-reviews/42183-device-control-through-bluetooth-nokia-mobile-phones-full-project.html]
and

[http://wiki.forum.nokia.com/index.php/Bluetooth_HID_profile_(client_device)]
video:

[http://www.youtube.com/watch?v=_s5F_wsuMn0]

Please help me!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 13:24:26
Moderators:
----------------------------------------------------------------------------

Very nice indeed! Great video :)

I have never used bluetooth at such low level, to implement a protocol from the scratch. The wiki article that you posted is quite interesting and thank you!

At this time of the year it is a bit impossible to find time to integrate the HID to the library but it is definitely a future work!

Since then, if you want to integrate the code of the wiki page that you posted to your program:

Option A) You implement your project at Symbian C++ so you use directly the code from the wiki. That's what they did at the first link that you posted above.

Option B) Implement at Qt and use the code from the wiki as we did here. Mixed QT and Symbian C++ : !http://developer.symbian.org/wiki/Using_Qt_and_Symbian_C++_Together

Option C) Extend this library yourself :)

Any option you choose, we will be able to help you if you report any specify problem that you have.

A personal opinion, i use Carbide C++ instead of Qt Creator for such development...but that's as i say personal opinion.

----------------------------------------------------------------------------
Author:     tms.turs
Created:    12/16/10 14:04:20
Moderators:
----------------------------------------------------------------------------

The first, many thanks to you!
I think I'll use Qt for my project. Because I don't know how to use carbide  because it is too difficult for me to make GUI like Qt.
And, this is my project:
[http://www.mediafire.com/?lo8gbeanyejn6o4]
I'll use Qt Creator through Qt Bluetooth Library, and I'll send a character via bluetooth as a short sms.
The circuit will get data, then decode it and control that I want.
But, I've tried to compile an example of another topic in this forum but Qt creator has a lot of  error.
I've tried to build Qt bluetooth library but Carbide has a lot of error.
Please give me advice!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 14:11:14
Moderators:
----------------------------------------------------------------------------

Sorry, i have one question. You say ''"I'll send a character via bluetooth as a short sms". ''

This means you will use the Serial Port out this project? Or you will somehow use the code fron the wiki page that you mentioned earlier?

----------------------------------------------------------------------------
Author:     tms.turs
Created:    12/16/10 14:52:27
Moderators:
----------------------------------------------------------------------------

Thank for your feed back!
Yes, I'll use the Serial Port for my project. Because you say: "The HID protocol is not supported from QBluetooth library". I've tried complie Qbluetooth Messenger Example at: [http://wiki.forum.nokia.com/index.php/QuteMessenger_-_Bluetooth_Chat] but, when I building Qbluetooth like this tutorial Carbide get's alot of error.
I've install complete Carbide c++ 2.3 -> Active Perl -> S60_3rd_edittion_FF2 -> Open C/C++ -> Nokia SDK (include Qt creator).
Please help me step by step for rebuild "QuteMessenger"!
Sorry because disturb you. But I'm an Electronic engineer, i'm amateur on Mobile Programming!
Wait for you!
Thanks you!
