**Edit nssm services**
# In one of the projects I worked on, we had over 40 services on the testing and development server. The development team frequently needed to make edits to these services, so I created the following script to assist them:

# Define Menu Options:
1- The variable $menu is defined with a list of options representing different services. Each option is numbered, and the list is presented to the user.
2- The menu prompt ends with "Enter the number of service you want to edit".

# Loop for User Input:
1- A Do...Until loop is used to keep prompting the user for input until they decide to exit by entering "X".

# Read User Input:
1- The script reads the user input using Read-Host -Prompt $menu.

# Switch Statement for Selection:
1- A Switch statement is used to handle different cases based on the user's selection.
2- Each case corresponds to a number (1 to 31) and executes the nssm edit command for the respective service.
3- If the user enters a valid number, the script executes the command for the corresponding service.
4- If the user enters "X", the loop terminates, and the script exits.