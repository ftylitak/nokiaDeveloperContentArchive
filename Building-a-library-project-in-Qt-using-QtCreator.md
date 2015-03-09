# Abstract 
This article shows how to create a shared DLL project using Qt Creator. The instructions include the extra step required for the Symbian platform of _freezing_ the API.

# Steps

* Create a new library project in Qt Creator as shown in the image below. Qt Creator generates all the needed files.
![](https://github.com/favoritas37/nokiaDeveloperContentArchive/blob/master/Building_a_library_project_in_Qt_using_QtCreator/CreatingNewLibraryProjectQtCreator.jpg)
* When compiling the project for Symbian, you will get a warning _Frozen .def file \QtSDKProjects\eabi\testLibrary.def not found - project not frozen_. This warning reminds you that you need to freeze the exports before release, so as to ensure that subsequent releases are backwards compatible.

# Freezing a project

## Note 
The Symbian Platform uses _link by ordinal_ (rather than _link by name_). In order to ensure that different versions of the library are compatible, functions have to be exported from the same ordinal position. We achieve this by storing ("freezing") the ordinal position of each function in a .def file, and using this file to define the function position each time the DLL is rebuilt. 

* To begin with, we need to open a command prompt whose environment will be configured for our target platform. For example, to do this on Windows 7 targeting Symbian^3 Qt 4.7.3, you can go under: _"Start"_ -> _"All programs"_ -> _"Qt SDK"_ -> _"Symbian^3 Qt 4.7.3"_ and open _Qt 4.7.3 for Symbian^3 Command Prompt_ 
![](https://github.com/favoritas37/nokiaDeveloperContentArchive/blob/master/Building_a_library_project_in_Qt_using_QtCreator/ConfiguredCommandPromptChangeDirectory.jpg)
* Once we have the Command Prompt, we change our current working directory to match the directory of the project we need to freeze.
* Then it's time to freeze the project. Type the following command:
* `abld freeze gcce` 
If the above operation is completed without errors you are done! 
