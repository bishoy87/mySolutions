**in this case we need the support team to check the result of specific query by sending this result from the mail inbox**
1- invoke-sqlcmd with -Query
2- get the result from the sql and piped to |Format-table and | out-string
3-scheduled this task as your choice