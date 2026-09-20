# Python

#### 1.Why Python for DevOps
Bash is perfect for gluing commands together. The moment a script needs to parse JSON, call an API, retry on failure, or keep its logic readable past fifty lines, bash starts fighting you. That's where Python takes over.

Python runs on every server you'll touch, reads like plain English, and ships with libraries for the exact problems DevOps work throws at you.

##### What you'll actually use it for
A few jobs come up again and again:

Reading and writing config files in JSON, YAML, and CSV.
Calling REST APIs to check a service or trigger a deployment.
Parsing logs to pull out errors or count events.
Running shell commands from inside a program and reacting to what they return.
Driving cloud providers through their SDKs (each cloud's own code library), like AWS with boto3.

* python3 -- version prints the interpreter version.     ##Python 3.12.3
* pip3 -- version prints the package installer version.   #pip 24.0 from /usr/lib/python3/dist-packages/pip (python 3.12)
* which python3 -- shows which Python program runs.         #/usr/bin/python3

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

## 3.Variables
A variable is a name that points at a value. You create one with the equals sign, putting the name on the left and the value on the right:

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
"web-01 web-02 db-01".split()       # three items, split on spaces  #['web-01', 'web-02', 'db-01']
"a,b,c".split(",")                  # split on commas               #['a','b','c']
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

`config = {"app": "billing", "port": 8080, "debug": False}`

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

##### Reading safely with get
get looks up a key but returns None instead of raising when the key is missing. Pass a second argument to choose your own fallback:
```
print(config.get("region"))            # None
print(config.get("region", "unset"))   # unset
print(config.get("port", 80))          # 8080
```

Reach for get whenever a key might be absent, which is common with config and API data where fields are optional. It turns a crash into a sensible default.

##### Updating and adding keys
Assigning to a key sets its value. If the key exists, you overwrite it; if it doesn't, you add it. Same syntax either way:
```
config["port"] = 9090            # updates the existing key
config["region"] = "us-east-1"   # adds a new key
print(config)                   # {'app': 'billing', 'port': 9090, 'region': 'us-east-1'}
````

This is the everyday way you build a dictionary up: start with a few keys, or none, and assign more as you go.

##### Removing a key
pop removes a key and returns its value:
```
removed = config.pop("region")
print(removed)   # us-east-1
```

##### keys, values, and items
keys gives you all the keys, values gives you all the values, and items gives you each key-value pair together:
```
config = {"app": "billing", "port": 8080}
print(list(config.keys()))     # ['app', 'port']
print(list(config.values()))   # ['billing', 8080]
print(list(config.items()))    # [('app', 'billing'), ('port', 8080)]
```

Notice that items hands back each pair as a tuple. Wrapping the result in list() here just makes it print cleanly; you'll most often feed these straight into a loop once loops arrive.

##### Checking for a key with in
The in operator checks whether a key is present, returning True or False. It looks at keys, not values:
```
print("app" in config)        # True
print("region" in config)     # False
print("billing" in config)    # False, that's a value not a key
```

This pairs naturally with get and with plain lookups. Check in before reaching for config["key"] when you aren't sure the key is there and you want to avoid a KeyError.

##### How many keys with len
len counts the pairs in the dictionary, same as it counts items in a list:
`print(len(config))   # 2`

## 8.Sets
A set is an unordered collection of unique values. Two things make it different from a list: it never holds duplicates, and it doesn't keep any order. Write one with curly braces:
```
ports = {80, 443, 8080}
```

##### Duplicates disappear
If you put the same value in twice, the set keeps just one copy. This is the defining behavior:
```
ports = {80, 443, 80, 8080, 443}
print(ports)   # {80, 443, 8080}
```

##### No order, no indexing
A set doesn't track position, so there's no first or last item and you can't index it. This fails:
```
ports[0]
# TypeError: 'set' object is not subscriptable
```
When you print a set the items may come out in any order. If you need order or indexing, you want a list, not a set.

##### Making a set from a list
Passing a list to set() builds a set from its values, dropping any duplicates along the way. This is the most common way you'll create a set in practice:
```
hosts = ["web-01", "web-02", "web-01"]
unique = set(hosts)
print(unique)   # {'web-01', 'web-02'}
```

##### Membership and size
The in operator and len work just like they do on a list, and in on a set is very fast even when the set is large:
```
print("web-01" in unique)   # True
print(len(unique))          # 2
```

##### The empty set gotcha
Empty braces {} make an empty dictionary, not an empty set. For an empty set you must call set():

`empty = set()`

#### Union, intersection, and difference
The reason to reach for a set is often to compare two collections. Sets answer three questions directly: what's in either, what's in both, and what's in one but not the other.

Take two sets to work with:
```
allowed = {"nginx", "curl", "git", "vim"}
installed = {"nginx", "git", "htop"}
```

##### Union: in either set
Union combines both sets into one, keeping every value once. Use the | operator or the union method:
```
print(allowed | installed)
# {'nginx', 'curl', 'git', 'vim', 'htop'}
```

##### Intersection: in both sets
Intersection keeps only the values present in both. Use & or the intersection method:
```
print(allowed & installed)
# {'nginx', 'git'}
```
This answers "which allowed packages are actually installed".

##### Difference: in one but not the other
Difference keeps the values in the left set that aren't in the right one. Use - or the difference method. Order matters here:
```
print(allowed - installed)
# {'curl', 'vim'}     allowed but not installed

print(installed - allowed)
# {'htop'}            installed but not allowed
```

These two differences answer different questions. allowed - installed is what's missing, and installed - allowed is what shouldn't be there.

#### Deduplicating with sets
Because a set can't hold duplicates, converting a collection to a set is the fastest way to strip repeats out of data. This is one of the most common jobs a set does in real scripts.

The pattern
Wrap a list in set() and the duplicates vanish:
```
ips = ["10.0.0.1", "10.0.0.2", "10.0.0.1", "10.0.0.3", "10.0.0.2"]
unique = set(ips)
print(unique)   # {'10.0.0.1', '10.0.0.2', '10.0.0.3'}
```

##### Counting unique values
Very often you don't even want the values, just how many distinct ones there are. Combine set() with len():
```
ips = ["10.0.0.1", "10.0.0.2", "10.0.0.1", "10.0.0.3", "10.0.0.2"]
print(len(set(ips)))   # 3
```
That single line answers questions like "how many unique client IPs hit this endpoint" or "how many distinct users appear in this log".

##### Getting a clean list back
If you need an ordered list again after removing duplicates, wrap the set in sorted() to get a sorted list, or list() to get one in no particular order:
```
clean = sorted(set(ips))
print(clean)   # ['10.0.0.1', '10.0.0.2', '10.0.0.3']
```

## 9.if, elif, and else
An if statement runs a block of code only when a condition is true. Add elif to test more conditions, and else to catch everything left over. This is how a script makes a decision instead of doing the same thing every time.

The basic if
```
cpu = 92
if cpu > 90:
    print("CPU is critical")
```

The expression after if evaluates to true or false. When it's true, the indented block runs. When it's false, Python skips the block and carries on.

##### Indentation defines the block
Python has no curly braces. The indented lines under an if are its block, and four spaces is the standard indent. Every line at that indent belongs to the block. The first line that dedents ends it.
```
if cpu > 90:
    print("critical")
    print("paging on-call")
print("done")   # not indented, so it always runs
```

#### elif and else
Chain more tests with elif, and give a fallback with else:
```
if cpu > 90:
    print("critical")
elif cpu > 70:
    print("warning")
else:
    print("ok")
```

Python checks the conditions top to bottom, runs the first block whose condition is true, and skips the rest. else runs only when none of the conditions matched.

#### Comparison operators
Six operators compare two values and hand back a boolean, True or False. They are what you put inside an if condition.

The six operators
```
a == b   # equal
a != b   # not equal
a <  b   # less than
a >  b   # greater than
a <= b   # less than or equal
a >= b   # greater than or equal
```

Each one evaluates to a boolean on its own:
```
print(5 > 3)      # True
print(5 == 3)     # False
print(5 != 3)     # True
```

##### One equals sign assigns, two compares
This trips up everyone at least once. A single = assigns a value. A double == asks whether two values are equal.
```
status = "ok"      # assignment
if status == "ok": # comparison
    print("healthy")
```
Writing if status = "ok" is a syntax error, which is Python saving you from a classic bug.

##### Comparisons work on strings too
== and != compare strings for an exact match, which is how you check a status code or an environment name:
```
env = "prod"
if env == "prod":
    print("be careful")
```

##### Comparisons produce values you can store
A comparison is just an expression, so you can save its result:
```
over_budget = cost > 100
print(over_budget)   # True or False
```

##### and: both must be true
and is true only when the expressions on both sides are true.
```
cpu = 85
memory = 92
if cpu > 80 and memory > 90:
    print("both under pressure")
```

##### or: at least one must be true
or is true when either side is true.
```
if cpu > 90 or memory > 90:
    print("something is critical") 
```

##### not: flip it
not turns true into false and false into true. It reads well in front of a single test:
```
healthy = False
if not healthy:
    print("investigate")
```

##### Use parentheses when you mix them
and binds tighter than or, which can surprise you. When a condition mixes both, add parentheses to make the grouping obvious to the reader and to Python:
```
if (env == "prod" and cpu > 80) or disk > 95:
    print("alert")
```

##### Short-circuiting
Python stops evaluating as soon as the answer is decided. With and, if the left side is false, the right side is never checked. With or, if the left side is true, the right side is skipped. This lets you guard a risky test with a cheaper one on its left, and it's the reason the order of your conditions can matter.

##### The falsy values
A short, fixed list of values count as false. Everything else counts as true.
```
False
None
0 and 0.0
"", the empty string
[], (), {}, and an empty set, the empty collections
```

So an empty list is falsy, but a list with anything in it is truthy. A non-empty string is truthy. The number 0 is falsy, 1 is truthy.

##### Test a value directly
Because collections are falsy when empty, you can check them without comparing lengths:
```
servers = []
if not servers:
    print("no servers to check")
```

That reads better than if len(servers) == 0, and it does the same job.


##### None means "no value yet"
None is Python's stand-in for missing or not-set. It's falsy, which makes it easy to check for:
```
alert_email = ""
if not alert_email:
    print("alert email is not configured")
```

## 10.for loops and range
A for loop runs a block once for each item in a sequence. It's the workhorse of automation, where you almost always have a list of servers, files, or records to process the same way.

Looping over a list
```
servers = ["web-01", "web-02", "db-01"]
for server in servers:
    print("checking", server)
```

On each pass, the variable after for (here server) holds the next item. The indented block runs once per item. The loop ends when the list runs out.

##### The loop variable is yours to name
server is just a name you pick. Choose something that describes one item, so the body reads clearly. for s in servers works but for server in servers is easier to follow.

range: loop a fixed number of times
When you want to repeat something a set number of times, range produces a sequence of integers to loop over:
```
for i in range(3):
    print("attempt", i)
```
That prints 0, 1, 2. range(3) counts from 0 up to but not including 3, so it yields three numbers.

##### range with a start and step
range takes up to three arguments, start, stop, and step:
```
range(1, 4)      # 1, 2, 3
range(0, 10, 2)  # 0, 2, 4, 6, 8
```

The stop value is always excluded. This off-by-one is deliberate and becomes natural quickly. Reach for range when you need counting; reach for a plain for item in collection when you already have the items.

### while loops
A `while` loop repeats its block as long as a condition stays true. Use it when you don't know up front how many times to loop, only the condition that should stop it.

The shape
```
attempts = 0
while attempts < 3:
    print("trying", attempts)
    attempts = attempts + 1
```

Python checks the condition before each pass. While it's true, the block runs. Once it's false, the loop stops and the script moves on.

##### Something in the loop must change the condition
The block has to move the condition toward false, or the loop never ends. Here attempts grows by one each pass until it reaches 3. Forget that line and you have an infinite loop that hangs the script.

#### for or while
When you want to repeat something a set number of times, for produces a sequence of integers to loop over:while when there's no fixed list and you're waiting on a condition, like polling until a service reports ready or retrying until a counter runs out.
```
retries = 5
while retries > 0:
    print("polling, retries left:", retries)
    retries = retries - 1
```

### Looping over lists and dictionaries
A for loop works on any collection, not just lists. Dictionaries and their keys and values are just as loopable, and a couple of built-ins make the common patterns clean.

##### Looping over a list with its index
Sometimes you want the position as well as the item. enumerate hands you both:
```
servers = ["web-01", "web-02", "db-01"]
for index, server in enumerate(servers):
    print(index, server)
```

That prints 0 web-01, 1 web-02, 2 db-01. enumerate counts from 0 by default.

##### Looping over a dictionary
Looping over a dict directly gives you its keys:
```
ports = {"web": 8080, "db": 5432}
for name in ports:
    print(name, ports[name])
```

##### items(), keys(), and values()
To get keys and values together in one step, loop over .items():
```
for name, port in ports.items():
    print(name, "listens on", port)
```

.items() yields each key/value pair, which you unpack into two variables. .keys() gives just the keys and .values() gives just the values, if you only need one side.

##### Building a result while you loop
A very common pattern is starting with an empty accumulator and updating it each pass:
```
total = 0
for port in ports.values():
    total = total + port
print(total)
```

This is how you count events, sum sizes, or collect matches out of a collection, one item at a time.

### break and continue
Two keywords steer a loop from the inside. break stops the loop early, and continue skips to the next item. Both work in for and while loops.

##### break: stop looping now
`break` exits the loop immediately, skipping any remaining items. It's how you stop as soon as you've found what you were looking for:
```
servers = ["web-01", "web-02", "db-01"]
for server in servers:
    if server == "db-01":
        print("found the database")
        break
```
Once break runs, the loop is done and the script continues after it. There's no point scanning the rest of the list once you have your answer.

##### continue: skip to the next item
continue abandons the current pass and jumps straight to the next item, leaving the rest of the block unrun for that item:
```
counts = [12, 0, 45, 0, 7]
for count in counts:
    if count == 0:
        continue
    print("processing", count)
```

Here the zero entries are skipped and only the non-zero ones print. continue keeps the loop going; it just short-cuts this one pass.

##### When to use which
Use break when the whole loop is finished, like finding a match or hitting a retry limit. Use continue to filter out items you want to ignore without nesting the rest of the body inside an if. Both keep loop bodies flatter and easier to read than piling on conditions.

## 11.List comprehensions
A list comprehension builds a new list from an existing collection in one line. It replaces the very common pattern of creating an empty list and appending to it in a loop.

##### The long way
Here's the pattern a comprehension shortens:
```
names = ["web-01", "db-01"]
upper = []
for name in names:
    upper.append(name.upper())
```
Three lines of setup and loop just to transform each item.

##### The comprehension
The same result in one line:
```
upper = [name.upper() for name in names]
```

Read it left to right: for each name in names, produce name.upper(), and collect the results into a new list. The expression on the left is what each item becomes; the for part on the right is where the items come from.

##### The shape
Every list comprehension follows the same skeleton:
`[expression for item in collection]`

The expression runs once per item and its result goes into the new list. The original collection is left untouched.

##### Why use them
Comprehensions are shorter and, once the shape is familiar, easier to read than the loop-and-append version. In DevOps scripts you'll use them constantly to turn one list into another: hostnames into URLs, raw strings into integers, paths into filenames. When the transform is simple, a comprehension says it in one clear line.

#### Filtering with an if clause
Add an if to the end of a comprehension to keep only the items you want. This is how you filter a list down as you build the new one.
```
[expression for item in collection if condition]
```

The if runs for each item. When it's true, the item's expression goes into the new list. When it's false, the item is skipped.

##### An example
Keep only the servers whose name starts with web-:
```
servers = ["web-01", "db-01", "web-02", "cache-01"]
web = [s for s in servers if s.startswith("web-")]
print(web)   # ['web-01', 'web-02']
```

Only the two web- names survive. The rest are filtered out.

##### Filter and transform at once
The expression and the filter are independent, so you can reshape the items you keep in the same line:
```
ports = [8080, 22, 443, 9090, 80]
high = [p for p in ports if p >= 1024]
print(high)   # [8080, 9090]
```

Here the filter drops the low port numbers and the surviving values land in the new list. You could just as easily transform them, for example str(p) to turn each kept port into a string.

##### When the filter gets complex
The if clause can use and, or, and not like any condition. But if the test grows long, or you need more than a simple keep-or-skip, a regular for loop with an if inside is clearer. Use the filtering comprehension when the condition stays short and the intent is obvious.

#### Dictionary comprehensions
A dictionary comprehension builds a dict in one line, the same idea as a list comprehension but producing key/value pairs. It's the clean way to turn a list into a lookup table.
```
{key_expression: value_expression for item in collection}
```
The curly braces and the colon are what make it a dict instead of a list. For each item you produce one key and one value.

##### Building a lookup from a list
Say you have server names and want a dict mapping each name to the length of its name:
```
names = ["web", "db", "cache"]
sizes = {name: len(name) for name in names}
print(sizes)   # {'web': 3, 'db': 2, 'cache': 5}
```

##### Parsing pairs into a dict
A common real task is turning name:value strings into a dict. Split each string, then use the two parts as key and value:
```
raw = ["web:8080", "db:5432"]
ports = {pair.split(":")[0]: int(pair.split(":")[1]) for pair in raw}
print(ports)   # {'web': 8080, 'db': 5432}
```

split(":") breaks each string in two; [0] is the name and [1] is the port, and int() turns the port text into a number.

##### Filtering works here too
Just like a list comprehension, you can add an if to keep only some pairs:
```
busy = {name: hits for name, hits in counts.items() if hits > 100}
```
Looping over .items() gives you the key and value to test and reuse. When a transform stays simple, a dict comprehension is the most direct way to reshape one collection into a lookup.

## 12.Defining functions with def
A function is a named block of code you can run whenever you need it. Instead of copying the same lines into three places, you write them once, give them a name, and call that name. The def keyword defines one.
```
def greet():
    print("deploy starting")
```

##### Calling a function
You call a function by writing its name followed by parentheses:
```
greet()
greet()
```
That runs the body twice and prints the line twice. The parentheses are what trigger the call. The name on its own, without them, just refers to the function without running it.

#### Arguments and return values
A function that always does the exact same thing is limited. Arguments let you pass data in, and a return value lets you get a result back out. Together they turn a function into a reusable tool.

Passing data in with arguments
List the inputs a function needs between its parentheses. These names are called parameters:
```
def greet(name):
    print("deploying", name)

greet("web")
greet("db")
```

When you call greet("web"), the string "web" becomes name inside the function. Call it again with a different value and the function does its work on that value instead. A function can take several parameters, separated by commas:
```
def report(service, status):
    print(service, "is", status)

report("web", "healthy")
```

##### Getting a result back with return
print shows a value on screen. return hands a value back to whatever called the function, so you can store it or use it further:
```
def percent(used, total):
    return used / total * 100

usage = percent(18, 40)
print(usage)
```

The call percent(18, 40) evaluates to 45.0, which gets stored in usage. That's the key difference: a function that prints only shows you something, while a function that returns gives you a value to keep working with.

##### return ends the function
As soon as Python hits return, the function stops and hands back the value. Any lines after it don't run. A function with no return returns the special value None.

#### Default and keyword arguments
Two features make function calls clearer and more flexible: default values that fill in when you leave an argument out, and keyword arguments that name the value you're passing.

##### Default values
Give a parameter a default with = in the definition. If the caller skips it, the default is used:
```
def connect(host, port=22):
    print("connecting to", host, "on", port)

connect("web01")
connect("web01", 2222)
```

The first call uses port 22, the default. The second overrides it with 2222. Defaults are perfect for the common case, letting callers set a value only when they need something other than the usual.

Parameters with defaults must come after parameters without them. def connect(port=22, host) is an error.

##### Keyword arguments
Normally arguments match parameters by position, left to right. You can instead pass them by name, which is called a keyword argument:

`connect(host="web01", port=2222)`

The order no longer matters when you name them, and the call documents itself. Compare these two calls:
```
create_user("deploy", True, False)
create_user("deploy", admin=True, locked=False)
```

The second is far easier to read six months later. Nobody has to guess what True, False means.

##### Mixing positional and keyword
You can combine both, but positional arguments must come first:
```
connect("web01", port=2222)
```

Here "web01" matches host by position, and port is named. A common habit is to pass the obvious required values positionally and name the optional ones.

#### *args, **kwargs, and scope
Sometimes you don't know ahead of time how many arguments a function will get. Two special parameters handle that, and a separate rule called scope decides which variables a function can see.

##### *args collects extra positional arguments
A parameter written with a * in front gathers any leftover positional arguments into a tuple:
```
def deploy(*services):
    for name in services:
        print("deploying", name)

deploy("web", "db", "cache")
```

Inside the function, services is the tuple ("web", "db", "cache"). The caller can pass one name, five, or none, and the loop handles whatever arrives. The name args is just convention; the * is what does the work.

##### **kwargs collects extra keyword arguments
Two stars gather leftover keyword arguments into a dictionary:
```
def configure(**settings):
    for key, value in settings.items():
        print(key, "=", value)

configure(retries=3, timeout=30)
```

Here settings is the dict {"retries": 3, "timeout": 30}. This is handy when a function passes options through to something else without needing to know every option in advance.

##### Scope: where a variable lives
A variable created inside a function is local. It exists only while the function runs and vanishes when it returns:
```
def worker():
    count = 5

worker()
print(count)   # error: count is not defined out here
```

A variable defined at the top level of your script is global, and functions can read it. But assigning to a name inside a function creates a new local one, separate from any global with the same name. Keeping data flowing through arguments and return values, rather than reaching for globals, keeps functions predictable and easy to test.

## 13.import, from, and as
A module is a file of Python code that someone else wrote, full of functions and values you can use. The import statement pulls a module into your script so you can reach what's inside it. Python ships with hundreds of modules, and this is how you get at them.

##### import the whole module
The plain form loads a module and gives you access through its name:
```
import os
print(os.getcwd())
```

After import os, everything the os module provides is available under the os. prefix. os.getcwd() calls the getcwd function that lives inside os. The prefix makes it obvious where the function came from, which helps when a script imports several modules.

##### from ... import a specific name
When you only want one or two things from a module, import them directly so you can use them without the prefix:
```
from os import getcwd
print(getcwd())
```
Now getcwd is available on its own. This reads well when you use a name often, but too many bare imports make it hard to tell which module each one came from.

##### as gives it a shorter name
as renames what you import, usually to shorten a long module name or avoid a clash:
```
import subprocess as sp
sp.run(["ls"])
```

You'll see this constantly in real code, where libraries have a conventional short alias.

#### The standard library
Python comes with a large collection of modules called the standard library. They install with Python itself, so you can import them on any machine without running pip. For DevOps work, a handful come up constantly. This node is a quick tour so you recognize them; later topics dig into each one.

os: talk to the operating system
The os module works with the environment, paths, and the filesystem:
```
import os
print(os.getcwd())
print(os.environ.get("HOME"))
```

os.getcwd() returns the current directory, and os.environ reads environment variables. This is your bridge from Python to the machine it runs on.

##### sys: the interpreter and its arguments
The sys module is about Python itself and how it was launched:
```
import sys
print(sys.argv)
print(sys.version)
```

sys.argv holds the command-line arguments passed to your script, which is how a tool reads its own flags.

##### json: structured data
The json module converts between JSON text and Python dicts and lists, the format APIs and config files use everywhere:
```
import json
data = json.loads('{"status": "ok"}')
print(data["status"])
```

##### datetime: dates and times
The datetime module handles timestamps, which you'll want for log lines and scheduling:
```
import datetime
print(datetime.datetime.now())
```

#### Writing your own module
A module is just a .py file. Any Python file you write can be imported by another file, which lets you keep shared functions in one place and use them from several scripts. There's nothing special to declare; the filename becomes the module name.

##### Splitting code across files
Say you have a file helpers.py with a useful function:
```
# helpers.py
def usage_percent(used, total):
    return round(used / total * 100)
```

A second file in the same folder can import and use it. You import by the filename without the .py:
```
# main.py
from helpers import usage_percent
print(usage_percent(18, 40))
```

Run main.py and it pulls usage_percent out of helpers.py. The two files now share one copy of that function. Fix a bug in helpers.py and every script that imports it gets the fix.

##### How Python finds the file
When you write import helpers, Python looks for helpers.py starting in the same folder as the script you ran. That's why keeping a module next to the script that imports it just works. Modules from the standard library and installed packages are found through other search locations, but your own module living beside your script is the simple case.

##### Importing runs the file
The first time a module is imported, Python runs it top to bottom to define its functions and values. If helpers.py has a stray print at the top level, that line runs on import. Keep a module to definitions, functions and constants, so importing it has no surprise side effects.

## 14.pip and PyPI
The standard library is broad, but it doesn't cover everything. When you need something like requests for HTTP or boto3 for AWS, you install it with pip. pip is Python's package installer, and it pulls from PyPI, the public index where the Python community publishes libraries.

##### Installing a package
The basic command names the package you want:
```
pip install requests
```

pip contacts PyPI, downloads requests and anything it depends on, and puts them where your Python can import them. After that, import requests works in your code.

##### Seeing what's installed
Two commands tell you what's already there:
```
pip list
pip show requests
```

pip list prints every installed package and its version. pip show gives details about one package, including its version and what it depends on.

##### Dependencies come along
Most packages rely on other packages. requests alone pulls in a handful of supporting libraries for URLs and certificates. pip resolves that whole tree for you and installs everything needed, which is the main reason you never copy library files by hand.

##### A quick note on where it installs
Where a package lands depends on whether a virtual environment is active. Inside an active venv, pip install drops the package into that venv. Outside one, it targets the system or user Python. The next nodes tie pip together with venvs and a requirements file so installs are both isolated and repeatable.

#### requirements.txt
Installing packages one command at a time works until someone else has to reproduce your setup. A requirements.txt file lists every package a project needs, one per line, so anyone can install the whole set with a single command. It's the standard way to record a project's dependencies.

##### The format
Each line names a package, optionally with a version:
```
requests==2.31.0
pyyaml==6.0.1
```

The == pins the exact version. requests==2.31.0 means that version and no other. Pinning matters because a project that worked with one version can break when a newer release changes behavior. Pinned versions mean the install you get today is the install you get in six months.

##### Installing from the file
The -r flag tells pip to read a requirements file and install everything listed:
```
pip install -r requirements.txt
```

One command, and the whole dependency set is in place. This is what runs in a Dockerfile, a CI job, or a teammate's first setup.

##### Generating it with pip freeze
You don't have to write the file by hand. pip freeze prints every installed package with its exact version, already in requirements format:
```
pip freeze
```

Redirect it into the file to capture your current environment:
```
pip freeze > requirements.txt
```
The catch is that freeze lists everything installed, including dependencies of dependencies. Run it inside a clean virtual environment that holds only your project's packages, and the output stays meaningful instead of dumping the whole system.

#### venv and reproducible installs
A virtual environment and a requirements file solve two halves of the same problem. The venv keeps a project's packages isolated from the system and other projects. The requirements file records exactly what goes in it. Put together, they let anyone recreate your setup from scratch. You met venv back in the first topic; here it joins up with pinned requirements.

##### The full workflow
Starting a project cleanly is four steps:
```
python3 -m venv .venv
source .venv/bin/activate
pip install requests
pip freeze > requirements.txt
```

Create the isolated environment, activate it so pip targets it, install what you need, then freeze the result into requirements.txt. Because the venv held only your packages, the frozen file lists only what the project actually uses.

##### Recreating it elsewhere
On another machine, or after deleting the venv, the same project comes back with:
```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Same venv, same pinned versions, same result. This is the pattern behind reproducible builds: the environment is disposable, and the requirements file is the source of truth you commit to version control.

##### What to commit and what to ignore
Commit requirements.txt. Never commit the .venv folder. The venv is large, machine-specific, and fully rebuildable from the requirements file, so it belongs in .gitignore. Anyone who clones the project rebuilds their own venv from the pinned list.

## 15.Exceptions and tracebacks
When Python hits something it can't do, dividing by zero, opening a file that isn't there, it raises an exception. An exception is Python's way of saying "I can't continue with this", and unless you handle it, the whole script stops on the spot.

##### What a crash looks like
Try to open a file that doesn't exist:
```
open("/etc/missing.conf")
```

Python prints a traceback and exits:
```
Traceback (most recent call last):
  File "script.py", line 1, in <module>
    open("/etc/missing.conf")
FileNotFoundError: [Errno 2] No such file or directory: '/etc/missing.conf'
```

##### Reading a traceback
A traceback is not noise, it's a map. Read it bottom to top:

The last line is the exception type and message. Here it's FileNotFoundError and the file it couldn't find. This tells you what went wrong.
The lines above show where it happened, tracing back through the calls that led there, with file names and line numbers.
The bottom line is where to look first. FileNotFoundError on a path tells you the path is wrong or the file is missing, before you read another word.

##### Exception types
Every failure has a type, and the type tells you the category of problem:
```
FileNotFoundError: the file or directory isn't there.
KeyError: a dictionary key doesn't exist.
ValueError: a value has the right type but a wrong content, like int("abc").
ZeroDivisionError: you divided by zero.
```
Knowing the type matters, because handling errors well means catching the specific type you expect, which the next node covers.


#### try and except
A try/except block lets your script attempt something risky and react when it fails, instead of crashing. You put the code that might raise an exception in the try block, and the recovery code in the except block.
```
try:
    config = open("/etc/app.conf")
except FileNotFoundError:
    print("config missing, using defaults")
```

If open succeeds, the except block is skipped entirely. If it raises FileNotFoundError, Python jumps straight to the except block and runs it. Either way the script keeps going instead of stopping on a traceback.

##### Catch the specific type
Name the exception type you expect, as above with FileNotFoundError. This matters. A bare except that catches everything will also swallow bugs you didn't intend to hide, like a typo in a variable name, and leave you debugging in the dark:
```
try:
    value = data["port"]
except KeyError:
    value = 22
```

Here you're saying "if the key is missing, fall back to 22", and only a missing key triggers the fallback. Any other error still surfaces, as it should.

##### Handling more than one type
A block can have several except clauses for different problems:
```
try:
    port = int(raw_value)
except ValueError:
    print("port is not a number")
except KeyError:
    print("port was not set")
```
Python runs the first clause whose type matches the exception raised.

##### Why this matters for DevOps
Automation runs unattended. A missing file, an unreachable host, or a malformed config shouldn't take down the whole run. Catching the errors you expect lets a script log the problem, fall back to a default, or skip one item and carry on with the rest.

#### else and finally
A try block can take two more clauses beyond except. The else clause runs only when no exception was raised, and finally runs no matter what happens. Both make cleanup and control flow clearer.

##### else: run only on success
Code in else runs when the try block finished without raising:
```
try:
    port = int(raw_value)
except ValueError:
    print("not a number")
else:
    print("port is", port)
```
Why not just put that line at the end of try? Because else makes the intent explicit. The try holds only the risky call, and else holds what to do once it worked. It also avoids accidentally catching an exception from the follow-up code, since else sits outside the protected block.

##### finally: always run
Code in finally runs whether the try succeeded, raised, or was even caught. It's for cleanup that must happen either way, like releasing a resource:
```
try:
    conn = open_connection()
    use(conn)
except ConnectionError:
    print("connection failed")
finally:
    close_connection()
```

The connection gets closed no matter how the block ends. Without finally, an exception would skip your cleanup and leak the resource.

##### Putting it together
A full block reads in order: try the risky thing, handle failures in except, do the success-only work in else, and always clean up in finally. You rarely need all four at once, but knowing each one lets you write exactly the flow you mean.

#### Raising exceptions
Catching errors is one half of the story. The other half is raising them yourself. When your code detects a condition it can't sensibly continue past, raise stops it with a clear error instead of limping on with bad data.

##### The raise statement
raise triggers an exception on purpose. You pick the type and give it a message:
```
def set_port(port):
    if port < 1 or port > 65535:
        raise ValueError("port must be between 1 and 65535")
    return port
```

Call set_port(70000) and it raises ValueError with your message. The caller can catch it, or let it surface as a traceback. Either way, the function refused to hand back a nonsense value.

##### Why raise instead of returning a bad value
A function that silently returns None or -1 on bad input pushes the problem downstream, where it shows up later as a confusing failure far from the real cause. Raising fails loudly at the exact point the problem was detected, with a message that says what's wrong. That's easier to debug and harder to ignore.

##### Write messages that help
The message is what a tired engineer reads at 2 a.m. Make it specific. raise ValueError("port must be between 1 and 65535") beats raise ValueError("bad input"), because it names the rule that was broken. Include the offending value when it helps.

##### Choosing a type
Use a built-in type that fits: ValueError for a bad value, KeyError for a missing key, FileNotFoundError for a missing file. Matching the type lets callers catch precisely what they expect, the same way you catch the standard library's exceptions.

## 16.Opening and reading files
The open function is how Python gets at a file. You give it a path and a mode, and it hands back a file object you read from or write to.
```
f = open("/etc/hostname", "r")
```
The "r" means open for reading, which is also the default. The file object f is your handle to the contents.

##### Reading the whole file
read() pulls the entire file into a single string:
```
f = open("/etc/hostname", "r")
text = f.read()
f.close()
print(text)
```

close() releases the file. Every file you open should be closed, and the next node shows the clean way to guarantee that.

##### Reading line by line
Config files and logs are usually line oriented. readlines() gives you a list where each element is one line, newline character included:
```
f = open("/etc/hosts", "r")
lines = f.readlines()
f.close()
print(len(lines), "lines")
```

You can also loop over the file object directly, which reads one line at a time without loading the whole thing into memory:
```
f = open("/var/log/dpkg.log", "r")
for line in f:
    if " installed " in line:
        print(line.strip())
f.close()
```
line.strip() trims the trailing newline and any surrounding spaces.

#### Writing files and the with statement
Calling close() by hand is easy to forget, and a file left open can lose data that Python has buffered but not yet flushed. The with statement fixes this by closing the file for you, even if an error is raised partway through.
```
with open("/tmp/report.txt", "w") as f:
    f.write("build succeeded\n")
```

When the indented block ends, the file is closed automatically. This is the form you should reach for every time you touch a file.

##### Writing text
write() takes a string and writes it exactly as given. It does not add a newline, so include \n yourself when you want one:
```
with open("/tmp/hosts.txt", "w") as f:
    f.write("web1\n")
    f.write("web2\n")
```

To write a list of lines at once, writelines() takes an iterable of strings and writes them back to back.

#### The modes
The second argument to open decides what you can do:

"r" opens for reading. This is the default and fails if the file is missing.
"w" opens for writing and truncates the file to empty first. A brand new file is created if it does not exist.
"a" opens for appending. Writes go to the end and nothing already in the file is lost.
The difference between "w" and "a" matters. Opening a log with "w" wipes it on every run; "a" adds to it. Choose deliberately.

#### Paths with pathlib
The pathlib module represents a filesystem path as an object instead of a raw string. It joins paths correctly on any operating system and bundles the common file operations onto the path itself.
```
from pathlib import Path
config = Path("/etc/myapp/config.yaml")
```
##### Building paths
The / operator joins path pieces, so you never hand-glue strings with slashes again:
```
base = Path("/etc/myapp")
config = base / "config.yaml"
print(config)          # /etc/myapp/config.yaml
print(config.name)     # config.yaml
```
config.name is the final component, handy when you only want the filename.

##### Checking and reading
exists() tells you whether the path is really there, which saves you from crashing on a missing file:
```
if config.exists():
    text = config.read_text()
```

read_text() reads the whole file into a string and write_text() writes a string out, each opening and closing the file for you:
```
Path("/tmp/note.txt").write_text("done\n")
```

##### Listing a directory
iterdir() yields a Path for every entry in a folder:
```
for entry in Path("/etc").iterdir():
    print(entry.name)
```

## 17.What is JSON
JSON is a text format for structured data. Almost every REST API speaks it, and plenty of config and state files use it too. Learning to move data in and out of JSON is a daily DevOps task.

The reason it feels natural in Python is that JSON maps directly onto types you already know.

##### The shapes
A JSON document is built from a small set of pieces:

An object, written with {}, becomes a Python dict.
An array, written with [], becomes a Python list.
A string in double quotes becomes a str.
A number becomes an int or float.
true and false become True and False.
null becomes None.
Here is a small JSON document:
```
{
  "service": "api",
  "port": 8080,
  "enabled": true,
  "tags": ["web", "public"]
}
```

Read into Python, that is a dict with four keys. data["port"] is the integer 8080, data["enabled"] is True, and data["tags"] is a list of two strings.

##### Why this matters
Because JSON becomes ordinary dicts and lists, you work with it using everything you already know: indexing, .get(), loops, comprehensions. There is no special JSON object to learn. You parse the text once, then it is just Python data.

The two nodes that follow show the two directions: turning JSON text into Python objects, and turning Python objects back into JSON text.

##### What this node introduces
JSON objects map to dicts and JSON arrays map to lists.
Strings, numbers, true/false, and null map to str, int or float, True/False, and None.
Once parsed, JSON is just dicts and lists you already know how to use.

#### loads and dumps
The json module in the standard library converts between JSON text and Python objects. Two functions handle strings: loads parses a string into objects, and dumps renders objects back into a string.
```
import json
```

A memory aid: the s on the end stands for string. loads loads from a string, dumps dumps to a string.

##### Parsing a string with loads
json.loads takes JSON text and gives you Python data:
```
import json

raw = '{"service": "api", "port": 8080}'
data = json.loads(raw)
print(data["port"])     # 8080
print(type(data))       # <class 'dict'>
```

This is exactly what you do with the body of an API response, which arrives as a JSON string.

##### Producing a string with dumps
json.dumps goes the other way, turning a dict or list into JSON text:
```
import json

data = {"service": "api", "port": 8080}
print(json.dumps(data))
```

By default the output is packed onto one line. Pass indent to make it readable, which is what you want when a human will read the file:
```
print(json.dumps(data, indent=2))
```
That prints the object across several lines with two space indentation. Add sort_keys=True to order the keys alphabetically for stable output.

#### Reading and writing JSON files
Most of the time the JSON you care about lives in a file, not a string variable. The json module has a second pair of functions for that: load reads from an open file, and dump writes to one. Note the missing s, since these work on files rather than strings.

##### Reading a file with load
Open the file, then hand the file object to json.load:
```
import json

with open("/etc/myapp/config.json") as f:
    config = json.load(f)

print(config["service"])
```

json.load(f) reads the whole file and parses it in one step, so you get back a dict or list directly. Combined with the with statement, the file is parsed and closed cleanly.

##### Writing a file with dump
json.dump takes the object first and the open file second:
```
import json

config = {"service": "api", "port": 8080}
with open("/etc/myapp/config.json", "w") as f:
    json.dump(config, f, indent=2)
```

Open the file in write mode, pass indent=2 for a readable layout, and the dict is written out as JSON. This is the standard way to persist structured state, such as a generated config or a cache of API results.

##### load versus loads
Keep the two pairs straight:

load and dump work with a file object.
loads and dumps work with a string.
Reach for the file pair when the data is on disk, and the string pair when you already hold the JSON as text, such as an API response body.

##### What this node introduces
json.load(f) reads and parses JSON from an open file.
json.dump(obj, f, indent=2) writes an object as JSON to an open file.
The file pair load/dump versus the string pair loads/dumps.

## 18.YAML and PyYAML
YAML is the config format you meet everywhere in DevOps: Kubernetes manifests, Ansible playbooks, GitHub Actions and GitLab CI pipelines, Docker Compose files. It stores the same kind of structured data as JSON but is written to be easy for people to read and edit.

Python does not parse YAML out of the box. The standard library is PyYAML, imported as yaml, and it is already installed on this lab machine.
```
import yaml
```

##### Why YAML for config
YAML earns its place in config files for a few reasons:

Indentation shows structure, so there are no braces or brackets to balance.
It allows comments with #, which JSON does not. Config files live and die by good comments.
Strings usually need no quotes, which keeps files clean.
A small YAML config looks like this:
```
# service configuration
service: api
port: 8080
enabled: true
tags:
  - web
  - public
```

##### The same shapes as JSON
Under the surface YAML describes the same data model as JSON. That config parses into a Python dict with the keys service, port, enabled, and tags, where tags is a list. Mappings become dicts, sequences become lists, and scalars become strings, numbers, or booleans, exactly like JSON.

Because the result is ordinary dicts and lists, everything you already know about working with them applies. The next node covers the safe way to parse YAML text into those objects.

#### safe_load
yaml.safe_load parses a YAML string or file into Python dicts and lists. It is the function you should always use to read YAML.
```
import yaml

text = """
service: api
port: 8080
tags:
  - web
  - public
"""
config = yaml.safe_load(text)
print(config["port"])     # 8080
```

safe_load accepts either a string of YAML or an open file object, and returns the parsed data.

##### Why safe, and why it matters
PyYAML also has a plain yaml.load. The problem is that full YAML can describe more than plain data: with the default loader it can be told to construct arbitrary Python objects while parsing. A malicious YAML file can use that to run code on your machine the moment you load it.

In DevOps you routinely load YAML that came from a pull request, a templating step, or a shared repository. Treating that input as trusted code is a real risk. safe_load sidesteps it entirely by refusing to build anything except the basic data types.

##### The rule
Always use safe_load. Only reach for the unsafe yaml.load if you fully control the file and specifically need a feature safe_load does not support, which is rare. For reading config, safe_load is correct every time.
```
import yaml

with open("/etc/myapp/config.yaml") as f:
    config = yaml.safe_load(f)
```

##### What this node introduces
yaml.safe_load(text_or_file) parses YAML into dicts and lists.
Plain yaml.load can execute arbitrary code from a crafted file.
Always use safe_load to read YAML, especially from untrusted sources.

#### Dumping YAML
To turn Python objects back into YAML text, use yaml.safe_dump. It is the writing counterpart to safe_load and, like it, sticks to plain data types so the output is clean, portable YAML.
```
import yaml

config = {"service": "api", "port": 8080, "tags": ["web", "public"]}
print(yaml.safe_dump(config))
```

That prints:
```
port: 8080
service: api
tags:
- public
- web
```

##### Controlling the output
Two keyword arguments come up often:

sort_keys=False keeps your keys in the order you inserted them instead of sorting them alphabetically, which is the default.
default_flow_style=False forces the expanded block layout you see above, with each item on its own line, rather than the compact inline form.
python
```
print(yaml.safe_dump(config, sort_keys=False))
```

##### Writing to a file
Pass an open file as the second argument and safe_dump writes the YAML straight to disk:
```
import yaml

config = {"service": "api", "port": 8080}
with open("/etc/myapp/config.yaml", "w") as f:
    yaml.safe_dump(config, f, sort_keys=False)
```

This is how you generate a config file from data your program computed, such as rendering a deployment manifest from a template.

#### What this node introduces
yaml.safe_dump(obj) renders dicts and lists into YAML text.
sort_keys=False preserves key order; default_flow_style=False forces the block layout.
Passing an open file writes the YAML directly to that file.

## 19.The csv module
CSV is the format spreadsheets, billing exports, and monitoring dumps hand you: rows of values separated by commas, one record per line. It looks simple enough to split on commas yourself, but quoting and escaped values make that fragile. The standard library csv module handles the rules for you.
```
import csv
```

##### Reading rows with reader
csv.reader wraps an open file and yields one row at a time, each row a list of strings:
```
import csv

with open("/root/data/metrics.csv", newline="") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```

For a file starting host,cpu,mem, the first row comes back as ["host", "cpu", "mem"]. Every value is a string, so a number like 85 arrives as "85" and you convert it with int() when you need arithmetic.

##### Writing rows with writer
csv.writer goes the other way. writerow takes a list and writes it as one comma separated line:
```
import csv

with open("/root/data/out.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["host", "cpu"])
    writer.writerow(["web1", "85"])
```

#### Why newline=""
Notice newline="" on every open. The csv module manages line endings itself, and passing newline="" stops Python from adding extra blank lines between rows on some systems. Always open CSV files this way.

What this node introduces
```
csv.reader(f) yields each row as a list of strings.
csv.writer(f) writes rows; writerow takes a list for one line.
Open CSV files with newline="" so line endings are handled correctly.
```

#### DictReader and DictWriter
Reading rows as plain lists means referring to columns by number, and row[1] tells you nothing about what that column holds. DictReader fixes this by using the header row to give every value a name.

##### Reading with DictReader
csv.DictReader reads the first row as the header, then returns each following row as a dict keyed by those column names:
```
import csv

with open("/root/data/metrics.csv", newline="") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["host"], row["cpu"])
```

For a file starting host,cpu,mem, each row is a dict like {"host": "web1", "cpu": "85", "mem": "40"}. Now you reach for columns by name, which is clearer and survives someone reordering the columns. Values are still strings, so wrap them in int() for math.

##### Writing with DictWriter
csv.DictWriter is the mirror image. You tell it the column names up front with fieldnames, call writeheader to emit the header row, then pass a dict per row:
```
import csv

rows = [{"host": "web1", "cpu": "85"}]
with open("/root/data/out.csv", "w", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["host", "cpu"])
    writer.writeheader()
    for row in rows:
        writer.writerow(row)
```

Each dict's keys must match the fieldnames you declared. DictWriter puts the values in the right column order for you, so you never track positions by hand.

#### What this node introduces
csv.DictReader(f) yields each row as a dict keyed by the header.
csv.DictWriter(f, fieldnames=[...]) writes dicts to columns.
writeheader() emits the header row before the data rows.

#### Writing CSV files
Writing CSV is where doing it by hand bites you, and where the csv module earns its place. It quotes and escapes values so the file you produce reads back correctly.

##### Writing several rows at once
You already have writerow for a single row. writerows takes a list of rows and writes them all:
```
import csv

rows = [
    ["host", "cpu"],
    ["web1", "85"],
    ["db1", "92"],
]
with open("/root/data/out.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerows(rows)
```

##### Quoting is handled for you
Here is the reason not to build CSV with string joins. Suppose a value contains a comma:
```
import csv

with open("/root/data/notes.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["web1", "high load, investigating"])
```

The writer sees the comma inside the value and wraps that field in quotes automatically, producing web1,"high load, investigating". A reader parses it back as two fields, not three. Values with quotes or newlines are escaped the same way. Joining strings with commas yourself would silently corrupt the file the first time a value held a comma.

##### The pattern
The everyday shape is: open with newline="" and mode "w", make a writer, write a header row, then write the data rows. Let the module worry about commas, quotes, and line endings.

##### What this node introduces
writerows(rows) writes a list of rows in one call.
The writer quotes and escapes values containing commas, quotes, or newlines automatically.
Standard pattern is open with newline="", write a header, write rows.

## 20.The re module
Python's regular-expression support lives in the re module, part of the standard library. Import it and you can search text for patterns, pull out matching pieces, and check that a string has the shape you expect.
```
import re
```

##### Why raw strings
Regex patterns lean heavily on the backslash: \d for a digit, \. for a literal dot, \s for whitespace. In a normal Python string the backslash is also an escape character, so "\d" is easy to misread and "\n" turns into a newline. To keep a pattern exactly as written, use a raw string by prefixing it with r:
```
pattern = r"\d+\.\d+"
```

The r tells Python to leave every backslash alone and hand the raw text straight to re. Always write regex patterns as raw strings. It costs nothing and avoids a whole class of confusing bugs.

##### A first match
The quickest way to try a pattern is re.search, which looks for the pattern anywhere in a string and returns a match object, or None when there's nothing:
```
import re

text = "release 1.20 shipped"
match = re.search(r"\d+\.\d+", text)
if match:
    print(match.group())   # 1.20
```

match.group() returns the slice of text the pattern matched. A None result means no match, which is why the if guard matters before you call .group().

##### search, match, and findall
Three functions in re cover most day-to-day work. They differ in where they look and what they hand back.

re.search: find one match anywhere
re.search scans the whole string and returns the first match, or None:
```
import re
re.search(r"\d+", "load is 42 today")   # matches "42"
re.search(r"\d+", "no numbers here")    # None
```

##### re.match: anchored at the start
re.match only matches at the beginning of the string. If the pattern isn't right at the start, it returns None even when the text appears later:
```
re.match(r"\d+", "42 servers")   # matches "42"
re.match(r"\d+", "load is 42")   # None, doesn't start with a digit
```

Reach for search when the thing you want can sit anywhere. Use match only when you truly mean "must start with".

##### re.findall: every match at once
re.findall returns a list of every non-overlapping match, which is perfect for pulling repeated values out of a line:
```
re.findall(r"\d+", "3 errors, 12 warnings, 5 ok")
# ['3', '12', '5']
```

An empty list means nothing matched. Because it returns plain strings, findall is the workhorse for counting and extracting.

##### What this node introduces
re.search returns the first match anywhere, or None.
re.match only matches at the start of the string.
re.findall returns a list of all matches. 

##### Groups and capturing
Parentheses in a pattern create a capturing group: a sub-part of the match you can pull out on its own. This is how you go from "the line matched" to "here are the exact fields I care about".

##### Numbered groups
Wrap the piece you want in (). group(1) returns the first group, group(2) the second, and group(0) (or group()) the whole match:
```
import re
m = re.search(r"(\d+)\.(\d+)", "version 1.20 here")
m.group(0)   # '1.20'  the full match
m.group(1)   # '1'     first group
m.group(2)   # '20'    second group
```

##### groups() returns them all
m.groups() gives every captured group as a tuple, handy for unpacking:
```
major, minor = m.groups()
```

##### Named groups
Past a couple of fields, numbers get hard to track. Name a group with (?P<name>...) and read it with group("name"):
```
m = re.search(r"user=(?P<user>\w+)", "user=alice action=login")
m.group("user")   # 'alice'
```

##### Groups inside findall
When a pattern has groups, re.findall returns the captured groups instead of the whole match. One group gives a list of strings, two or more gives a list of tuples:
```
re.findall(r"(\w+)=(\w+)", "user=alice action=login")
# [('user', 'alice'), ('action', 'login')]
```

#### What this node introduces
() creates a capturing group; group(n) reads it.
groups() returns all captured groups as a tuple.
(?P<name>...) names a group; group("name") reads it.
With groups, findall returns the captured pieces.

#### Parsing log lines
Log parsing is where regex earns its keep in DevOps work. A log line has a predictable shape, and a single pattern with named groups can break it into fields you can count, filter, and report on.

##### A sample line
Say your application writes lines like this:
```
2026-07-24 08:18:44 ERROR user=bob action=checkout status=500
````
Four things matter: the timestamp, the level, the user, and the status code. One pattern captures the ones you need:
```
import re
line = "2026-07-24 08:18:44 ERROR user=bob action=checkout status=500"
pattern = r"(?P<time>\S+ \S+) (?P<level>\w+) .* status=(?P<status>\d+)"

m = re.search(pattern, line)
m.group("level")    # 'ERROR'
m.group("status")   # '500'
```

\S+ matches non-whitespace, so \S+ \S+ grabs the date and time. \w+ matches the level word. The .* skips the middle, and status=(\d+) pins the status code at the end.

##### Looping over a file
Open the log, run the pattern on each line, and act on the matches. A common job is counting errors:
```
import re
errors = 0
with open("/root/logs/app.log") as f:
    for line in f:
        if re.search(r"\bERROR\b", line):
            errors += 1

print(errors)
```

\b is a word boundary, so \bERROR\b matches the level word ERROR without also matching something like ERRORS or PREERROR.

##### What this node introduces
Named groups pull labelled fields out of a structured line.
\S, \w, and \b match non-whitespace, word characters, and word boundaries.
Running a pattern over each line of a file counts or extracts events.

## 21.The datetime module
Working with time in Python starts with the datetime module. It gives you a datetime type for a full moment (date plus time) and a date type for a calendar day on its own.
```
from datetime import datetime, date
```

##### The current moment
datetime.now() returns the current local date and time as a datetime object:
```
now = datetime.now()
print(now)   # 2026-07-24 14:03:51.412000
```

date.today() gives just today's calendar date:
```
date.today()   # 2026-07-24
```

##### Building a specific moment
Pass year, month, and day (and optionally hour, minute, second) to construct an exact value:
```
released = datetime(2026, 3, 1, 9, 30, 0)
deadline = date(2026, 7, 30)
```

##### Reading the parts
Every datetime and date exposes its fields as attributes:
```
now = datetime.now()
now.year     # 2026
now.month    # 7
now.day      # 24
now.hour     # 14
```

These are plain integers, so you can compare them, print them, or feed them into other calculations.

#### What this node introduces
```datetime.now() returns the current date and time.
date.today() returns the current calendar date.
datetime(y, m, d, ...) and date(y, m, d) build specific values.
.year, .month, .day, .hour read the parts.
```

#### Formatting with strftime and strptime
A datetime prints in a default layout, but reports and filenames need a specific format, and logs arrive as strings you have to read back. Two methods bridge objects and text. strftime turns a datetime into a string, and strptime parses a string into a datetime.

##### strftime: datetime to string
Call strftime on a datetime with a format made of placeholder codes:
```
from datetime import datetime

now = datetime(2026, 7, 24, 14, 3)
now.strftime("%Y-%m-%d")   # '2026-07-24'
now.strftime("%d %b %Y")   # '24 Jul 2026'
now.strftime("%H:%M")      # '14:03'
```

The common codes:
    %Y four-digit year, %m two-digit month, %d two-digit day.
    %H hour (24-hour), %M minute, %S second.
    %b abbreviated month name, %A full weekday name.

##### strptime: string to datetime
strptime is a function on the datetime class. Give it the string and a format that describes its shape:
```
from datetime import datetime

text = "2026-03-08 15:45:00"
moment = datetime.strptime(text, "%Y-%m-%d %H:%M:%S")
moment.year    # 2026
```

The format must match the input exactly, separators and all. If it doesn't, strptime raises a ValueError.

#### What this node introduces
    dt.strftime(fmt) formats a datetime as a string.
    datetime.strptime(text, fmt) parses a string into a datetime.
    Format codes include %Y %m %d %H %M %S %b %A.

#### timedelta and arithmetic
Subtract one datetime from another and you get a timedelta, the duration between them. Add a timedelta to a datetime and you get a new datetime shifted by that amount. This is how you answer "how long ago" and "how long until".
```
from datetime import datetime, timedelta
```

##### Subtracting two datetimes
```
start = datetime(2026, 3, 1, 9, 30, 0)
end = datetime(2026, 3, 8, 15, 45, 0)
gap = end - start
gap.days             # 7          whole days
gap.total_seconds()  # 628200.0   the whole span in seconds
```
gap.days is the number of complete days. total_seconds() gives the entire duration as seconds, which you can divide to get hours or minutes.

##### Building a duration
Create a timedelta directly with the units you care about, then add or subtract it:
```
deadline = start + timedelta(days=30)
yesterday = datetime.now() - timedelta(days=1)
timedelta accepts days, hours, minutes, seconds, and weeks.
```

##### Comparing moments
Datetimes compare with the normal operators, so you can check whether one time is before another:
```
if datetime.now() > deadline:
    print("past due")
```

#### What this node introduces
    Subtracting two datetimes returns a timedelta.
    .days and .total_seconds() read a duration.
    timedelta(days=..., hours=...) builds a duration to add or subtract.
    Datetimes compare with <, >, and ==.

## 22.The os module
The os module is Python's door to the operating system: the working directory, the filesystem layout, and the environment. This node covers the path and directory helpers you'll reach for constantly.
```
import os
```

##### Where am I
os.getcwd() returns the current working directory as a string:
```
os.getcwd()   # '/root/project'
````

##### Listing and creating directories
os.listdir(path) returns the names in a directory as a list. os.makedirs(path) creates a directory, including any missing parents:
```
os.listdir("/root")            # ['project', 'answers', ...]
os.makedirs("/root/logs/app")  # creates logs and logs/app
```

Pass exist_ok=True to makedirs so it doesn't error when the folder is already there:
```
os.makedirs("/root/logs", exist_ok=True)
```

##### Building and inspecting paths
os.path.join joins path pieces with the right separator, which is safer than gluing strings with slashes:
```
os.path.join("/root", "logs", "app.log")   # '/root/logs/app.log'
```

os.path.exists(path) tells you whether a path is there, and os.path.basename and os.path.dirname split a path into its file name and its folder:
```
os.path.exists("/root/logs")            # True
os.path.basename("/root/logs/app.log")  # 'app.log'
os.path.dirname("/root/logs/app.log")   # '/root/logs'
```

#### What this node introduces
- os.getcwd() returns the current directory.
- os.listdir(path) lists a directory; os.makedirs(path) creates one.
- os.path.join, os.path.exists, os.path.basename, and os.path.dirname build and inspect paths.

#### Environment variables
Configuration usually reaches a script through environment variables: secrets, region names, feature flags, the current user. Python exposes them through os.environ.
```
import os
```

##### Reading a variable
os.environ behaves like a dictionary of strings. Index it by name to read a value:
```
os.environ["HOME"]   # '/root'
```

The catch is that indexing a variable that isn't set raises KeyError and crashes the script.

##### Reading safely with get
os.environ.get returns None, or a default you supply, when the variable is missing, so it never raises:
```
os.environ.get("LOG_LEVEL")          # None if unset
os.environ.get("LOG_LEVEL", "info")  # 'info' if unset
```

For any optional setting, get with a default is the right habit. It keeps the script running with a sensible fallback instead of failing.

##### Setting a variable
Assigning to os.environ sets a variable for this process and any programs it launches:
```
os.environ["DEPLOY_ENV"] = "staging"
```

#### What this node introduces
    os.environ["NAME"] reads a variable and raises KeyError if unset.
    os.environ.get("NAME", default) reads with a fallback, never raising.
    Assigning to os.environ sets a variable for this process.

### sys.argv and exit codes
The sys module connects your script to how it was run: the arguments on the command line and the exit code it returns. Both matter the moment a script becomes a tool that other scripts call.
```
import sys
```

##### Command-line arguments
sys.argv is a list of the words on the command line. sys.argv[0] is the script's own name, and the real arguments start at index 1:
```
# run as: python3 deploy.py production
import sys
sys.argv        # ['deploy.py', 'production']
sys.argv[1]     # 'production'
```

If the caller forgot to pass an argument, sys.argv[1] raises IndexError, so check the length first:
```
if len(sys.argv) < 2:
    print("usage: deploy.py <environment>")
    sys.exit(1)
```

##### Exit codes
sys.exit(code) ends the program and returns an exit code to the shell. 0 means success, and any non-zero value signals a specific failure:
```
sys.exit(0)   # success
sys.exit(2)   # a defined error
```

The shell reads this code as $?, and automation uses it to decide whether the step passed. A script that always exits 0 can't be trusted in a pipeline.

##### What this node introduces
    sys.argv holds the command-line arguments; real ones start at index 1.
    Guard against a missing argument by checking len(sys.argv).
    sys.exit(code) ends the program with an exit code, where 0 is success.

## 23.The subprocess module
Sometimes the tool you need already exists as a command: df, git, systemctl, kubectl. Rather than reimplement it, you run it from Python and work with what it prints. The subprocess module in the standard library does exactly that.

##### subprocess.run
The one function you'll reach for is subprocess.run. Pass it the command as a list, where the first item is the program and the rest are its arguments:
```
import subprocess
subprocess.run(["ls", "-l", "/etc"])
```

This runs ls -l /etc, and its output goes straight to your terminal, just as if you had typed the command yourself. run waits for the command to finish before your Python continues.

##### Why a list, not a string
Each argument is its own item in the list. You write ["ls", "-l"], not "ls -l". Python hands that list straight to the operating system as the program and its arguments, so nothing reinterprets your spaces or quotes.

There is another form, shell=True, that takes a single string and runs it through the shell:
```
subprocess.run("ls -l /etc", shell=True)
```

It looks convenient, but it hands your string to a full shell, which interprets characters like ;, |, and $. If any part of that string comes from outside your program (a filename a user typed, a value from an API), someone can smuggle in extra commands. That's a command-injection bug. The next scenario looks at it in detail.

The habit to build: use the list form. Reach for shell=True only for a fixed string you wrote yourself, never with untrusted input.

#### Capturing output
Running a command is only half the job. Most of the time you want to read what it printed and use it in your program: the number of files, a version string, a list of running containers. By default that output goes to the terminal, not to your code. Two arguments change that.

##### capture_output and text
Pass capture_output=True to grab the command's output instead of letting it print. Pass text=True so you get back a normal string rather than raw bytes:
```
import subprocess
result = subprocess.run(
    ["df", "-h", "/"],
    capture_output=True,
    text=True,
)
```
run returns a result object. What the command printed to standard output is now waiting in result.stdout as a string.

##### Reading stdout and stderr
A command has two output streams. Normal output goes to .stdout, and error messages go to .stderr:
```
print(result.stdout)
print(result.stderr)
```

Both are plain strings when you use text=True. You can slice them, split them into lines, or feed them to the string and regex tools you already know:
```
lines = result.stdout.splitlines()
```

##### Why text=True matters
Without text=True, .stdout comes back as bytes, printed like b'Filesystem ...'. You'd have to decode it yourself before you could work with it. Passing text=True decodes it for you, so you get a string every time. Make it a habit to set both arguments together.

#### What this node introduces
capture_output=True collects the command's output instead of printing it.
text=True returns that output as a string, not bytes.
result.stdout holds normal output; result.stderr holds errors.

#### Return codes and errors
Every command reports whether it succeeded through an exit code. Zero means success. Anything else means something went wrong, and the number often says what. Automation lives or dies on checking this, because a command that quietly failed can wreck everything downstream.

##### returncode
The result object carries the exit code in .returncode:
```
import subprocess
result = subprocess.run(["ls", "/nope"], capture_output=True, text=True)
print(result.returncode) 
```

For a path that doesn't exist, ls exits non-zero, so .returncode is something like 2. You can branch on it:
```
if result.returncode != 0:
    print("command failed:", result.stderr)
```

By default run does not care whether the command failed. It hands back the result and lets you decide. If you never look at .returncode, a failure slips by unnoticed.

##### check=True
When a failed command should stop your program, pass check=True. Now a non-zero exit raises an exception instead of returning quietly:
```
subprocess.run(["ls", "/nope"], check=True)
```

This is the safer default for scripts that must not continue after a failed step, like a deploy that shouldn't proceed if the build broke.

##### CalledProcessError
The exception check=True raises is subprocess.CalledProcessError. Catch it to handle the failure yourself:
```
try:
    subprocess.run(["ls", "/nope"], check=True)
except subprocess.CalledProcessError as err:
    print("exit code was", err.returncode)
```

The exception object still carries .returncode, and .stdout and .stderr when you captured them, so you can log exactly what happened.

#### What this node introduces
    result.returncode holds the command's exit code (0 means success).
    check=True raises an exception on a non-zero exit.
    subprocess.CalledProcessError is that exception; catch it to react.

## 24.argparse basics
A script that needs input can read sys.argv by hand, but then you're writing your own code to check how many arguments came in, print usage when they're wrong, and convert strings to numbers. The argparse module in the standard library does all of that for you and gives your script the same feel as ls or git.

##### The parser
Start by creating an ArgumentParser. A short description shows up in the tool's help text:
```
import argparse
parser = argparse.ArgumentParser(description="Scale a service")
```

This object collects the arguments your tool accepts. You describe each one, then ask it to read what the user actually typed.

##### parse_args
Once the parser knows its arguments, parse_args reads them from the command line and returns an object holding the values:
```
args = parser.parse_args()
```

parse_args looks at what came in on the command line, checks it against the arguments you defined, and stops the program with a helpful message if something is missing or wrong. When it returns, you have clean, validated values to work with.

##### The shape of a tool
Almost every argparse tool follows the same three steps:
```
import argparse
parser = argparse.ArgumentParser(description="Scale a service")
# (arguments get added here)
args = parser.parse_args()
# (your logic uses args here)
```

The next nodes fill in the middle: how to add arguments the user must supply, optional flags, and how to control their help text and types.

##### What this node introduces
    argparse.ArgumentParser(description=...) creates a parser.
    parser.parse_args() reads and validates the command line.
    The returned object holds the values your code then uses.

#### Positional and optional arguments
You add arguments to a parser with add_argument. There are two kinds, and the difference is whether the name starts with a dash.

##### Positional arguments
A positional argument is required and identified by its place on the command line. Add one by giving add_argument a plain name:
```
parser.add_argument("service")
```

Now the user must supply a value, and it lands on args.service:
```
python3 scale.py web
```

Here args.service is "web". Leave it out and argparse stops the program with a usage message, so you never have to check for it yourself.

##### Optional arguments
An optional argument starts with -- and, as the name says, can be left out. Add one the same way, with a dashed name:
```
parser.add_argument("--replicas")
```

The user supplies it by flag, in any order:
```
python3 scale.py web --replicas 3
```

argparse drops the leading dashes to name the attribute, so this value lands on args.replicas. Skip the flag and args.replicas is None until you give it a default, which the next node covers.

##### Flags that are just on or off
Some options are a simple yes or no, like --verbose. Setting action="store_true" makes the argument a switch that's True when present and False when absent, with no value to type:
```
parser.add_argument("--verbose", action="store_true")
```

#### What this node introduces
    add_argument("name") defines a required positional argument.
    add_argument("--name") defines an optional flag, read from args.name.
    action="store_true" makes a flag a simple on/off switch.

#### Help, defaults, and types
Three keyword arguments to add_argument turn a bare argument into one that documents itself, fills in sensible values, and hands you the right data type.

##### help
Pass help= to describe an argument. That text shows up when someone runs your tool with -h:
```
parser.add_argument("service", help="name of the service to scale")
parser.add_argument("--replicas", help="how many copies to run")
```

argparse builds the whole help screen for you, so python3 scale.py -h prints usage plus these descriptions. You never write that screen by hand.

##### default
An optional argument left out by the user is None unless you set a default. Give it the value the tool should assume:
```
parser.add_argument("--replicas", default=1)
```

Now leaving off --replicas puts 1 on args.replicas instead of None, so the rest of your code doesn't have to handle a missing value.

##### type
Everything from the command line arrives as a string. "3" is text, not the number 3. Pass type= to have argparse convert it for you:
```
parser.add_argument("--replicas", type=int, default=1)
```

Now args.replicas is a real integer you can do math on. As a bonus, argparse rejects input that can't convert: pass --replicas abc and it stops with a clear error, before your code ever runs. Common choices are type=int and type=float.

#### What this node introduces
help= gives an argument its description in the -h output.
default= sets the value used when an optional argument is omitted.
type=int converts and validates the value, so you get a number, not a string.

## 25.Logging versus print
A quick print is fine while you're poking at a script. Once that script runs unattended, on a schedule or as part of a pipeline, print stops being enough. The logging module in the standard library is built for programs that run on their own and need a record of what they did.

##### What print can't do
A print call always fires, always goes to the same place, and carries no context. When a scheduled job fails at 3am, a screen full of bare print lines tells you little. You can't dial the detail up or down, you can't tell an ordinary message from an emergency, and you can't send the output to a file without redirecting the whole program.

##### What logging adds
Logging gives you three things print never will:
```
Severity. Every message has a level, from routine detail up to critical failure, so you can tell what matters at a glance.
A volume knob. You set a threshold, and messages below it are dropped. Turn up detail while debugging, turn it down in production, without touching the log calls themselves.
Destinations and format. The same messages can go to the screen, a file, or both, each line stamped with a time and a level.
```

##### The first log message
The simplest use looks a lot like print:
```
import logging
logging.warning("disk space is low")
```

Out of the box this prints WARNING:root:disk space is low. The level is right there in the output, which a plain print could never give you. The next nodes cover the levels and how to control where these lines go and what they look like.

#### What this node introduces
```
logging is the standard tool for output from scripts that run unattended.
It adds severity levels, a threshold to filter by, and control over format and destination.
logging.warning(...) and its siblings replace print for this.
```

#### Log levels
Logging sorts every message into one of five severity levels. Picking the right one is what lets you later filter noise from signal.

##### The five levels
From least to most severe:

DEBUG is fine-grained detail useful only while diagnosing a problem. Variable values, "entering this function", that sort of thing.
INFO confirms things are working as expected. "Backup started", "processed 40 records".
WARNING flags something unexpected that didn't stop the program. "Disk usage at 85 percent", "retrying after a timeout".
ERROR means an operation failed. "Could not connect to the database", "file not found".
CRITICAL is a serious failure that may bring the whole program down. "Out of disk", "unrecoverable state".
Each level has a matching function:
```
import logging
logging.debug("connecting to host")
logging.info("backup started")
logging.warning("disk usage at 85 percent")
logging.error("could not connect to database")
logging.critical("out of disk space")
```

##### The threshold
The level does more than label a message. Logging has a threshold, and anything below it is dropped. The default threshold is WARNING, which is why logging.info(...) on its own seems to print nothing: INFO sits below WARNING and gets filtered out.

That filtering is the point. In production you might run at WARNING and see only problems. While chasing a bug you drop the threshold to DEBUG and suddenly every detail appears, with no change to the log calls themselves. The next node shows how to set that threshold.

##### What this node introduces
The five levels, from DEBUG up to CRITICAL, and when each fits.
A matching function for each: logging.debug through logging.critical.
Messages below the current threshold are dropped; the default threshold is WARNING.

#### Configuring format and handlers
One function sets up logging for a whole script: logging.basicConfig. It controls the threshold, the look of each line, and where the lines go. Call it once, near the top of your program, before any log calls.

##### Setting the level
Pass level= to lower the threshold from its WARNING default so more messages get through:
```
import logging
logging.basicConfig(level=logging.INFO)
logging.info("backup started")
```

With the threshold at INFO, that message now appears. Set it to logging.DEBUG to see everything.

##### Setting the format
Pass format= to control what each line looks like. You build it from named fields that logging fills in:
```
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s",
)
```

Now every line is stamped with the time (asctime), its level (levelname), and the text (message), giving output like 2026-07-24 09:00:00,123 INFO backup started. Those percent-named fields are the standard placeholders logging understands.

##### Sending output to a file
By default logging writes to the screen. Pass filename= to send it to a file instead, which is what you want for a job that runs unattended:
```
logging.basicConfig(
    filename="/root/answers/app.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s",
)
```

Every message now appends to that file, each line timestamped and labelled. Under the hood filename= sets up a file handler, the object that decides where log records are written. You can configure handlers directly for more control, but basicConfig covers the common case in one call.

#### What this node introduces
    logging.basicConfig(...) configures logging once for the program.
    level= sets the threshold; format= sets each line's layout using fields like %(asctime)s and %(levelname)s.
    filename= writes to a file through a file handler instead of the screen.

## 26.The requests library
Most DevOps automation eventually talks to an HTTP API: checking a health endpoint, triggering a deploy, reading a metric, posting to a chat webhook. In Python, the requests library is how you do that. It turns an HTTP call into a single readable line.

Python ships with a lower-level urllib in the standard library, but it's clumsy for everyday work. requests is the community standard, and it's already installed in your lab environment.

##### Importing it
Bring the library in at the top of your script:
```
import requests
```

If that line runs without an error, the library is available.

##### A GET request
A GET request asks a server for data. Pass the URL to requests.get and you get back a response object:
```
import requests
r = requests.get("https://api.github.com/repos/python/cpython")
```

The call blocks until the server answers, then hands you an object that holds everything about the reply: the status code, the headers, and the body.

##### Reading the body
The response body is available two ways. Use r.text for the body as a string, and r.content for the raw bytes (useful for images or downloads):
```
print(r.text)
```

For a JSON API, you rarely want the raw text. The next node covers turning that body into a Python dictionary and checking whether the request actually succeeded.

##### What this node introduces
    import requests loads the library.
    requests.get(url) sends a GET request and returns a response object.
    r.text is the body as a string; r.content is the raw bytes.

#### Status codes and JSON
Every HTTP response carries a status code, a three-digit number that says whether the request worked. Before you trust a response body, you check that code.

r.status_code
The response object exposes the code as r.status_code:
```
r = requests.get("https://api.github.com/repos/python/cpython")
print(r.status_code)
```

The ranges you'll see most:
    200 OK, the request succeeded.
    201 Created, common after a successful POST.
    404 Not Found.
    401 and 403, authentication or permission problems.
    500 and up, the server broke.

##### raise_for_status
Rather than compare numbers by hand, let requests do it. r.raise_for_status() does nothing on a success code but throws an exception on any 4xx or 5xx:
```
r = requests.get(url)
r.raise_for_status()   # stops here if the request failed
```

This is the clean way to fail fast. Wrap it in the try/except you learned in error handling, and a bad response becomes a caught error instead of a script that carries on with garbage data.

##### r.json()
Most APIs answer with JSON. Instead of parsing r.text yourself, r.json() decodes the body straight into Python dicts and lists:
```
data = r.json()
print(data["stargazers_count"])
```

From here it's a normal dictionary. Everything you know about indexing keys and looping over lists applies.

#### What this node introduces
    r.status_code is the numeric HTTP status.
    r.raise_for_status() raises on a 4xx or 5xx response.
    r.json() parses a JSON body into Python objects.

##### POST, headers, and query params
A GET request reads. To send data, filter a result, or authenticate, you need three more pieces: POST requests, headers, and query parameters.

##### Query parameters
Query parameters go on the end of a URL after a ?, like ?userId=1&completed=true. Instead of building that string yourself, pass a dictionary as params and requests encodes it for you:
```
r = requests.get(
    "https://jsonplaceholder.typicode.com/todos",
    params={"userId": 1},
)
```

The request that goes out is .../todos?userId=1. This keeps values safely escaped and your code readable.

##### Headers
Headers carry metadata about the request: the content type, an API token, a user agent. Pass them as a dictionary with headers:
```
headers = {"Authorization": "Bearer my-token"}
r = requests.get(url, headers=headers)
```

API tokens almost always ride in an Authorization header like this.

##### POST requests
A POST request sends data to create or change something. Use requests.post. For a JSON API, pass your dict as json and requests serializes it and sets the content type for you:
```
payload = {"title": "deploy", "userId": 1}
r = requests.post(url, json=payload)
```

A successful create usually answers with 201. Use data= instead of json= when you need to send form-encoded fields rather than JSON.

#### What this node introduces
    params={...} adds query parameters to a request.
    headers={...} sends request headers such as Authorization.
    requests.post(url, json={...}) sends a JSON POST body.

## 27.What boto3 is
boto3 is the AWS SDK for Python. Everything you can do in the AWS console or with the aws CLI you can also do from Python code: launch an EC2 instance, upload to S3, read a queue, list IAM users. It is the standard way to automate AWS from a program.

It's already installed in your lab environment. Import it like any library:
```
import boto3
```

##### Clients
A client is a thin wrapper over the AWS API for one service. You create one by naming the service:
```
s3 = boto3.client("s3")
```

Client methods map almost one to one onto AWS API calls and CLI commands. s3.list_buckets(), s3.create_bucket(...), and s3.put_object(...) line up with what you'd run as aws s3api list-buckets and so on. Clients return plain Python dicts, the same shape as the JSON the API sends back.

##### Resources
boto3 also offers a higher-level "resource" interface that wraps those same calls in objects, so you write bucket.upload_file(...) instead of passing a bucket name to every call. It reads nicely, but it covers fewer services and AWS now steers new work toward clients.

For DevOps automation, stick with clients. They cover every service, they match the CLI you already know, and every AWS example you'll find online uses them. This topic uses clients throughout.

###### What this node introduces
    boto3 is the AWS SDK for Python.
    boto3.client("s3") creates a client for a named service.
    Clients map onto AWS API calls and return dictionaries; prefer them over the resource interface for automation.

#### Clients and credentials
Before boto3 can call AWS, it needs two things: which region to talk to, and which credentials to authenticate with. You rarely hard-code either.

##### Region
Most services are regional. You can set the region when you build the client:
```
s3 = boto3.client("s3", region_name="us-east-1")
```

If you leave it out, boto3 falls back to the AWS_DEFAULT_REGION environment variable or your AWS config file.

##### Where credentials come from
You never pass your access keys as plain strings in code. boto3 looks for credentials in a fixed order and uses the first it finds:

    Environment variables AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
    The shared credentials file at ~/.aws/credentials, written by aws configure.
    On an EC2 instance or in a container, the IAM role attached to it, fetched automatically.

That last option is the one you want in production: the instance carries a role, boto3 picks up short-lived credentials on its own, and no secret is ever stored on disk.

##### Labs use a mock
Real AWS calls cost money and need a real account. In these labs there is no live AWS behind boto3. Instead the environment uses moto, a library that stands in for AWS and answers boto3's calls locally. Your code is identical either way, you still write boto3.client("s3") and call the same methods. The mock just means the bucket you create lives in memory instead of in a real account, so you can practice without a bill.

#### What this node introduces
    region_name= sets the region on a client.
    boto3 finds credentials from env vars, then ~/.aws/credentials, then an instance role, in that order.
    Labs replace real AWS with the moto mock; your boto3 code is unchanged.

##### Working with S3
S3 is object storage: buckets hold objects, and each object is a blob of bytes stored under a key. It's where DevOps work parks artifacts, backups, logs, and static files. Three client calls cover the basics.

##### Create a bucket
In us-east-1, creating a bucket needs only its name:
```
s3 = boto3.client("s3", region_name="us-east-1")
s3.create_bucket(Bucket="my-artifacts")
```

Bucket names are global across all of AWS, so real ones must be unique. In every other region create_bucket also needs a CreateBucketConfiguration naming the region, but us-east-1 is the special default that doesn't.

##### Upload an object
put_object writes an object. You give it the bucket, a key, and the body as bytes:
```
s3.put_object(
    Bucket="my-artifacts",
    Key="deploy/config.txt",
    Body=b"version=1.0",
)
```

The key can contain slashes like deploy/config.txt. S3 has no real folders, but the console displays those prefixes as if they were.

##### List objects
list_objects_v2 returns what's in a bucket. The objects come back under a Contents key, each a dict with a Key:
```
resp = s3.list_objects_v2(Bucket="my-artifacts")
for obj in resp.get("Contents", []):
    print(obj["Key"])
```

Use resp.get("Contents", []) rather than resp["Contents"], because an empty bucket omits the key entirely and a plain lookup would raise.

##### What this node introduces
    create_bucket(Bucket=...) makes a bucket (name only in us-east-1).
    put_object(Bucket=, Key=, Body=) uploads an object.
    list_objects_v2(Bucket=) lists objects under the Contents key.

## 28.Retries and backoff
Anything that crosses a network fails sometimes: an API times out, a connection drops, a service is briefly overloaded. A one-off script can just crash. A script that runs unattended should try again before giving up. That's a retry loop.

##### time.sleep
To wait between attempts, use sleep from the built-in time module. It pauses the program for a number of seconds:
```
import time
time.sleep(2)   # pause for two seconds
```

##### A basic retry loop
Wrap the risky call in a loop, catch the exception, and try again:
```
import time
for attempt in range(3):
    try:
        result = do_the_thing()
        break
    except Exception:
        time.sleep(1)
```

This tries up to three times, pausing a second after each failure, and stops as soon as one succeeds.

##### Exponential backoff
Retrying immediately, or on a fixed delay, can make an overloaded service worse by hammering it. Exponential backoff spaces attempts further apart each time, doubling the wait: 1 second, then 2, then 4. You give a struggling service room to recover:
```
delay = 1
for attempt in range(3):
    try:
        result = do_the_thing()
        break
    except Exception:
        time.sleep(delay)
        delay = delay * 2
```

##### Give up eventually
A retry loop must have a limit. After the last attempt fails, re-raise the exception or exit with an error, so the failure is visible instead of silently swallowed. Retrying forever just hides a real outage.

#### What this node introduces
time.sleep(seconds) pauses execution.
A retry loop wraps a risky call in try/except and loops a fixed number of times.
Exponential backoff doubles the delay between attempts and always caps the number of tries.

##### Scheduling with cron
Automation usually needs to run on a schedule: a backup every night, a health check every five minutes. On Linux, cron is the scheduler. You don't write cron in Python, you write a Python script that cron calls.

##### The crontab line
A crontab entry is five time fields followed by the command to run. This runs a script every day at 2:30 am:
```
30 2 * * * /usr/bin/python3 /opt/scripts/backup.py
```

The fields are minute, hour, day of month, month, and day of week. A * means "every".

##### Exit codes matter
cron decides whether your script succeeded by its exit code, not by what it prints. An exit code of 0 means success; anything else means failure. Python exits 0 automatically when a script finishes normally, and non-zero when an uncaught exception crashes it.

To signal failure on purpose, use sys.exit with a non-zero number:
````
import sys
if not backup_succeeded:
    sys.exit(1)
```

A script that catches every error and always exits 0 looks healthy to cron even when it failed. That's how broken backups go unnoticed for months. Let real failures produce a non-zero exit. 

##### Cron runs with a bare environment
cron does not load your shell profile. The PATH is minimal and your usual environment variables may be missing. Two habits avoid surprises:

    Use absolute paths for the interpreter and the script, as in the line above.
    Don't assume env vars you set in your shell exist. Set what you need inside the crontab or the script.

##### Robust scripts
A script you run by hand and a script that runs unattended are held to different standards. Nobody is watching the scheduled one, so it has to report its own health. Three habits you've already met combine into a reliable tool: logging, error handling, and exit codes.

##### Log instead of print
A scheduled job's print output usually vanishes. Use the logging module so every run leaves a timestamped record you can read later:
```
import logging
logging.basicConfig(
    filename="/var/log/backup.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s",
)
logging.info("backup started")
```
When something breaks at 3 am, this log is how you find out what happened.

##### Catch errors, log them, then fail loudly
Wrap the real work in try/except. Log the error so there's a record, then exit non-zero so the scheduler knows it failed:
```
import sys
try:
    run_backup()
except Exception as e:
    logging.error("backup failed: %s", e)
    sys.exit(1)
logging.info("backup finished")
```

The order matters: log first so you keep the detail, then exit non-zero so cron records a failure.

##### Don't swallow errors
The tempting anti-pattern is a bare except that logs and carries on, or worse, does nothing. That hides failures behind a green light. Handle the errors you can actually recover from, and let the rest stop the script with a non-zero exit.

##### The shape of a reliable script
Put together, a robust job looks like: configure logging, do the work inside try/except, log both success and failure, and exit 0 only when it truly succeeded. Retries from earlier in this topic slot inside the try.

##### What this node introduces
    Log to a file so unattended runs leave a record.
    Catch errors, log them, then sys.exit non-zero so failures surface.
    Never swallow an error into a silent success.