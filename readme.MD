# Python

* python3 -- version prints the interpreter version.
* pip3 -- version prints the package installer version.
* which python3 -- shows which Python program runs.

#### What a venv is
A virtual environment is a folder holding its own copy of Python and its own site-packages directory. Anything you pip install while it's active goes into that folder and nowhere else. Delete the folder and the environment is gone, with no trace left on the system.

```
python3 -m venv .venv           #Create 
source .venv/bin/activate       #Activate
deactivate                      #leaves it
```

Run without activating <br>
`.venv/bin/pip install requests` <br>
`.venv/bin/python script.py`

#### REPL
Run python3 with no filename and you land in the REPL - an interactive prompt where you type one line of Python and see the result straight away. REPL stands for Read, Evaluate, Print, Loop. It reads what you type, runs it, prints the result, and waits for the next line.
```
python3
```
To leave, type exit() and press Enter, or press Ctrl and D together.

#### Printing values
print() writes its arguments to the screen, followed by a newline:

```
print("service is up")
print(42)
print("cpu count", 4)           #Pass several values
````

#### Variables

`hostname = "web-01"` <br>
`cpu_count = 4`

Read that as "hostname is now web-01". After this, using the name hostname anywhere gives you back its value. There is no separate declaration step and you do not state a type - Python figures out the type from the value you assign.

```
retries = 3
retries = retries + 1   # now 4
status = "pending"
status = "done"         # replaces the old string
````
The old value is simply forgotten. Nothing about the first assignment is permanent.

#### Naming rules and conventions
Names must start with a letter or underscore and can contain letters, digits, and underscores. They cannot contain spaces or start with a digit. Names are case sensitive, so Host and host are different variables.
````
max_retries = 5
db_host = "10.0.0.5"
is_healthy = True
````
Pick names that say what the value means. db_host tells a reader far more than x or h, and clear names are the cheapest documentation a script can have.

#### Assigning several at once
Python lets you assign multiple names on one line, which is handy for related values:

`host, port = "10.0.0.5", 8080` <br>
Now host holds the string and port holds the number.


