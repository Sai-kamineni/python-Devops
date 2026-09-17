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

## 4.Strings
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

#### Slicing and indexing
Indexing a single character
Indexes start at 0, so the first character is at position 0, the second at 1, and so on. Put the index in square brackets:
```
name = "web-01"
print(name[0])    # "w"
print(name[3])    # "-"
```

Negative indexes count from the end, so -1 is the last character:
```
print(name[-1])   # "1"
```

##### Slicing a range
A slice uses [start:stop] and returns the characters from start up to but not including stop:
```
print(name[0:3])   # "web"
print(name[4:6])   # "01"
```

The stop position is excluded, so name[0:3] gives you three characters, at positions 0, 1, and 2. This off-by-one feels odd at first but becomes second nature.

##### Leaving out start or stop
Omit start and the slice runs from the beginning. Omit stop and it runs to the end:
```
print(name[:3])    # "web", from the start
print(name[4:])    # "01", to the end
```

##### Slices never overflow
Unlike a single index, a slice that runs past the end does not error. It just stops at the end of the string, which makes slicing safe to use on input of unknown length:
```
print(name[:100])  # "web-01", no error
```

## 5.lists
A list is an ordered collection of values kept in a single variable. Write one with square brackets and commas between the items:
```
servers = ["web-01", "web-02", "db-01"]
```

Lists keep their order, so the item you put first stays first. They can hold any type, and you can even mix types in one list, though in practice you'll usually keep a list to one kind of thing:
```
ports = [80, 443, 8080]
mixed = ["web-01", 8080, True]
empty = []
```

##### Reaching an item by index
Each item has a position, called its index, starting at 0. Use square brackets with the index to read one item:
```
servers = ["web-01", "web-02", "db-01"]
print(servers[0])   # web-01
print(servers[2])   # db-01
```

Counting from zero trips up everyone at first. The third item is at index 2, not 3.
Negative indexes count from the end, which saves you from working out the length just to grab the last item:
```
print(servers[-1])  # db-01
print(servers[-2])  # web-02
```
##### Slicing out a range
A slice pulls several items at once with start:stop. The start is included and the stop is not:
```
nums = [10, 20, 30, 40, 50]
print(nums[1:3])    # [20, 30]
print(nums[:2])     # [10, 20]
print(nums[2:])     # [30, 40, 50]
```

##### Changing an item in place
Assign to an index to replace whatever was there:
```
servers = ["web-01", "web-02", "db-01"]
servers[1] = "web-99"
print(servers)   # ['web-01', 'web-99', 'db-01']
```

##### Adding items
append adds one item to the end. It's the workhorse you'll use to collect results as a program runs:
```
servers = ["web-01"]
servers.append("web-02")
print(servers)   # ['web-01', 'web-02']
```

insert puts an item at a specific index and shifts the rest right:
```
servers.insert(0, "lb-01")
print(servers)   # ['lb-01', 'web-01', 'web-02']
```

##### Removing items
remove deletes the first item that matches a value:
```
servers.remove("web-01")
print(servers)   # ['lb-01', 'web-02']
```

pop removes an item by index and hands it back to you. With no index it removes and returns the last item, which is handy for draining a list one item at a time:
```
last = servers.pop()
print(last)      # web-02
print(servers)   # ['lb-01']
```
These methods change the list they're called on and return either nothing (append, insert, remove) or the removed item (pop). There's no new list to catch, so don't write servers = servers.append(x).

##### len
len returns the number of items. It's a built-in function, so the list goes in the parentheses:
```
servers = ["web-01", "web-02", "db-01"]
print(len(servers))   # 3
```

##### Sorting
There are two ways to sort, and the difference matters. sort is a method that reorders the list in place and returns nothing:
```
nums = [30, 10, 20]
nums.sort()
print(nums)   # [10, 20, 30]
```

`sorted` is a built-in function that leaves the original alone and hands back a new sorted list:
```
nums = [30, 10, 20]
ordered = sorted(nums)
print(ordered)   # [10, 20, 30]
print(nums)      # [30, 10, 20]
```

Reach for sorted when you still need the original order, and sort when you don't. Both take reverse=True to go high to low.

##### Membership with in
The in operator checks whether a value is present and gives back True or False:
```
print("db-01" in servers)   # True
print("db-99" in servers)   # False
```

##### Finding a position with index
index returns the position of the first matching item:
```
print(servers.index("db-01"))   # 2
````

If the value isn't in the list, index raises an error, so check with in first when you aren't sure it's there.

## 6.tuple
A tuple is an ordered collection of values, just like a list, with one key difference: once you create it, you can't change it. Write one with parentheses and commas:
```
server = ("web-01", "10.0.0.5", 8080)
```

Like a list, a tuple keeps its order and can hold any mix of types. Here the tuple groups three related facts about one server into a single value.

##### Reading items
Access items by index with square brackets, exactly as you do with a list. Indexing starts at 0, and negative indexes count from the end:
```
server = ("web-01", "10.0.0.5", 8080)
print(server[0])    # web-01
print(server[-1])   # 8080
```
Slicing works too, and a slice of a tuple gives back a new tuple.

##### You can't change a tuple
Trying to assign to an item raises an error rather than changing anything:
```
server[2] = 9090    # TypeError: 'tuple' object does not support item assignment
```

There's no append, no remove, no pop. A tuple is fixed from the moment you create it.

##### The one-item gotcha
A single value in parentheses is not a tuple, it's just that value. The comma is what makes a tuple, so a one-item tuple needs a trailing comma:
```
not_a_tuple = ("web-01")     # just the string "web-01"
one_tuple = ("web-01",)      # a tuple with one item
```

#### Tuples vs lists
A list is mutable. You build it up, sort it, and remove from it as your program runs. A tuple is immutable. Once created, its contents are locked:
```
hosts = ["web-01", "web-02"]   # a list, can change
hosts.append("web-03")         # fine

record = ("web-01", "10.0.0.5", 8080)   # a tuple, fixed
record.append("x")             # AttributeError, no such method
```

##### When a tuple is the right choice
Use a tuple when the collection represents one thing made of a fixed set of parts, and changing any part would make it a different thing. A server's (hostname, ip, port) is a good example: swapping the IP doesn't edit the record, it describes a different server.

Immutability buys you a guarantee. If a value is a tuple, you know no other part of the program quietly changed it out from under you. That makes code easier to reason about.

##### When a list is the right choice
Use a list when the collection is a bag of similar items that grows, shrinks, or gets reordered: a list of hostnames you're collecting, a queue of tasks to work through, results you append as you go.

##### A rough rule of thumb
If you find yourself thinking "record" or "a fixed group of related fields", reach for a tuple. If you're thinking "a collection I'll add to or change", reach for a list. Both index the same way, so you lose nothing in readability by picking the honest one.

##### Tuple unpacking
Unpacking assigns the items of a tuple to several variables in one line. Put the names on the left, matching the number of items:
```
server = ("web-01", "10.0.0.5", 8080)
hostname, ip, port = server
print(hostname)   # web-01
print(ip)         # 10.0.0.5
print(port)       # 8080
```

This is much clearer than pulling each item out by index with server[0], server[1], and server[2]. The names tell the reader what each position means.

##### The counts must match
You need exactly as many names as there are items. Too few or too many raises an error:
```
a, b = ("web-01", "10.0.0.5", 8080) # ValueError: too many values to unpack
```

##### Unpacking works on lists too
Unpacking isn't tuple-only. It works on any ordered collection, including a list, as long as the counts line up:
```
first, second = ["web-01", "web-02"]
```

##### Swapping values
A neat trick falls out of unpacking: you can swap two variables in one line, with no temporary variable, because the right side is packed into a tuple first and then unpacked:
```
a, b = 1, 2
a, b = b, a
print(a, b)   # 2 1
```

You'll see unpacking constantly once loops and dictionaries arrive, since many built-ins hand back tuples that read best when unpacked into named variables.

## 7.Dictionaries
A dictionary maps keys to values. Where a list finds items by numeric position, a dictionary finds them by a name you choose. Write one with curly braces and key: value pairs:

'config = {"app": "billing", "port": 8080, "debug": False}'

Each pair joins a key on the left to a value on the right. Here the key "app" maps to the value "billing", and "port" maps to 8080.

##### Why dictionaries fit DevOps work
Configuration, JSON payloads, API responses, and environment settings are all key-to-value data. A dictionary is the shape that data takes in Python, so you'll meet dictionaries constantly. This structure reads much better than trying to remember that the port lives at index 1 of some list.

##### Keys and values
Values can be anything: strings, numbers, booleans, even lists or other dictionaries. Keys are almost always strings in the config and JSON data you'll handle:
```
server = {
    "hostname": "web-01",
    "ports": [80, 443],
    "healthy": True,
}
```

Splitting a dictionary across lines like this, one pair per line, is the usual style once it grows past two or three keys. The trailing comma after the last pair is allowed and makes edits cleaner.

##### Keys are unique
Each key appears once. If you write the same key twice, the last value wins:
```
d = {"port": 80, "port": 8080}
print(d)   # {'port': 8080}
```
Splitting a dictionary across lines like this, one pair per line, is the usual style once it grows past two or three keys. The trailing comma after the last pair is allowed and makes edits cleaner.

##### Keys are unique
Each key appears once. If you write the same key twice, the last value wins:
```
d = {"port": 80, "port": 8080}
print(d)   # {'port': 8080}
```

##### Reading a value by key
Put the key in square brackets:
```
config = {"app": "billing", "port": 8080}
print(config["app"])    # billing
print(config["port"])   # 8080
```

If the key isn't there, this raises a KeyError and stops your program:
```
print(config["region"])
# KeyError: 'region'
```
