# Manage dotnet in ubuntu
# startup.sh to running Running a .NET Service:
```console
cd /usr/dotnet.Services/mob.login && dotnet  Services.Login.API.Mobile.dll --urls http://*:8104
```
# create an aliase The alias provides a convenient shorthand for starting the service. This can save time and reduce errors for system administrators or developers who frequently start and stop the service and investigate if nedeed to run it as a console
```console
alias moblog='/usr/dotnet.Services/mob.login/startup.sh'
```
# install dotnet as a systemd service

```console
[Unit]
Description=mob.login

[Service]
Type=simple
WorkingDirectory=/usr/dotnet.Services/mob.login
ExecStart=dotnet MCDRServices.Login.API.Mobile.dll --urls http://*:8104
TimeoutSec=0

[Install]
WantedBy=multi-user.target

```