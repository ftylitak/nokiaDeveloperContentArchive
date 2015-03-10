Topic #19 - qDebug() doesn't work using QBluetooth in a project
----------------------------------------------------------------------------
Author:     dmartino
Created:    02/12/11 13:37:41
Moderators:
----------------------------------------------------------------------------

Hi!

I'm using QBluetooth on windows. I have this code: 

#include "discoverer.h"
{{{
#include <QBluetooth.h>
}}}

{{{
#include <QDebug>
}}}

{{{
#include <QCoreApplication>
}}}

{{{

}}}

{{{

}}}

{{{
Discoverer::Discoverer(QObject* parent):QObject(parent)
}}}

{{{
{
}}}

{{{
    QBtDeviceDiscoverer* discoverer = new QBtDeviceDiscoverer(this);
}}}

{{{
    qDebug() << "discovering ...";
}}}

{{{
    discoverer->startDiscovery();
}}}

{{{

}}}

{{{
    QList<QBtDevice> deviceList = discoverer->getInquiredDevices();
}}}

{{{
     qDebug() << "there are" << deviceList.size() << "devices in range";
}}}

{{{

}}}

{{{
}
}}}


I don't know why qDebug() call doesn't print in Application Output. It worked fine, before the installation of the "debugging tools for windows" ...

How can resolve the problem?

Thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/20/11 19:23:17
Moderators:
----------------------------------------------------------------------------

I tested it with the code you posted to an other thread and it works fine.

And i will suppose it works for you too now, isn't it?

----------------------------------------------------------------------------
Author:     dmartino
Created:    02/21/11 18:12:45
Moderators:
----------------------------------------------------------------------------

Hi favoritas37,

first: thanks very much for your replies and for your tips, now qDebug() works fine! :)

Could you help me once again? 

I got the debug of the code you posted to an other thread and I've the error:

QObject::connect: Cannot queue arguments of type 'QBtDevice'

<Make sure 'QBtDevice' is registered using qRegisterMetaType<>. >

I inserted this function call at the top of the main class:

 int queued = qRegisterMetaType<QBtDevice>("QBtDevice");
{{{
qDebug() << queued;
}}}
{{{

}}}


I used the macro Q_DECLARE_METATYPE(QBtDevice) in the header file but I've any change ...

Many thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/21/11 21:45:25
Moderators:
----------------------------------------------------------------------------

Interesting, it happed to me the very first time i wrote a program for the library...but i forgot it ever since...

I have no idea why it happens.The thing is that i can't really explain why this problem didn't came up sooner...
If anyone has any idea it would be great.

PS. to the application that i mentioned earlier, i removed the line that you posted and still works fine....no idea..

----------------------------------------------------------------------------
Author:     dmartino
Created:    02/23/11 13:11:14
Moderators:
----------------------------------------------------------------------------

Hi favoritas37,

I solved the problem adding another parameter in connect() call. I added Qt::!DirectConnection like last parameter. So, the !DeviceDiscoverer class can detect remote devices.

Now I've another problem: the class emits the discoveryStopped() signal but in my slot to handle this I found the !DeviceDiscoverer is busy ... how is it possible? I tried to call stopDiscovery() directly but I've no change ... Have you an idea?

The program execution stops in qatomic_windows.h file @ return QT_INTERLOCKED_INCREMENT(&_q_value) != 0;

Thanks!

