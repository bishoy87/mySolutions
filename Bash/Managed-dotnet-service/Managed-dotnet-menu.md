# Purpose
The primary goal of this script is to simplify the process of managing (Stopping, starting, Restarting) multiple services, and each status has a split script It provides a user-friendly interface to select and stop services, which can be particularly useful for system administrators or developers engineers who need to manage services efficiently.

**Steps**
# Initialization and Display Menu
- Function show_menu:
   - Lists all systemd unit files starting with "dotnet" in /etc/systemd/system/.
   - Displays them with numbered options for selection.
   - Populates an array (unit_list) with the unit filenames.
   - Adds an "Exit" option at the end of the menu.
- Purpose: Provides an easy-to-read list of services that can be stopped, making it simple for the user to select the desired services.

# User Input
- The user is prompted to enter the numbers corresponding to the services they want to stop, separated by commas.
- The input is read and split into an array of selected choices.

# Stopping, starting, restarting Selected Services
- Function stop_selected_services:
   - Processes the selected choices.
   - If "Exit" is chosen, the script exits.
   - For valid service numbers, the corresponding service is stopped using systemctl restart.
   - Invalid choices result in an error message.
- Purpose: Ensures that the selected services are stopped efficiently and provides feedback on the action taken.

# Loop
- The script loops back to display the menu again until the user chooses to exit.
- Purpose: Allows for repeated use without needing to restart the script, enhancing usability and efficiency.