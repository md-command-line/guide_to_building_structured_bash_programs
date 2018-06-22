# scalable_bash_programs
What you need to know to build structured bash programs. A few lessons learned.

## As often as possible include set on Debug mode for better transparency.
```bash
# set -e provides more output that can help with testing your program.
set -e
```

## Getting past the first file, calling other scripts:
Running a script from afar is easy, 
Get the current directory name: [source](https://www.linuxquestions.org/questions/programming-9/bash-how-to-get-current-workin-directory-92961/)
```bash
DIR=${PWD##*/}; echo $DIR; 
# means "PWD with the first part removed, up to, and including the last slash.
```

<br/>The first challenge a developer runs into is trying to run another script that is nearby the script you are running from afar. 

Scripts know the location of the person running the script 
<br/>but are not automatically self aware of their own location.

This line of code ensures that your script will know its current location.[source](https://stackoverflow.com/questions/59895/getting-the-source-directory-of-a-bash-script-from-within)
```bash
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )";
```
relative file locations are now accessable
```bash
ls $DIR/../; 
```

## Calling a script or executable is as simple as:
./$DIR/script
open $DIR/.app #-mac

## Waiting for a script to complete.
I know this guide is about bash but its worth considering javascript.
The most elegant solution I have seen for this might be using javascript promises to await the completion of the program .then(run_the_next_script)

sh-exec is javascript library that is worth checking out.

There is a way in bash called autoexpect that I am not overly familiar with in bash. [source](https://stackoverflow.com/questions/7197527/bash-script-how-can-i-wait-for-certain-output-from-a-process-then-continue)

For operations dependent on a user, I have stored information in a hidden file fired a notification every five seconds and every second checked if the file existed.

```bash
adaptive_wait () { # bash function for macintosh notifications. Checking presence of file every second.
TIMEOUT=25;
  until [ -d ~/Library/Screen\ Savers/$screen$saver ] || [ -d /Library/Screen\ Savers/$screen$saver ] || [ $TIMEOUT -eq 0 ] ; do
    if ! ((TIMEOUT % 5)); then
      osascript -e 'display notification "To allow for Decision/User Credentials" with title "Application Waiting"';
    fi
    sleep 1;
    TIMEOUT=$[$TIMEOUT-1];
  done
}
```

## Testing, 
Calling functions from another file: [source](https://stackoverflow.com/questions/10822790/can-i-call-a-function-of-a-shell-script-from-another-shell-script)
```bash
source ./file_to_source;
name_of_function;
```

Running multiple tests in one go pass your parameters in with read. [source](https://stackoverflow.com/questions/10822790/can-i-call-a-function-of-a-shell-script-from-another-shell-script)
```bash
# Run tests
# argument table format:
# testarg1   testarg2     expected_relationship
source ./file_we_are_running_function_from;
echo "The following tests should pass"
while read -r test
do
    testvercomp $test
done << EOF
1            1            =
2.1          2.2          <
3.0.4.10     3.0.4.2      >
4.08         4.08.01      <
3.2.1.9.8144 3.2          >
3.2          3.2.1.9.8144 <
1.2          2.1          <
2.1          1.2          >
5.6.7        5.6.7        =
1.01.1       1.1.1        =
1.1.1        1.01.1       =
1            1.0          =
1.0          1            =
1.0.2.0      1.0.2        =
1..0         1.0          =
1.0          1..0         =
EOF

echo "The following test should fail (test the tester)"
testvercomp 1 1 '>'
```

## Write to a specific line of another file.
```bash
# uses "?" as a wildcard inserts into the 9th line the variable pwd3 wiping overidding the line that was previously at that location.
sed -i '' '9s?.*?'"$pwd3"'?' $DIR/.tmp/com.monitor.charge.plist
```

## Append to the end of a file 
```bash 
echo "hello" >> test.txt
```

## Overide file with new contents
```bash
echo "hello is gone\ngoodbye" > test.txt"
```

## For each directory is a useful command.
```bash
for d in ./*/ ; do (cd "$d" && echo "$d" && echo "hi"); done
```

## One great attribute bash provides is that you can quickly test everything on terminal in a single line of code <br/>
In bash everything can be a one liner and copied and pasted directly into terminal.

## Use bash when it makes sense to use bash. High Level operating system programs or curl requests.<br/>
Bash can be used by other languages such as ruby or node-javascript to in a simple way add functionality to your application.

## Do not always use bash. Come up with an Idea first and let that determine the language you choose.


everything below from [source](http://tiswww.case.edu/php/chet/bash/bashref.html)
## Regarding alias usage:
Aliases are expanded when a command is read, not when it is executed. Therefore, an alias definition appearing on the same line as another command does not take effect until the next line of input is read. The commands following the alias definition on that line are not affected by the new alias. This behavior is also an issue when functions are executed. Aliases are expanded when a function definition is read, not when the function is executed, because a function definition is itself a command. As a consequence, aliases defined in a function are not available until after that function is executed. To be safe, always put alias definitions on a separate line, and do not use alias in compound commands.

For almost every purpose, shell functions are preferred over aliases.

## Regarding braces vs parameters
```
( list )
Placing a list of commands between parentheses causes a subshell environment to be created

{ list; }
Placing a list of commands between curly braces causes the list to be executed in the current shell context. 
```
