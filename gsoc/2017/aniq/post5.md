# DSL to Python Translator

July 14, 2017


As mentioned in the previous post, I have written the code for the DSL to Python translator. The code is working fine and has been merged with the master branch. 😀

The structure is very simple, it interprets each line of the program written in DSL and checks if it has a valid command. If it has a valid command, the line is processed and it’s arguments are identified and its python-equivalent code is generated and written in the python file for that program.

In case an invalid command is entered, it is ignored (for now).

## Automation Scripts

The translation process has been automated as mentioned in [Getting rid of redundancy](https://wordpress.com/post/uniquevolution.wordpress.com/272) and the automation scripts are included in `learnbot/learnbot-dsl/bin/`

Added two separate scripts to create and run a program written in DSL.
The two files are:
**learnbot_create**: to generate the python file from the file written in DSL
**learnbot_run**: to run the generated python file
These *custom bash commands* can be used from any directory
### Installation

    $ gedit ~/.bashrc

Then add the following line at the end of it

    export PATH=$PATH”:$HOME/robocomp/components/learnbot/learnbot-dsl/bin”

Then reload the *.bashrc* file

    $ . ~/.bashrc

### Using the commands

The commands can now be used from any directory.
Suppose you have the following file written in DSL: `/path/to/file/<filename>`
cd into /path/to/file/ then
To generate the python file, the command is: `learnbot_create <filename>`
To run the <filename>.py file generated in `~/robocomp/components/learnbot/learnbot-dsl/` , simply type: `learnbot_run <filename>`

## Work ahead

The DSL to Python translator isn’t complete without an error reporter. I will be writing a help page which will list down all the valid learnbot commands and it’s correct usage. The user will just type `learnbot-help` and the help page will come up in the terminal.
Everytime an invalid command is detected, the translator will stop and report the error, saying:

    Invalid command. Type learnbot-help for more info on commands and their correct usage.

* * *
Aniq Ur Rahman