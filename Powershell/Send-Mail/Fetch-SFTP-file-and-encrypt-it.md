**This is particularly useful in environments where regular, secure transfer and distribution of log files or data files are required, such as in compliance reporting, data analysis pipelines, or secure information sharing between teams or systems and we need to send this file in the mail compressed and encrypted**

# Retrieve Files from SFTP Server:
1-Reads a password from a file and converts it to a secure string.
2-Establishes an SFTP session with specified credentials.
3-Sets the source and destination directories for file transfer.
4-Filters and downloads files from the SFTP server that were modified today and have a .txt extension.
5-Outputs the full name of each downloaded file.

# Compress Files with Encryption:
1-Reads an encryption password from a file and converts it to a secure string.
2-Compresses all downloaded .txt files into a password-protected ZIP archive.
3-Names the ZIP archive with the current date.

# Clean Up Text Files:
1-Removes the original .txt files after compression.

# Send an Email with the ZIP Archive:
1-Checks if the ZIP archive exists.
2-Collects the full paths of the ZIP archive.
3-Sets up email credentials by reading another password file.
4-Sends an email with the ZIP archive as an attachment to specified recipients.

# Clean Up ZIP File:
1-Removes the ZIP archive after sending the email.