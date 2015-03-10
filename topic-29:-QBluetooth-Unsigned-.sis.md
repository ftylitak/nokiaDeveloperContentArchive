----------------------------------------------------------------------------
Author:     derick.lungaho
Created:    03/16/11 14:16:49
Moderators:
----------------------------------------------------------------------------

This is for favoritas37.

I've tried building QBluetooth (i'm using symbian on linux.....) to no avail. All I wainted was to generate an unsigned .sis that I can sign together with my app.
Is it possibile for you to an unsigned sis and attach it with the qbluetooth binaries? I'm running S60 3rd Edition FP2.

Thanks in advance,

Derick

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/16/11 16:30:38
Moderators:
----------------------------------------------------------------------------

Hello, test the following:

!https://projects.forum.nokia.com/qbluetooth/files/Symbian_unsigned.zip

I used the following capabilities:

{{{
TARGET.CAPABILITY += Location
        LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData
}}}
Is it ok?[[BR]]

----------------------------------------------------------------------------
Author:     derick.lungaho
Created:    03/16/11 16:51:06
Moderators:
----------------------------------------------------------------------------

Thanks.

If its not a bother could you include these:

Basic capabilities - approved by the end user

LocalServices
UserEnvironment
NetworkServices
ReadUserData
WriteUserData
Location (from S60 3rd FP2)*
Extended capabilities - approved by Symbian Signing

Open signed online

SwEvent
SurroundingsDD
ProtServ
PowerMgmt
ReadDeviceData
WriteDeviceData
TrustedUI
Location (before S60 3rd FP2)*


I'm experimenting with the library and I didn't want to hit a roadblock which required some capabilities. I have developer certificates which allow me to play with the above :)

Thank you again for taking time to help me.

Best,

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/16/11 17:15:59
Moderators:
----------------------------------------------------------------------------

Ready: https://projects.forum.nokia.com/qbluetooth/files/QBluetooth_unsigned_%28Open_signed_capabilities%29.zip

I placed 2 files, one for Debug and one for Release. :)

----------------------------------------------------------------------------
Author:     derick.lungaho
Created:    03/16/11 17:23:45
Moderators:
----------------------------------------------------------------------------

Thanks! :)

