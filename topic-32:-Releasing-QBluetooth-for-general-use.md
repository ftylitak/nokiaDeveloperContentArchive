Topic #32 - Releasing QBluetooth for general use
----------------------------------------------------------------------------
Author:     ltomuta
Created:    03/18/11 20:04:52
Moderators:
----------------------------------------------------------------------------

Hi!

This library seems to be of interest for several developers and it is quite likely that some of them will find it useful enough for use in their published applications. However for that to happend you must ensure that the library can be distributed with apps (e.g. in Ovi Store) which would require that:

 * the library has a unique name which does not clash with other similar libraries (or with the self-signed version of itself)
 * the library is certified (either Express Signed or through Ovi Publishing)
 * the library has the maximum of capabilities you can afore (taking signing options under consideration)
 * the library suppports embedding by ensuring that its version can be queried at install time and downgrades are avoided (see http://wiki.forum.nokia.com/index.php/TSS001331_-_Enforcing_version_checking_of_embedded_SIS_file )

Are you having any plans in this regard? If so, do let me know if you need help with this task.

Best regards,
Lucian

----------------------------------------------------------------------------
Author:     lpvalente
Created:    03/20/11 19:40:46
Moderators:
----------------------------------------------------------------------------

> Hi! This library seems to be of interest for several developers and it is quite likely that some of them will find it useful enough for use in their published applications. However for that to happend you must ensure that the library can be distributed with apps (e.g. in Ovi Store) which would require that:
>
> * the library has a unique name which does not clash with other similar libraries (or with the self-signed version of itself)

How can we get this unique UID for the library? Can we use one of the UIDs given when we sign-up as a publisher for Ovi Store?


>  * the library is certified (either Express Signed or through Ovi Publishing)

Does this mean we should perform the tests in the symbian signed criteria themselves, or take the library to the formal certification process?

> * the library has the maximum of capabilities you can afore (taking signing options under consideration) *

In this case can we just use all possible grantable capabilities for Express Signed? (TARGET.CAPABILITY = All -TCB -AllFiles -DRM ) ?

>  the library supports embedding by ensuring that its version can be queried at install time and downgrades are avoided (see http://wiki.forum.nokia.com/index.php/TSS001331_-_Enforcing_version_checking_of_embedded_SIS_file  ) Are you having any plans in this regard? If so, do let me know if you need help with this task. Best regards, Lucian
> [[BR]]

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/23/11 17:36:09
Moderators:
----------------------------------------------------------------------------

> * the library has a unique name which does not clash with other similar libraries (or with the self-signed version of itself)

I have one question here...

Does that mean that we have to change the name of the project because of the official support of Bluetooth support in Qt Mobility 1.2?

----------------------------------------------------------------------------
Author:     lpvalente
Created:    03/23/11 22:15:21
Moderators:
----------------------------------------------------------------------------

The "name" refers to the UID the library uses

----------------------------------------------------------------------------
Author:     favoritas37
Created:    03/24/11 11:24:48
Moderators:
----------------------------------------------------------------------------

Yes Luis i understand the difference. I am talking about the name itself (apart from the issue of the UID).

I mention that because both ours and Qt Mobility's libraries have the same name, QBluetooth.

That's what i am asking, if that is a problem.

