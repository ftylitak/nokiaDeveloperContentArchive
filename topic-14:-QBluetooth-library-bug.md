Topic #14 - QBluetooth library bug?
----------------------------------------------------------------------------
Author:     nadiolina82
Created:    01/27/11 11:43:06
Moderators:
----------------------------------------------------------------------------

Hi guys,
I have a problem with discovery devices.
When I can't find any device in the range the application crashes.
This is my code:
void Neighbours::getNeighboursBTs(QString javascriptmethod)
        {
        if(deviceDiscoverer)
                {
                connect(deviceDiscoverer,SIGNAL(discoveryStarted()), this, SLOT(discoveryStarted()));
                connect(deviceDiscoverer, SIGNAL(discoveryStopped()), this, SLOT(deviceDiscoveryCompleteReport()));
                connect(deviceDiscoverer, SIGNAL(newDeviceFound(const QBtDevice &)), this,SLOT(populateDeviceList(const QBtDevice &)));
                connect(deviceDiscoverer,SIGNAL(error (QBtDeviceDiscoverer::DeviceDiscoveryErrors)), this, SLOT(deviceDiscoveryErrors(QBtDeviceDiscoverer::DeviceDiscoveryErrors)));


                        QBtLocalDevice::askUserTurnOnBtPower();
                         if (deviceDiscoverer->isBusy() )
                                 {
                                         return;
                                 }
                         try
                         {
                                 deviceDiscoverer->startDiscovery();
                         }catch(...)
                                 {
                                         manager->debug("eccezione startDiscovery");
                                 }
                         /*foundDevices=deviceDiscoverer->getInquiredDevices();
                         if(foundDevices.size()==0)
                                 {
                                debug("No bt devices found");
                                 }*/


                }

        }

void Neighbours::discoveryStarted()
        {
        debug("discoveryStarted");


        }
void Neighbours::populateDeviceList(QBtDevice newDevice)
{

                        manager->debug(QString("found device: %1 address: %2").arg(newDevice.getName()).arg (newDevice.getAddress().toString()));

                        foundDevices.append(newDevice);


}
void Neighbours::deviceDiscoveryErrors(QBtDeviceDiscoverer::DeviceDiscoveryErrors error)
        {
debug("error");
        }
void Neighbours::deviceDiscoveryCompleteReport()
{
        disconnect(deviceDiscoverer,SIGNAL(discoveryStarted()), this, SLOT(discoveryStarted()));
        disconnect(deviceDiscoverer, SIGNAL(discoveryStopped()), this, SLOT(deviceDiscoveryCompleteReport()));
        disconnect(deviceDiscoverer, SIGNAL(newDeviceFound(const QBtDevice &)), this,SLOT(populateDeviceList(const QBtDevice &)));
        disconnect(deviceDiscoverer,SIGNAL(error (QBtDeviceDiscoverer:eviceDiscoveryErrors)), this, SLOT(deviceDiscoveryErrors(QBtDeviceDiscoverer::DeviceDiscoveryErrors)));



}
When no bt devices is found, i can see an alert with string "discoveryStarted", then the application crashes.
What's wrong?
Another problem is when i try to use startDiscovery  () with getInquiredDevices(), in this way:
{
                /*connect(deviceDiscoverer,SIGNAL(discoveryStarted()), this, SLOT(discoveryStarted()));
                connect(deviceDiscoverer, SIGNAL(discoveryStopped()), this, SLOT(deviceDiscoveryCompleteReport()));
                connect(deviceDiscoverer, SIGNAL(newDeviceFound(const QBtDevice &)), this,SLOT(populateDeviceList(const QBtDevice &)));
                connect(deviceDiscoverer,SIGNAL(error (QBtDeviceDiscoverer::DeviceDiscoveryErrors)), this, SLOT(deviceDiscoveryErrors(QBtDeviceDiscoverer::DeviceDiscoveryErrors)));
                */
                BtLocalDevice::askUserTurnOnBtPower();
                         if (deviceDiscoverer->isBusy() )
                                 {
                                         return;
                                 }
                         try
                         {
                                 deviceDiscoverer->startDiscovery();
                         }catch(...)
                                 {
                                         manager->debug("eccezione startDiscovery");
                                 }
                         foundDevices=deviceDiscoverer->getInquiredDevices();
                         if(foundDevices.size()==0)
                                 {
                                debug("No bt devices found");
                                 }
else
{
debug(QString::number(foundDevices.length()) + " bts");
}

Please help me.

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/27/11 12:30:29
Moderators:
----------------------------------------------------------------------------

One minor thing, change the definition of the funtion

{{{
void Neighbours::populateDeviceList(QBtDevice newDevice);
}}}
to

{{{
void Neighbours::populateDeviceList(const QBtDevice& newDevice);
}}}

Then, what you can do that would make it easier for us to help is: run the application in debug mode and copy paste here all the console output.
The portion of the code that you posted seems ok...

----------------------------------------------------------------------------
Author:     nadiolina82
Created:    01/27/11 13:18:16
Moderators:
----------------------------------------------------------------------------

The error is:

Thread [Thread id: 2823] (Suspended: Signal 'Exception 100' received. Description: Thread 0xb07 has panicked. Category: USER; Reason: 130.)
        2 Unknown (0x8029B7E8)()  0x8029b7e8
        1 Unknown (0x8029B7E8)()  0x8029b7e8
On the cnsole i can see:
[Qt Message] "deviceDiscoveryCompleteReport()"


----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/27/11 14:01:10
Moderators:
----------------------------------------------------------------------------

If it is helpful, the User:130 means that an array (or pointer or anything like that) goes out of bounds.

Have you initialized "foundDevices"?

----------------------------------------------------------------------------
Author:     favoritas37
Created:    01/27/11 14:12:39
Moderators:
----------------------------------------------------------------------------

Also, deviceDiscoverer->startDiscovery() is not blocking method so you can't use any other relevant to it code in the same function.

This could be a problem.

