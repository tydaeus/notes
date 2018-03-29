# Windows Startup Programs

On Windows 7-10, startup programs are located in `c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`.

On Windows XP, startup programs are located in `c:\Documents and Settings\All Users\Start Menu\Programs\Startup`.

Startup programs can also be set by adding values to reg keys:

* `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run` to run repeatedly
* `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce` to run only once