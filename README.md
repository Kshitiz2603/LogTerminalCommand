Logging Terminal Commands along with it's date time and virtual environment if present.


### Steps to Implement**

1. Create a Separate File for the Logging Script:**
   - Create a file named `~/.zsh_logging` (or another name of your choice) to store the logging logic.
   - Open it with a text editor:
     nano ~/.zsh_logging

   - Add the logging logic:

     LOG_FILE=~/Command.txt
  
     log_command() {
     
         # Get the full path of the command using which
         local command_path
         command_path=$(which "$1" 2>/dev/null)
     
         # If the command is not found, skip logging
         if [ -z "$command_path" ]; then
             return
         fi
         # Get the current date and time
         local date_time
         date_time=$(date "+%Y-%m-%d %H:%M:%S")
         # Check for an active virtual environment
         local venv_name
         if [[ -n "$VIRTUAL_ENV" ]]; then
             venv_name=$(basename "$VIRTUAL_ENV")
         else
             venv_name="No virtual environment"
         fi
         # Log the information
         {
             echo "Command: $command_path"
             echo "Date/Time: $date_time"
             echo "Virtual Environment: $venv_name"
             echo "-----------------------------------"
         } >> "$LOG_FILE"
     }
     
     preexec() {
         log_command "$1"
     }
     

   - Save and close the file.

2. Source the Script from `~/.zshrc`:
   Add the following line to your `~/.zshrc` to include the logging logic:

   source ~/.zsh_logging


3. Reload Your Shell Configuration:**
   Reload your `~/.zshrc` so the changes take effect:
  
   source ~/.zshrc


4. Test the Setup:
   - Run some commands in your terminal, like `ls` or `python --version`.
   - Check the `Command.txt` log file in your home directory:
   - 
     cat ~/Command.txt

---

### Advantages of This Approach**
- Cleaner `~/.zshrc`: By moving the logic to a separate file, your main configuration file remains more readable and organized.
- Reusability: The logging script can be reused or sourced in other configurations if needed.
- Easy Maintenance: If you need to modify or debug the logging logic, it’s easier to do so in a dedicated file.

---
