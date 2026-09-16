# Python

* python3 -- version prints the interpreter version.
* pip3 -- version prints the package installer version.
* which python3 -- shows which Python program runs.

# What a venv is
A virtual environment is a folder holding its own copy of Python and its own site-packages directory. Anything you pip install while it's active goes into that folder and nowhere else. Delete the folder and the environment is gone, with no trace left on the system.

```
python3 -m venv .venv   #Create
source .venv/bin/activate   #Activate
deactivate  #leaves it
```
```
* Run without activating
.venv/bin/pip install requests
.venv/bin/python script.py
````