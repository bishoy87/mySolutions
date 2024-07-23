**Technical Details for menu Scipt:**
**Overall Workflow**
- Extract the package.
- For each server:
   - Backup the current package.
   - Deploy the new package.
   - Restart the service.
- Optionally clean up the workspace based on user input.
- This script automates the deployment process, ensuring consistency and reducing the chances of manual errors.

# Variables
- package_name="MCDR.mob.account": The name of the package to be deployed.
- deployment_dir="/opt/deploymentcenter": Directory where the package will be temporarily deployed.
- source_dir="/opt/deploymentcenter/$package_name.tar.gz": The source location of the package tar.gz file.
- destination_dir="/usr/MCDR.Services": The target directory on the servers where the package will be deployed.
- backup_dir="/usr/MCDR.Backup": Directory where backups of the current package version will be stored.
- backup_package="/usr/MCDR.Services/$package_name": Path to the current version of the package to be backed up.
- current_date=$(date +"%Y-%m-%d_%H_%M_%S"): Timestamp to tag the backup.

# Extract Package
- Extracts the tar.gz package to the deployment directory.
- Checks if extraction was successful and prints the appropriate message.

# Loop Through Servers
For each server in the list (10.0.2.11, 10.0.2.12, 10.0.2.13), the script performs the following steps:
- Backup:
   - Creates a backup of the current package version on the server.
   - Checks if the backup was successful and prints the appropriate message.
- Publish:
   - Copies the extracted package from the deployment directory to the destination directory on the server using scp.
   - Checks if the publish was successful and prints the appropriate message.
- Restart the Service:
   - Restarts the service on the server using systemctl.
   - Checks if the restart was successful and prints the appropriate message.
# CleanUP Workspace
- Asks the user if they want to clean up the workspace.
- If the user agrees, it deletes the source and deployment directories and prints the appropriate message based on the success of the operation.
# Functions
- prompt_yes_no(): A function to prompt the user with a yes/no question and return 0 for yes and 1 for no.

