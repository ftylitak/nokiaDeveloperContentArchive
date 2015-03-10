Topic #21 - exception using QBluetooth QBtDeviceDiscoverer
----------------------------------------------------------------------------
Author:     dmartino
Created:    02/16/11 17:33:37
Moderators:
----------------------------------------------------------------------------

Hi!

I'm working with QBluetooth on Window.

I'm trying to discover the devices. I'm using Bluesoleil.

My header file is:

{{{
#ifndef DISCOVERER_H
}}}
{{{
#define DISCOVERER_H

#include <QBluetooth.h> #include <QObject>

class Discoverer : QObject {

  Q_OBJECT

public:

  Discoverer(QObject* parent = 0);

};
}}}
{{{
#endif // DISCOVERER_H
}}}
My .cpp file is:

{{{
#include "discoverer.h"
#include <QBluetooth.h>
#include <QDebug>

Discoverer::Discoverer(QObject* parent):
        QObject(parent)
{
    QList<QBtDevice>* devicesList = new QList<QBtDevice>();
    QBtDeviceDiscoverer* deviceDiscoverer = new QBtDeviceDiscoverer(parent);

    qDebug() << "discovering ...";

    while(devicesList->size()<1){
        qDebug() << "sono nel while";
        deviceDiscoverer->startDiscovery();
        qDebug() << "startDiscovery chiamata";
        devicesList->append(deviceDiscoverer->getInquiredDevices());
        qDebug() << "devices number";
        qDebug() << devicesList->size();
    }

    qDebug() << "discovering finita";
}
}}}
I don't know  how to resolve the exception that i got in Application output:

Exception at 0x6876562f, code: 0xc0000005: write access violation at: 0x1101407, flags=0x0

The stack state is:

0 QBasicAtomicInt::ref qatomic_windows.h 319 0x680e562f [[BR]]1 QList<QBtDevice>::operator= qlist.h 428 0x3e1a1e [[BR]]2 QList<QBtDevice>::operator+= qlist.h 801 0x3e18bd [[BR]]3 QList<QBtDevice>::append qlist.h 822 0x3e1853 [[BR]]4 Discoverer::Discoverer discoverer.cpp 25 0x3e1407 [[BR]]5 main main.cpp 9 0x3e119c [[BR]]6 !__tmainCRTStartup crtexe.c 555 0x3e282f [[BR]]7 mainCRTStartup crtexe.c 371 0x3e265f [[BR]]8 !BaseThreadInitThunk kernel32 0 0x75303677 [[BR]]9 !RtlInitializeExceptionChain ntdll 0 0x770e9d72 [[BR]]10 !RtlInitializeExceptionChain ntdll 0 0x770e9d45 

Can somebody help me?

Many thanks!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/20/11 19:00:53
Moderators:
----------------------------------------------------------------------------

One problem that you might face here is that the startDiscovery function is not blocking so you will end up in an endless loop.
So you would better connect to the signals :
{{{
void    newDeviceFound (const QBtDevice &remoteDevice)
void    discoveryStopped ()
void    discoveryStarted ()
}}}
and try to handle the information.

Tip N.2: better call the functions that you want outside of the object constructor.

So to sum up, your code should more or less look like this:

Discoverer.h
{{{
#ifndef DISCOVERER_H
#define DISCOVERER_H
#include <QBluetooth.h>
#include <QList>
#include <QObject>

class Discoverer : QObject {
        Q_OBJECT
public:
        Discoverer(QObject* parent = 0);
        ~Discoverer();
        void discover();

public slots:
        void reportNewDeviceFound (const QBtDevice &remoteDevice);
        void discoveryStopped();

private:
        QList<QBtDevice>* devicesList;
        QBtDeviceDiscoverer* deviceDiscoverer;
};
#endif // DISCOVERER_H
}}}


and Discoverer.cpp
{{{
#include "discoverer.h"
#include <QDebug>
#include <iostream>


Discoverer::Discoverer(QObject* parent): QObject(parent)
{

}
Discoverer::~Discoverer()
{
        if(devicesList != NULL)
                delete devicesList;

        if(deviceDiscoverer != NULL)
                delete deviceDiscoverer;
}


void Discoverer::discover()
{
        devicesList = new QList<QBtDevice>();
        deviceDiscoverer = new QBtDeviceDiscoverer();
        qDebug() << "discovering ...";

        connect(deviceDiscoverer, SIGNAL(newDeviceFound (const QBtDevice &)),
                this, SLOT(reportNewDeviceFound (const QBtDevice &)));
        connect(deviceDiscoverer, SIGNAL(discoveryStopped()),
                this, SLOT(discoveryStopped ()));

        qDebug() << "sono nel while";
        deviceDiscoverer->startDiscovery();
        qDebug() << "startDiscovery chiamata";
}

void Discoverer::reportNewDeviceFound (const QBtDevice &remoteDevice)
{
        qDebug() << "New device found: " + remoteDevice.getName();
}

void Discoverer::discoveryStopped()
{
        devicesList->append(deviceDiscoverer->getInquiredDevices());
        qDebug() << "devices number"  << devicesList->size();

        if(devicesList->size() < 1)
                this->discover();

        qDebug() << "discovering finita";
}
}}}

