# Cheatsheet

Good article to learn bash - https://ryanstutorials.net/bash-scripting-tutorial/

- When referring to or reading a variable we place a $ sign before the variable name.
- When setting a variable we leave out the $ sign.


Special Variables:

$0 - The name of the Bash script.
$1 - $9 - The first 9 arguments to the Bash script. (As mentioned above.)
$# - How many arguments were passed to the Bash script.
$@ - All the arguments supplied to the Bash script.
$? - The exit status of the most recently run process.
$$ - The process ID of the current script.
$USER - The username of the user running the script.
$HOSTNAME - The hostname of the machine the script is running on.
$SECONDS - The number of seconds since the script was started.
$RANDOM - Returns a different random number each time is it referred to.


- $1, $2, ...
The first, second, etc command line arguments to the script.

- variable=value
To set a value for a variable. Remember, no spaces on either side of =

- Quotes " '
 Double will do variable substitution, single will not.

- variable=$( command )
  Save the output of a command into a variable

- export var1
Make the variable var1 available to child processes.

### Formatting:
The presence or absence of spaces is important.
Manageability
If a particular value is used several times within a script (eg a file or directory name) then using a variable can make it easier to manage.


### if statement

if [ <some test> ]
then
<commands>
fi

### if else

if [ <some test> ]
then
<commands>
else
<other commands>
fi

### if elif else

if [ <some test> ]
then
<commands>
elif [ <some test> ]
then
<different commands>
else
<other commands>
fi

### while loop

while [ <some test> ]
do
<commands>
done

### for loop

for var in <list>
do
<commands>
done

We can put range as list like this:

for size in {1..5}


while do done
Perform a set of commands while a test is true.

until do done
Perform a set of commands until a test is true.

for do done
Perform a set of commands for each item in a list.

break
Exit the currently running loop.

continue
Stop this iteration of the loop and begin the next iteration.

select do done
Display a simple menu system for selecting items from a list.
