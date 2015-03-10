Topic #17 - building QBluetooth library with Qt Creator
----------------------------------------------------------------------------
Author:     dmartino
Created:    02/04/11 22:47:53
Moderators:
----------------------------------------------------------------------------

Hi!

I made the following steps to compile QBluetooth API:

1) I installed the 3rd Edition FP1;
2) I installed the OpenC/C++ plugin;
3) I installed the BluetoothEngineAPI;
4) I installed the Qt for Symbian;
5) I downloaded last source and bin svn;

I used the QBluetooth.pro file to open the project in the Qt Creator to create the DLL.

For the build step I used the target Qt Simulator and as a result of building I got the following errors:

1) class QBtServiceDiscovererPrivate has no member named 'DiscoverServices'
C:\Users\..\source\ServiceDiscoverer\Impl\QBtServiceDiscoverer.cpp   QBtServiceDiscoverer.cpp 56

2) class QBtServiceDiscovererPrivate has no member named 'DiscoverObexServices' QBtServiceDiscoverer.cpp 65

3) class QBtServiceDiscovererPrivate has no member named 'DiscoverRfcommServices' QBtServiceDiscoverer.cpp 73

4) no matching function for call to 'QBtServiceDiscovererPrivate::DiscoverSpecificClass(QBtDevice*, const QBtUuid&)' QBtServiceDiscoverer.cpp 82

5) class QBtServiceDiscovererPrivate has no member named 'DiscoverSpecificClasses' QBtServiceDiscoverer.cpp 91

How can to resolve these?

Another perplexity: is Qt Simulator the right choice in the target selector for building or is a real Symbian device needed?

Many thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/05/11 12:48:48
Moderators:
----------------------------------------------------------------------------

You found the problem your own :)

Yes the Qt Simulator is the problem, it doesn't support any bluetooth operation.

Try compiling and running on Phone Debug (or Release) and you will be fine!