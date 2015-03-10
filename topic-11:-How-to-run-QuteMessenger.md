----------------------------------------------------------------------------
Author:     tms.turs
Created:    12/16/10 19:40:11
Moderators:
----------------------------------------------------------------------------

Hi guy, I've downloaded QuteMessenger, a application using Qtbluetooth Library, but I don't know how to run it.
I've tried to build follow tutorial, but Qt Creator has a lot of error!
I was freeze export Qtbluetooth Library complete with no error and a lot of warning!
Please help me how to run this example:
Link: [http://wiki.forum.nokia.com/index.php/QuteMessenger_-_Bluetooth_Chat]
Thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 19:51:36
Moderators:
----------------------------------------------------------------------------

Hi again, i have you in mind. As soon as i find some time i will be back.

----------------------------------------------------------------------------
Author:     tms.turs
Created:    12/22/10 21:03:30
Moderators:
----------------------------------------------------------------------------

Thanks you so much! I've tried import and build Qutemessenger to Carbide, but when building, I've get a error:
[[Image(http://i1222.photobucket.com/albums/dd484/tms_turs/Qt%20Nokia%20Error/error1.jpg)]]
Thanks you for feedback!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/23/10 12:24:31
Moderators:
----------------------------------------------------------------------------

What is your build configuration?

You have to build it as Phone Debug (GCCE) or Release.

Also for your future convenience, instead of posting a printscreen you can just copy paste the output from the Console tab in the Carbide.

----------------------------------------------------------------------------
Author:     amarquesferraz
Created:    01/16/11 22:59:07
Moderators:
----------------------------------------------------------------------------

I found a little issue which was not letting me compile successfully !QuteMessenger via Qt Creator.

In the line 37 of the file !QuteMessenger.pro; apparently, there is a little mistake; it shows:

{{{
"@\"$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_selfsigned.sisx\",(0xA003328D)"\
}}}
Here, I only managed to compile when I changed it to:

{{{
"@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_selfsigned.sisx\",(0xA003328D)"\
}}}
(note the “$$” just before “(EPOCROOT)” ). 

