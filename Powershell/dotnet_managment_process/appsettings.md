** this script designed to changed in the appsettings like connection strings or any other changing but not frequently change
as example we used it in the production when the DB is damaged and get the new IP for the new cluster **

# the structure as the following:

appsetting.script
├── service1
     ├──appsettings.conf
├── service2
     ├──appsettings.conf
├── service3
     ├──appsettings.conf     

# the steps of script do the following:
1-stop the service (we can use wildcard to stop all services as the change will be for all services)
2-copy the appsettings from the above source to the serivce destination
3-start the service