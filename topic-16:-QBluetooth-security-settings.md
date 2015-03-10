Topic #16 - QBluetooth security settings
----------------------------------------------------------------------------
Author:     dpdrm
Created:    02/03/11 10:03:07
Moderators:
----------------------------------------------------------------------------

Hello,

Correct me if I'm wrong, but I couldn't find any ready made function(s) to set the BT service security parameters, so I viewed the source code, my question:

Can I just add the security settings into the _symbian.cpp sources and build the library again? This will of course affect all services created but that is not a problem. So, if I add the usual Symbian security lines into for example QBtSerialPortServer_symbian.cpp:

{{{
TBTServiceSecurity secSettings;
 TUid settingsUID;
settingsUID.iUid = KBT_serviceID;
secSettings.SetUid(settingsUID);
secSettings.SetAuthentication(EFalse);
secSettings.SetAuthorisation(EFalse);
secSettings.SetEncryption(EFalse);
btsockaddr.SetSecurity(secSettings);
}}}

Would this work when I build the library again or is there something extra I should take into account?

Thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/03/11 10:42:33
Moderators:
----------------------------------------------------------------------------

Hello dpdrm,

The library should work just fine as you describe.

The security issue, till now is left unimplemented. From the beginning of this project it is marked as TODO (reference Roadmap tab under [https://projects.forum.nokia.com/qbluetooth/milestone/Symbian%20bluetooth%20security%20support security support]) but as you can see it never happened.

All information needed for security can be found in the [http://www.forum.nokia.com/info/sw.nokia.com/id/89261a1d-ed6d-4cc5-8855-8250a33de328/S60_Platform_Bluetooth_API_Developers_Guide.html S60 Platform: Bluetooth API Developer's Guide] document under section 8.1.

----------------------------------------------------------------------------
Author:     dpdrm
Created:    02/03/11 10:53:27
Moderators:
----------------------------------------------------------------------------

Thanks,

That's what I thought too, cheers!

