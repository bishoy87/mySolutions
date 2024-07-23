**the solution here desinged for the following requirment:**
1- Export CSV or Ecel file
2- Send the file via email to the responsible person

**Technical Details**

├── Export Report from SQL
1- connected with sql by invoke-sqlcmd , we can make the connection string encryprted for security
2- piped the path of .sql file to -inputfile , or pass the quey to -query
3- piped to Export-Csv or Export-Excel (!! Export-Excel need to import its module) , specify the csv saved folder

├──Send the CSV by Mail
1-Specifiy the following variables:
a) files as empty array
b) csv saved folder
c) smtp credentials
d) for loop to get all any file in the files
e) Send-MailMessage and pass the required parameters like body ,subkect , to ,cc , smtp port