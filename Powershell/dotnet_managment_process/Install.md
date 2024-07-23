**batch file for installing dotnet service by utilized nssm (the Non-Sucking Service Manager)**

# the below is .bat file
```console
@echo off

set "params=%*"
cd /d "%~dp0" && ( if exist "%temp%\getadmin.vbs" del "%temp%\getadmin.vbs" ) && fsutil dirty query %systemdrive% 1>nul 2>nul || (  echo Set UAC = CreateObject^("Shell.Application"^) : UAC.ShellExecute "cmd.exe", "/k cd ""%~sdp0"" && %~s0 %params%", "", "runas", 1 >> "%temp%\getadmin.vbs" && "%temp%\getadmin.vbs" && exit /B )

Title Install Windows Service 

:step1
Echo *******************MAIN MENU********************
Echo 1) Install service with argument
Echo 2) Install API 
Echo 3) Exit

Echo.
Echo.
Echo Please type your choice
set /p reply=">"

if %reply% equ 1 goto Install_service_with_argument
if %reply% equ 2 goto Api
if %reply% equ 3 goto Exit.cmd
if %reply% neq 1 goto Exit.script
if %reply% neq 2 goto Exit.script
if %reply% neq 3 goto Exit.script



:Install_service_with_argument
echo Please Enter the Service Name
set /p servicename=">"
echo Please Enter the Argument
set /p argument=">"

nssm install %servicename% "C:\Program Files\dotnet\dotnet.exe"  "dotnetservice.dll %argument%" 
nssm set %servicename% AppDirectory "C:\Users\<user>\Desktop\<dotnetservicedirectory>"
goto return


:API
echo Please Enter the Service Name
set /p servicename=">"
echo Please Enter the Api Location Path
set /p Api.Path=">"

nssm install %servicename% "%Api.path%\startup.bat" 
goto return


:return
Echo Did you need to install another service? y or n
set /p choice=">"
if %choice% equ y goto step1
if %choice% equ n goto Exit

:Exit.script
cls
goto step1
pause

:Exit.cmd
exit
)
```
# End of script


