**The menu script provides an interactive way to select and run deployment scripts.
The deployment script automates the detailed steps needed to deploy a package across multiple servers.
Together, they streamline the deployment process by allowing the user to easily select and execute complex deployment tasks.**

Workflow Overview
# Stage 1: Menu Script
- Displays a menu for selecting deployment scripts.
- Allows the user to choose and run specific deployment scripts.
# Stage 2: Deployment Script
- Executes deployment steps for a specified package on multiple servers.
- Manages extraction, backup, deployment, and restarting of services.

# Stage 1: Menu Script
**Objective: To provide an interactive menu for the user to select and run one or more deployment scripts.**

# Display Menu:
- The show_menu function lists all .sh scripts in the /opt/deployment.Scripts/ directory.
- The scripts are presented with numbered options for selection.
-An "Exit" option is provided to terminate the menu script.

# User Input:
- The user is prompted to enter the numbers of the scripts they wish to run, separated by commas.
- The input is read and split into an array of selected choices.

# Run Selected Scripts:
- The run_selected_scripts function processes the selected choices:
  - If "Exit" is chosen, the script exits.
  - For valid script numbers, the corresponding scripts are executed using the bash command.
  - Invalid choices result in an error message.

# Loop:
- The script loops back to display the menu again until the user chooses to exit.

# Stage 2: Deployment Script
**Objective: To automate the deployment process of a package across multiple servers.**

# Initialize Variables:
- Define variables for package name, directories, and current date.

# Extract Package:
- Extract the package tarball to the deployment directory.
- Check and print the status of the extraction.

# Loop Through Servers:
- For each server as example (10.0.0.20, 10.0.0.21, 10.0.0.22), perform the following steps:

# Backup:
- Create a backup of the current package version on the server.
- Check and print the status of the backup.

# Publish:
- Copy the new package from the deployment directory to the destination directory on the server.
- Check and print the status of the deployment.

# Restart Service:
- Restart the service on the server using systemctl.
- Check and print the status of the restart.

# Clean Up Workspace (Optional):
- Prompt the user to clean up the workspace by deleting the source and deployment directories.
- If confirmed, perform the cleanup and print the status.

# Integrated Workflow
**User Runs Menu Script:**
- The user executes the menu script, which displays the available deployment scripts.

**User Selects Deployment Script:**
- The user selects one or more deployment scripts to run.
**Menu Script Executes Deployment Script:**
The selected deployment scripts are executed in sequence by the menu script.

**Deployment Script Executes Deployment Steps:**
- The deployment script performs extraction, backup, deployment, and service restart steps on the specified servers.
Optionally, the user can clean up the workspace after deployment.

