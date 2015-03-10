Topic #27 - Problem with QBluetooth linking in Windows
----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    03/11/11 23:11:57
Moderators:
----------------------------------------------------------------------------

Hello.
I've got a question: where exactly headers must be placed?
I copied all QBluetooth .h files from QBluetooth lib.zip (bin/epoc32/include) to the same directory as library file (qt/QBluetooth/bin) and I got "undefinded refrence to" errors for each QBluetooth class used.

----------------------------------------------------------------------------
Author:     colorfulFOOL
Created:    03/12/11 22:59:59
Moderators:
----------------------------------------------------------------------------

Thanks fou your reply, but the problem was, I builded QBluetooth lib with Visual Studio (Qt Creator gived thousands of errors) using msvc compiler and for my application compiling I used Qt Creator with mingw. After switching Creator to msvc, everything works fine.

One more question, all devices I get from QBtDeviceDiscoverer have correct names but their addresses are "00:00:00:00:00:00". May it be caused by not purchased (working in demo-mode) BlueSoleil drivers?
