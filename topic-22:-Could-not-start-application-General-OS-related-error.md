Topic #22 - "Could not start application: General OS-related error"
----------------------------------------------------------------------------
Author:     PGiZ
Created:    02/24/11 07:41:34
Moderators:
----------------------------------------------------------------------------

Hello everybody..[[BR]][[BR]]    I 'm newbies in Qt. I 'm need to develop app for communication on bluetooth.

I 'm test your 3rd party API with simple bluetooth discovery app. No error when build application

but on run in my mobile (Nokia 5800 !XpressMusic , OS9.4 5th) I got this error message.

{{{
Executable file: 9439 2011-02-24T12:03:30 C:\S60\devices\S60_5th_Edition_SDK_v1.0\epoc32\release\gcce\udeb\BTClient.exe

Package: 116548 2011-02-24T12:03:38 C:\QT_Project\BTClient\BTClient\BTClient.sis

Deploying application to 'Nokia 5800 XpressMusic USB Serial Port (COM8)'...

Copying installation file...

Installing application...

Starting application...

Could not start application: General OS-related error

Finished.

}}}
This development environment[[BR]]    [[BR]]    1. S60 5th SDK version 1.0[[BR]]    2. S60 5th SDK Plugin version 1.0_2[[BR]]    3. QT for Symbian  4.7.1 (Framework Only)[[BR]]    4. QT Creator 2.0.1[[BR]][[BR]]I 'm no problem when created other app. It 's run normally.[[BR]][[BR]]My Project

.pro file


{{{
QT       += core gui

TARGET = BTClient
TEMPLATE = app


SOURCES += main.cpp\
        mainwindow.cpp

HEADERS  += mainwindow.h

FORMS    += mainwindow.ui

CONFIG += mobility
MOBILITY =

symbian {
    TARGET.UID3 = 0xe45eae36
    # TARGET.CAPABILITY +=
    TARGET.EPOCSTACKSIZE = 0x14000
    TARGET.EPOCHEAPSIZE = 0x020000 0x800000
}

symbian{
       INCLUDEPATH += /epoc32/include/QBluetooth
       LIBS += -lQBluetooth

       customrules.pkg_prerules  = \
        ";QBluetooth" \
        "@\"$$(EPOCROOT)Epoc32/InstallToDevice/QBluetooth_selfsigned.sisx\",(0xA003328D)"\
        " "

        DEPLOYMENT += customrules
}
}}}
[[BR]]mainwindows.h

{{{
#include <QMainWindow>
#include <QBluetooth/QBluetooth.h>
#include <QBluetooth/QBtDeviceDiscoverer.h>
#include <QBluetooth/QBtDevice.h>

namespace Ui {
    class MainWindow;
}

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = 0);
    ~MainWindow();

private:
    Ui::MainWindow *ui;
    QBtDeviceDiscoverer* deviceDiscoverer;

private slots:
    void on_pushButton_Stop_clicked();
    void on_pushButton_Start_clicked();
    void newDeviceFound (QBtDevice& remoteDevice);
};
}}}
[[BR]]mainwindows.cpp

{{{
MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    deviceDiscoverer = new QBtDeviceDiscoverer(this);

    // Connect Signal
    connect(deviceDiscoverer , SIGNAL(newDeviceFound (QBtDevice)),
            this, SLOT(handlerFunction(QBtDevice)));
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::on_pushButton_Start_clicked()
{
    if(deviceDiscoverer->isBusy())
        return;

    deviceDiscoverer ->startDiscovery();
}

void MainWindow::on_pushButton_Stop_clicked()
{
    if(deviceDiscoverer->isBusy())
        deviceDiscoverer->stopDiscovery();
}

void MainWindow::newDeviceFound (QBtDevice &remoteDevice)
{
    ui->listWidget->addItem(remoteDevice.getName());
}
}}}
[[BR]][[BR]]I 'm think because your pre-binary package not support OS9.4 5th ?[[BR]]

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/24/11 12:33:00
Moderators:
----------------------------------------------------------------------------

> I 'm think because your pre-binary package not support OS9.4 5th ?

No its not that, the binaries where tested at S60 3rd FP2, S60 5th and Symbian 3!^....

May be you have to define the target capabilities:

{{{
TARGET.CAPABILITY += Location
        LocalServices \
        NetworkServices \
        ReadUserData \
        UserEnvironment \
        WriteUserData
}}}

----------------------------------------------------------------------------
Author:     PGiZ
Created:    02/24/11 15:20:12
Moderators:
----------------------------------------------------------------------------

Thanks for quick reply.[[BR]][[BR]]    OK , I defined target capabilities and rebuild project. [[BR]]and error again. - -[[BR]]    In QBluetooth article page , I try to run !QuteMessager Example App[[BR]]I found .sisx in project directory. I used this file to install and got error message [[BR]]on mobile screen. [[BR]][[BR]]    "Program use QT 4.7 or later"[[BR]][[BR]]Umm. I found 'QT_Install.sis' and 'QT.sis' in [[BR]][[BR]]    !ftp://ftp.qt.nokia.com/qt/symbian/4.7.1/ [[BR]][[BR]]and run QT.sis first and QT_Install.sis later. I don't know what differrence of this 2 files. :P[[BR]][[BR]]QT.sis update QT from 4.6 to 4.7.[[BR]]QT_Installer update Qt ,PIPS Installer , Open C LIPSSL , Install Standdard C++ , QT Webkit etc. [[BR]][[BR]]And go back to test project discovery bluetooth. Program run success :)[[BR]]

----------------------------------------------------------------------------
Author:     favoritas37
Created:    02/24/11 15:50:21
Moderators:
----------------------------------------------------------------------------

Nice, glad to hear it works! :)

