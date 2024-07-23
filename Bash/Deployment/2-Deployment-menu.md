
**Technical Details for menu Scipt:**
**Main Components**

# Function to Display the Menu:

# show_menu:
- Lists all the scripts in the /opt/deployment.Scripts/ directory with numbered options.
- Displays an "Exit" option for the user to exit the script.
- Populates an array (script_list) with the paths to the scripts for easy reference when running them.

# Function to Run Selected Scripts:
**run_selected_scripts:**
- Accepts an array of selected choices as input.
- Iterates through the selected choices and executes the corresponding script if the choice is valid.
- Exits the script if the "Exit" option is chosen.
- Prints an error message for invalid choices.

# Main Loop for the Menu:
- Continuously displays the menu, prompting the user to enter their script selections or exit.
- Reads the user's input, splits it into an array of choices, and passes it to the run_selected_scripts function for execution.

# Detailed Workflow

# Show Menu:
- The show_menu function is called to display the available deployment scripts.
- The scripts are listed with numbered options, and an "Exit" option is provided.

# User Input:
- The user is prompted to enter the numbers corresponding to the scripts they want to run, separated by commas.
- The input is read and split into an array of choices using the IFS=',' read -ra selected_choices <<< "$choices" command.

# Run Selected Scripts:
- The run_selected_scripts function is called with the array of selected choices.
- For each choice:
   - If the choice matches the "Exit" option, the script exits.
   - If the choice corresponds to a valid script number, the script is executed using the bash "$selected_script" command.
   - If the choice is invalid, an error message is displayed.

# Repeat:
- The main loop continues, redisplaying the menu and prompting for input until the user chooses to exit.