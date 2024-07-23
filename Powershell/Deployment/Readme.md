# Script Ideas and Descriptions

**Module usage in this solution:**
-Posh-SSH
-7Zip4Powershell
-psmenu
-CopyWithPogress
-for authentication use sftp key

**Environments:**
Testing
Production/Staging

**Steps of process first stage:**
1-first stage dedicated to download the pakage and upload for the prodcution environment
2-combined all scripts let's say we have multiple API's we need to crreate script of each one 
so we will use the psmenu to combined this scripts with multiselect feature
3-after the user select the desired script will do the following work in the test server
which is has the package we need to upload to production:
a) compressed the package
b) download the package
c) in case we have change in settings will uncompress the package ,chaneg settings by using "get-content and replace"
c) Upload the package to the production sverer which is will be the stage of publish to all servers in the environments

**Steps of process second stage:**
1-second stage dedicated for take a backup_(with date&time) from current package then publish the new release on all servers
a) we take backup in the same server (app server) and take another backup in the server what we deployed from !! becasue we will use this backup in coming process of rollback
2-Expand the ZIP Package
3-put all servers in for loop and execute the following:
a) stop the service "we have here -Erroraction Stop to stop copying process in case the service refused to stop"
b) start copy process with execluding some file like appsettings
c) start the service
4-cleaning up the workspace of deployment

**Steps of process Rollback:**
1-in case we need to rollback for any reasone we will use the same script in **Steps of process second stage:** we will use the backup we taked in point "1- a)" in **Steps of process second stage:**
and we will follow the same steps