Topic #42 - Pairing the Bluetooth device
----------------------------------------------------------------------------
Author:     naimidrees
Created:    10/31/11 16:31:41
Moderators:
----------------------------------------------------------------------------

I want to pairing the bluetooth device. In QBluetooth we found a function as blow,

{{{
TBool QBtLocalDevicePrivate::AddNewDevice(const QBtDevice& device)
{
    try
    {
        CBTDevice* newDev;

        RBTRegServ regServ;

        RBTRegistry view;

        TRequestStatus stat;

        QT_TRAP_THROWING
        (
            newDev = CBTDevice::NewL(device.getAddress().convertToSymbianBtDevAddr()));

            newDev->SetDeviceClass(TBTDeviceClass(device.getType()));

            TBuf8<4> psd(_L8("0000"));

            TPINCodeV10 pinCode10;

            pinCode10.iLength = 4;

            memcpy(pinCode10.iPIN,psd.Ptr(),4);

            TBTPinCode pinCode(pinCode10);

            TPtrC8 name8((const TUint8 *)device.getName().toUtf8().data(),device.getName().toUtf8().size());

            TPtrC16 name16(device.getName().utf16());

        QT_TRAP_THROWING(
        {
            newDev->SetDeviceNameL(name8);

            newDev->SetFriendlyNameL(name16);

            newDev->SetPaired(ETrue);

            newDev->SetPassKey(pinCode);

            regServ.Connect();

            view.Open(regServ);

            view.AddDeviceL(*newDev,stat);
        });

        User::WaitForRequest(stat);

        stat.Int();

        view.Close();

        regServ.Close();

        SafeDelete(newDev);
    }
    catch(char* str)
    {
        return false;
    }

    return true;
}
}}}

But I did not pair the device. I can communicate with the device successfully by pair the device manually.
Anybody can help me.

----------------------------------------------------------------------------
Author:     naimidrees
Created:    11/01/11 18:26:50
Moderators:
----------------------------------------------------------------------------

We have tried above code but it is not working.


