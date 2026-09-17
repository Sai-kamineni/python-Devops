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

##9.if, elif, and else
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
That reads better than if len(servers) == 0, and it does the same job.
```

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
