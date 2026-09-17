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

#### Numbers - int and float
```
cpu_count = 4        # int
load_average = 0.75  # float
```

#### Arithmetic
```
total = 8 + 4        # 12
free = 16 - 3        # 13
scaled = 5 * 3       # 15
```

Division with / always gives a float, even when the result is whole:
```
print(10 / 2)        # 5.0, a float
```

When you want a whole number, use floor division //, which divides and drops the remainder. The modulo operator % gives you that remainder:

```
print(17 // 5)       # 3
print(17 % 5)        # 2
```

#### Powers
Two asterisks raise a number to a power, which is handy for sizes and byte math:
```
print(2 ** 10)       # 1024
```

#### Mixing int and float
Combine an int and a float and the result is a float, because Python widens to the type that can hold a decimal:
```
print(4 + 0.5)       # 4.5
print(3 * 2.0)       # 6.0
```

#### Readability with underscores
Large numbers are hard to read, so Python lets you group digits with underscores. They are ignored by the interpreter and are purely for your eyes:

```
max_bytes = 1_000_000
```

### Booleans and None
A boolean is a truth value, one of exactly two options - True or False. Both are capitalized, and both are keywords, not strings:
```
is_healthy = True
in_maintenance = False
```

#### Comparisons produce booleans
You rarely type True and False by hand. More often they come out of a comparison:
```
print(4 > 2)         # True
print(10 == 10)      # True
print("a" == "b")    # False
```

The comparison operators are == for equal, != for not equal, and <, >, <=, >= for ordering. Note that == compares values, while a single = assigns a variable - mixing them up is a classic bug.

#### Combining conditions
Combine booleans with and, or, and not:
```
ready = is_healthy and not in_maintenance
```
and is True only when both sides are True. or is True when either side is. not flips a value.

#### None means no value
None is a special value that stands for "nothing here yet" or "no result". It is its own type, and it is not the same as 0, False, or an empty string:
```
last_error = None    # no error has happened
```
You will see None returned by functions that have nothing useful to give back, and used as a placeholder before a real value is known. To test for it, compare with is:
```
print(last_error is None)   # True
```

Use is None and is not None rather than == when checking for None. It is the idiomatic form and reads clearly.

#### Checking and converting types
Every value in Python has a type, and the type() function tells you what it is:
```
print(type(4))         # <class 'int'>
print(type(0.75))      # <class 'float'>
print(type("web-01"))  # <class 'str'>
print(type(True))      # <class 'bool'>
```

##### Why conversion matters
Values often arrive as the wrong type. A port number read from a config file or an environment variable comes in as the string "8080", not the number 8080. You cannot do math on a string:
```
port = "8080"
print(port + 1)        # TypeError, can't add int to str
```

You have to convert it first. This is one of the most common fixes in DevOps scripting, because so much input arrives as text.

#### The conversion functions
Four built-in functions convert between the core types:

int(x) turns a value into an integer.
float(x) turns a value into a float.
str(x) turns a value into a string.
bool(x) turns a value into a boolean.
```
port = int("8080")       # 8080 as an int
print(port + 1)          # 8081

ratio = float("0.75")    # 0.75 as a float
count_text = str(4)      # "4" as a string
```

#### How bool converts
bool() follows a simple rule - "empty" or "zero" values become False, everything else becomes True:
```
print(bool(0))       # False
print(bool(""))      # False
print(bool(None))    # False
print(bool(5))       # True
print(bool("ok"))    # True
```

### 4.Strings
A string is a piece of text. You write one by wrapping characters in quotes, and Python accepts single or double quotes with no difference in meaning:
```
name = 'web-01'
role = "database"
message = "it's healthy"
path = 'the "prod" cluster'
```

##### Escaping and special characters
A backslash starts an escape sequence. The two you will meet most are \n for a newline and \t for a tab:
```
print("line one\nline two")
```
That prints across two lines. To put a literal backslash in a string, double it as \\.

##### Multi-line strings
Triple quotes, either ''' or """, let a string span several lines exactly as written:
```
banner = """
deploy-bot
version 1.0
"""
```

##### Joining and repeating
The + operator joins strings end to end, and * repeats one:
```
full = "web" + "-" + "01"   # "web-01"
rule = "=" * 20              # twenty equals signs
```

##### Length
The built-in len() function returns how many characters a string holds:
```
print(len("web-01"))   # 6
```

##### Trimming whitespace
.strip() removes whitespace from both ends of a string, including stray spaces, tabs, and newlines. It is the first thing you do to any value read from a file or command, where trailing newlines are common:
```
raw = "  web-01\n"
print(raw.strip())    # "web-01"
```

##### Changing case
.upper() and .lower() return the string with every letter shifted to one case. Lowercasing is a standard way to compare values without worrying about how they were typed:
```
print("Web-01".upper())   # "WEB-01"
print("Web-01".lower())   # "web-01"
```

##### Splitting into pieces
.split() breaks a string into a list of pieces. With no argument it splits on runs of whitespace. Give it a separator to split on that instead:
```
"web-01 web-02 db-01".split()      # three items, split on spaces
"a,b,c".split(",")                  # split on commas
```

This is how you pull fields out of a log line or a comma-separated value. Lists get their own topic next, so for now just know .split() is what turns one string into separate fields.

##### Joining pieces together
.join() is the reverse of .split(). You call it on the separator string and pass the pieces you want glued together, and it returns one string with that separator between each piece:
```
names = ["web", "api", "monitor"]
print(", ".join(names))   # "web, api, monitor"
```
The separator is whatever string you call .join() on, so "-".join gives you web-api-monitor and "".join glues the pieces with nothing between them. This is how you turn a list of values back into a single line for printing or writing to a file.

##### Replacing text
.replace(old, new) returns a copy with every occurrence of one substring swapped for another:
```
print("web_01".replace("_", "-"))   # "web-01"
```

##### Testing content
The in operator checks whether one string appears inside another and returns a boolean. It is the quick way to scan a line for a keyword:
```
line = "ERROR disk full"
print("ERROR" in line)   # True
```

#### f-strings
An f-string builds a string with values dropped straight into it. Put an f before the opening quote, then wrap any expression in curly braces inside the text. Python replaces each brace with the value:
```
host = "web-01"
port = 8080
print(f"connecting to {host} on port {port}")
```

That prints connecting to web-01 on port 8080. This is the modern, readable way to assemble output, and you will use it constantly for log lines, messages, and report text.

##### Why f-strings beat joining with plus
Without f-strings you would glue pieces together with + and convert every non-string by hand:
```
print("connecting to " + host + " on port " + str(port))
```

That is noisy and easy to get wrong. An f-string converts values for you, so you can drop a number straight in without str().

##### Any expression fits in the braces
The braces are not limited to plain variable names. Any expression works, and it is evaluated on the spot:
```
cpus = 4
print(f"total threads {cpus * 2}")
print(f"host is {host.upper()}")
```

##### Formatting numbers
A colon inside the braces starts a format spec. Two show up often - rounding a float to a fixed number of decimals, and padding a number to a width:
```
ratio = 0.75123
print(f"usage {ratio:.2f}")     # usage 0.75
```
The .2f means "a float with two digits after the point". You will reach for this whenever raw floats look untidy in a report.

##### Printing a variable and its value
An = right before the closing brace prints both the expression and its value, which is a fast debugging trick:
```
print(f"{port=}")   # port=8080
```