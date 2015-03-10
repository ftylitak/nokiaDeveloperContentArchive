Topic #37 - QBtSerialPortServer::sendData(QByteArray )
----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    05/15/11 11:45:59
Moderators:
----------------------------------------------------------------------------

Hello. 

I noticed, there is sendData('''QByteArray''' ) method in QBtSerialPortServer_symbian.h, but there is only sendData('''QString''' ) in headers from "Symbian_S60_3rd_FP1_binaries.zip" and "Symbian_binaries.zip".  QString variant doesn't work in my app, and QByteArray variant might be just what I need. Is this method in .sis library from "Symbian_S60_3rd_FP1_binaries.zip" and will it work, if I just add it to the header?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    05/15/11 12:40:19
Moderators:
----------------------------------------------------------------------------

No it's not going to work the way you are thinking it.

But it will be easy enough for me to add it to the API. Would that be ok?

----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    05/15/11 13:41:58
Moderators:
----------------------------------------------------------------------------

Yes, of course. I use S60 3rd Edition FP 1 version.

Thanks in advance.

