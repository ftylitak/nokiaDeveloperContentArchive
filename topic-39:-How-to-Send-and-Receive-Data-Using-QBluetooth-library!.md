Topic #39 - How to Send and Receive Data Using QBluetooth library!
----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    06/28/11 17:44:22
Moderators:
----------------------------------------------------------------------------

Hi,

I want to send data from mobile1 to mobile2 and later want to receive data from mobile2 to mobile1. After installation of my sis file in mobile1, I am able to connect and pair from mobile1 to mobile2. But I am getting the following warnings like:

''20:02:09Debug: file:  "/Carbide/NewLibVersion/QBluetooth_selfsigned/Connection/SerialPort/Client/Impl/qbtserialportclient_symbian.cpp"  line:  106   "warning: trying to connect and state != ENone"

20:02:09Debug: file:  "/Carbide/NewLibVersion/QBluetooth_selfsigned/Connection/SerialPort/Client/Impl/qbtserialportclient_symbian.cpp"  line:  106   "warning: trying to connect and state != ENone"

20:02:09Debug: file:  "/Carbide/NewLibVersion/QBluetooth_selfsigned/Connection/SerialPort/Client/Impl/qbtserialportclient_symbian.cpp"  line:  106   "warning: trying to connect and state != ENone"''


Now after pairing I want to send some data like:

QByteArray data = { 0xb1, 0x00, 170, 0x00, 65, 0, 28, 01, 21, 0xbb }; from mobile1 to mobile2...

I read the doc of "QBtSerialPortClient.h" and found the followings:

{{{
public slots:

    /**
     * sendData()
     * Send a string to the server.
     * If text is transmitted, it is up to the user to decide which text encoding to use.
     * Upon succesfull transmittion, dataSent() signal is emitted.
     */
    void sendData(const QString& data);

    /**
     * Send a array of bytes to the server, as is.
     * Upon succesfull transmittion, dataSent() signal is emitted.
     */
    void sendData (const QByteArray & data);

signals:


    /**
     * Emitted when after sendData(QString) is called, if the data is send
     * successfully.
     */
    void dataSent();

    /**
     * Emitted as feedback when data are received successfully.
     */
    void dataReceived(const QString & data);

}}}

How can I use these Signals and Slots for communication from mobile1 to mobile2 and vice versa.

Please let me know.

Thanks...

----------------------------------------------------------------------------
Author:     favoritas37
Created:    06/28/11 18:38:19
Moderators:
----------------------------------------------------------------------------

This warning means that you are trying to connect to the other device at Serial Port Profile more than once at the same time. I would suppose that you call it once, it get connected or is trying to connect and at the same time you call it 3 more times somewhere of some reason.

Now about how to use the classes...i would suggest you take a look at the following project: http://www.developer.nokia.com/Community/Wiki/QuteMessenger_-_Bluetooth_Chat

It does what you say. 2 (or more devices) connect and exchange data through Serial Port Profile.

Thing that you have to be careful with:

 * One device has to have an instance of Serial Port Server ([http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_server.html QBtSerialPortServer]) and the other device an instance of Serial Port Client ([http://www.csd.uoc.gr/~ftylitak/QBluetooth_repo_doc/class_q_bt_serial_port_client.html QBtSerialPortClient])
 * By been connected to signal ''dataReceived(const QString &)  ''you will be able to receive all the incoming messages sent by the other device.
 * By calling the function ''sendData(const QString&) ''you send the data you want to the other device.

It's as simple as that. If get successfully connected then it is trivial to exchange data.

Once again, spend some time looking at the example that i have sent you. In ''!QuteMessenger.cpp'' you will find the code for the creation of a Serial Port Server and in QChatWidgetClient.cpp there is the code for Serial Port Client usage.

Regards

----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    06/29/11 18:41:07
Moderators:
----------------------------------------------------------------------------

Thanks favoritas37!

I tried as per your suggestion in the following way...

I reimplemented the Client.cpp like the following:

{{{
#include "Client.h"
#include <QBtAuxFunctions.h>

Client::Client(QWidget* parent)
    : QWidget(parent), device(NULL), service(NULL), client(NULL)
{
}

Client::~Client()
{
    if(client)
    {
        disconnect(client, SIGNAL(connectedToServer()));
        disconnect(client, SIGNAL(disconnectedFromServer()));
        disconnect(client, SIGNAL(dataReceived(const QString)));
        disconnect(client, SIGNAL(error(QBtSerialPortClient::ErrorCode)));
    }

    SafeDelete(client);
    SafeDelete(device);
    SafeDelete(service);
}

void Client::StartSerialPortClient()
{
    SafeDelete(client);
    client = new QBtSerialPortClient(this);

    connect(client, SIGNAL(connectedToServer()),
            this, SLOT(connectedToServerReport()));

    connect(client, SIGNAL(disconnectedFromServer()),
            this, SLOT(disconnectedFromServerReport()));

    connect(client, SIGNAL(dataReceived(const QString)),
            this, SLOT(dataReceivedReport(const QString)));

    connect(client, SIGNAL(error(QBtSerialPortClient::ErrorCode)),
            this, SLOT(errorReport(QBtSerialPortClient::ErrorCode)));

    client->connect(*device, *service);
}

void Client::setParameters(QBtDevice dev, QBtService serv)
{
    device = new QBtDevice(dev);
    service = new QBtService(serv);

    StartSerialPortClient();
}

void Client::connectedToServerReport()
{
    qDebug() << "---Client Device Connected Successfully to Server Device---";
}

void Client::disconnectedFromServerReport()
{
    qDebug() << "---Client Device Disconnected Successfully from Server Device---";
}

void Client::dataReceivedReport(const QString data)
{
    QString str("-> ");
    str += data;
    qDebug() << "Data received are: " << str;
}

void Client::errorReport(QBtSerialPortClient::ErrorCode code)
{
    QString str("--Error occurred: ");
        str += code;
    qDebug() << str;
}

//Called to send data typed in QLineEdit
void Client::sendMessageRequest(QByteArray dataStr)
{
//    const QString data = QString::fromUtf16(dataStr.utf16());
    if(client)
    {
        client->sendData(dataStr);
//        QString sendingString("<- ");
        QString sendingString (dataStr);
    qDebug() << "Sending String: " << sendingString;
    }
}

}}}

My main screen where i want to connect and send data to other device is like the following:


{{{
#include "Home.h"
#include "ui_Home.h"

//Bluetooth related
#include <QMovie>
#include <QBtAuxFunctions.h>
#include "Client.h"
Home::Home(QWidget *parent) :
    QMainWindow(parent),rfcommServer(NULL), devDisc(NULL), serviceDisc(NULL),
    dialog(NULL),
    ui(new Ui::Home)
{
    ui->setupUi(this);

    //Bluetooth related functionality
    //setup devSearchButton signal handler
    connect(ui->buttonSearchDevices, SIGNAL(clicked()), this, SLOT(startDeviceDiscovery()));

    connect(ui->listWidgetDevices, SIGNAL(itemActivated(QListWidgetItem*)), this,
        SLOT(deviceSelected(QListWidgetItem*)));

    connect(ui->listWidgetDevices, SIGNAL(itemClicked(QListWidgetItem*)), this,
        SLOT(deviceSelected(QListWidgetItem*)));

    InitializeBluetooth();
    pClient = new Client(this);
}

Home::~Home()
{
    if (rfcommServer) {
        disconnect(rfcommServer, SIGNAL(clientDisconnected()));
        disconnect(rfcommServer, SIGNAL(clientConnected(QBtAddress)));
        delete rfcommServer;
    }
    if (devDisc) {
        disconnect(devDisc, SIGNAL(newDeviceFound(QBtDevice)));
        disconnect(devDisc, SIGNAL(discoveryStopped()));
        delete devDisc;
    }
    if (serviceDisc)
        delete serviceDisc;
    delete ui;
}

void Home::on_buttonStart_clicked()
{
    ui->homeWidget->hide();
    ui->HRMonitorWidget->showFullScreen();
    startSendingData();
}

//////////////////////////////////////////////////
/////////Bluetooth related functionality//////////
//////////////////////////////////////////////////
void Home::SaveSelectedDevice()
{
    //Get the selected device
    for (int i = 0; i < foundDevices.size(); i++) {
        if (foundDevices[i].getName() == selectedDeviceName) {
            deviceQueriedForServices = foundDevices[i];
            break;
        }
    }
}

void Home::InitializeBluetooth()
{
    // power on bluetooth
    QBtLocalDevice::askUserTurnOnBtPower();

    const QBtUuid uid = QBtUuid(QBtConstants::SerialPort);

    // start rfcomm server
    rfcommServerServiceName = QString("Messenger Protocol ");
    rfcommServerServiceName += QBtLocalDevice::getLocalDeviceName();

    rfcommServer = new QBtSerialPortServer(this);
    rfcommServer->startServer(uid, rfcommServerServiceName);


    connect(rfcommServer, SIGNAL(clientDisconnected()),
            this, SLOT(restartSerialPortServer()));

    connect(rfcommServer, SIGNAL(clientConnected(QBtAddress)),
            this, SLOT(SetupServer(QBtAddress)));
    //create an instance of BT device discoverer
    devDisc = new QBtDeviceDiscoverer(this);

    //service discoverer
    serviceDisc = new QBtServiceDiscoverer(this);
}

void Home::restartSerialPortServer()
{
        const QBtUuid uid = QBtUuid(QBtConstants::SerialPort);
    rfcommServer->startServer(uid, rfcommServerServiceName);
}

void Home::SetupServer(QBtAddress address)
{
    Server* tmpServer;
    tmpServer = new Server(this);

    tmpServer->SetDiscoverer(devDisc);

    tmpServer->SetServer(rfcommServer);
}

void Home::startDeviceDiscovery()
{
    if (devDisc) {
        ui->listWidgetDevices->clear();
        foundDevices.clear();
        connect(devDisc, SIGNAL(newDeviceFound(QBtDevice)), this,
            SLOT(populateDeviceList(QBtDevice)));
        connect(devDisc, SIGNAL(discoveryStopped()), this, SLOT(deviceDiscoveryCompleteReport()));
        devDisc->startDiscovery();

        dialog = new QProgressDialog("Searching devices...", "Stop", 0, 0, this);
        dialog->setWindowModality(Qt::WindowModal);
        connect(dialog, SIGNAL(canceled()), this, SLOT(deviceDiscoveryCompleteReport()));

        dialog->show();
    }
}

void Home::populateDeviceList(QBtDevice newDevice)
{
    ui->listWidgetDevices->addItem(newDevice.getName());
    foundDevices.append(newDevice);
//    SaveSelectedDevice();
}

void Home::deviceDiscoveryCompleteReport()
{
    disconnect(devDisc, SIGNAL(newDeviceFound(QBtDevice)), this,
        SLOT(populateDeviceList(QBtDevice)));
    disconnect(devDisc, SIGNAL(discoveryStopped()), this, SLOT(deviceDiscoveryCompleteReport()));

    if (dialog) {
        dialog->hide();
        delete dialog;
    }
}

void Home::deviceSelected(QListWidgetItem* item)
{
    selectedDeviceName = item->text();
    SaveSelectedDevice();
    connectionhandlerFunction(deviceQueriedForServices);
}

void Home::connectionhandlerFunction(const QBtDevice& d)
{
    connect (serviceDisc, SIGNAL(newServiceFound(const QBtDevice&, const QBtService&)),
    this, SLOT (ConnecttoDevice(const QBtDevice&, const QBtService& )));
    serviceDisc->startDiscovery (d);
}

void Home::ConnecttoDevice(const QBtDevice& d,const QBtService& ser)
{
    pClient->client->connect(d, ser);'''//Crashes Here'''
}

void Home::startSendingData()
{

    connect(this, SIGNAL(serialParamRetreived(QBtDevice, QBtService)), pClient,
        SLOT(setParameters(QBtDevice, QBtService)));

    connect(serviceDisc, SIGNAL(newServiceFound(const QBtDevice&, const QBtService&)), this,
        SLOT(newSerialServiceFound(const QBtDevice&, const QBtService&)));

    const QBtUuid uid(QBtConstants::SerialPort);
    serviceDisc->startDiscovery(deviceQueriedForServices, uid);

    QByteArray ba;
    ba.resize(10);

    const int bHeight = 172;
    const int bWeight = 123;
    const int bSexType = 0;
    const int bAge = 50;
    const int bMonth = 01;
    const int bDay = 21;

    ba[0] = 0xb1;
    ba[1] = 0x00;
    ba[2] = bHeight;
    ba[3] = 0x00;
    ba[4] = bWeight;
    ba[5] = bSexType;
    ba[6] = bAge;
    ba[7] = bMonth;
    ba[8] = bDay;
    ba[9] = 0xbb;

    QString str(ba);
    qDebug() << str;

    pClient->sendMessageRequest(ba);
}

void Home::newSerialServiceFound(const QBtDevice& dev, const QBtService& serv)
{
    QString exprectedServiceName("Messenger Protocol ");
    exprectedServiceName += dev.getName();

    if (serv.getName() == exprectedServiceName)
        {
        serviceDisc->stopDiscovery();

        disconnect(serviceDisc, SIGNAL(newServiceFound(const QBtDevice&, const QBtService&)),
                                        this, SLOT(newSerialServiceFound(const QBtDevice&, const QBtService&)));
        disconnect(this, SIGNAL(serialParamRetreived(QBtDevice, QBtService)));

        selectedService = serv;
        emit serialParamRetreived(dev, serv);
    }
}
}}}

But the above code crashes when I tried to connect.
How could I connect, pair and then send data to other device.

Please help me.

----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    07/05/11 09:40:20
Moderators:
----------------------------------------------------------------------------

Hi favoritas37,

I am little confused. Actually I have a hardware device which emits Bluetooth data in hexadecimal format. And I am developing an Mobile App which will first pair with the hardware device and will receive the hexadecimal Bluetooth data continuously emitted by that hardware device. So I guess I don't have to implement the Server side code. Only the Client that I have implemented is sufficient for the communication between my Mobile App and the hardware device.

So as per my idea I just implemented the Client with following Signal/Slots....

{{{
    client = new QBtSerialPortClient(this);

    connect(client, SIGNAL(connectedToHW()),
            this, SLOT(connectedToHWrReport()));

    connect(client, SIGNAL(disconnectedFromHW()),
            this, SLOT(disconnectedFromHWReport()));

    connect(client, SIGNAL(dataReceived(const QString)),
            this, SLOT(dataReceivedReport(const QString)));

    connect(client, SIGNAL(dataSent()),
            this, SLOT(dataSentReport()));

    connect(client, SIGNAL(error(QBtSerialPortClient::ErrorCode)),
            this, SLOT(errorReport(QBtSerialPortClient::ErrorCode)));
}}}

So that after successful pairing I'll get the Data from the Hardware device like:

{{{
void Client::dataReceivedReport(const QString data)
{
    int nLen = data.length();
    QString str("-> ");
    str += data;
    qDebug() << "Data received are: " << str; '''// Getting Garbage data'''
}
}}}

As per the code I am able to receive the data but not able to convert the QString data, as the Hardware device gives the data in Hexadecimal form and I am trying to display it in String format.

How could I convert this thing. Also kindly let me know if my approach is correct or not.

Thanks...

----------------------------------------------------------------------------
Author:     favoritas37
Created:    07/05/11 12:56:10
Moderators:
----------------------------------------------------------------------------

Yes your approach is correct.

About the heximal data, you can get them with various ways. I just have one question. The data received by the device are readable strings? or just heximal data that you want to print them to the screen?

To begin with you can do the following to convert every byte to a heximal string:

{{{
QString hexStr = QString();

 for(int i=0; i<data.length(); i++)
     hexStr +=  "0x" + QString("%1").arg(data.at(i).unicode(),0,16) + ", ";

cout << hexStr.toAscii().data() << endl;
}}}

----------------------------------------------------------------------------
Author:     rajeevsahu
Created:    07/05/11 15:56:58
Moderators:
----------------------------------------------------------------------------

Thanks. The code snippet works fine.

Now when should I send Data to the Hardware device? Actually the hardware device operates on few commands in hexadecimal form. So I want to send some data. I have implemented following function in Home.cpp

{{{
void Home::SendDataCommand()
{
    QByteArray ba;
    ba.resize(10);
    const int nHeight = 172;
    const int nWeight = 123;
    const int nSexType = 0;
    const int nAge = 50;
    const int nMonth = 01;
    const int nDay = 21;
    ba[0] = 0xb1;
    ba[1] = 0x00;
    ba[2] = nHeight;
    ba[3] = 0x00;
    ba[4] = nWeight;
    ba[5] = nSexType;
    ba[6] = nAge;
    ba[7] = nMonth;
    ba[8] = nDay;
    ba[9] = 0xbb;
    QString str(ba);
    qDebug() << str;

        if (pClient)
        {
                 pClient->sendMessageRequest(ba);
        }
}
}}}

And following function  in Client.cpp:

{{{
void Client::sendMessageRequest(QByteArray dataStr)
{
    if(client)
    {
        client->sendData(dataStr);
    }
}
}}}

in return it will again emit some different data, which I'll receive in Client::dataReceivedReport(const QString data) function. So I just want to know when should I call that '''SendDataCommand()''' function to send data to hardware device???

Any ideas...

----------------------------------------------------------------------------
Author:     favoritas37
Created:    07/05/11 17:17:18
Moderators:
----------------------------------------------------------------------------

I think this question has to be answered by you. It really depends on what you want to do, what your device does and what you want to achieve by sending those data to the device. So at least from our side there is no restriction when to call the sendData function as long as the devices are connected.
