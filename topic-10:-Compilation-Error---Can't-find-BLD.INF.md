----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 11:51:58
Moderators:
----------------------------------------------------------------------------

<Original written by "tech.spvn", moved from "How to compile QBluetooth">

Hey guy! I've tried compiled bluetooth library by Carbide, I've follow a  member of forum, but in carbide when building, I've some error:  "BLDMAKE ERROR: Can't find "\Symbian....\BLD.INF" And, this error at  follow image:  [[Image(http://i1039.photobucket.com/albums/a477/Go_Ahead/error1.jpg)]] Can you please explain me where i've make a mistakes? thanks alot!

----------------------------------------------------------------------------
Author:     favoritas37
Created:    12/16/10 12:10:13
Moderators:
----------------------------------------------------------------------------

As a reply to the question of '''tech.spvn'''...

The error that you are getting means that '''qmake'' '''''was not executed for your project.

A) Make sure Carbide is correctly configured. Check that the path of the Qt version that you are using is set correctly under "Window"->"Preferences"->"Qt"

B) Run '''qmake '''manually: "Project"->"Run qmake".

Now it should work :P
