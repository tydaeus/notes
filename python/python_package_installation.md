# Python Package Installation

## Pip
Pip is the default package manager installed with standard distributions of Python.

### Install Package
Install a package for the current user(e.g. Numpy):

```cmd
python -m pip install -U numpy --user
```

On Windows, user-installed package files will install within `%localappdata%\programs\python\pythonXX\libs\site-packages` (where `pythonXX`) is the installed Python version.

### Install Packages from File
Install from requirements.txt:

```cmd
python -m pip install -r requirements.txt
```

This appears to be managed atomically -- either everything gets installed or nothing does.

### Capture Install Environment
Capture the current package's install environment into requirements.txt (conventional name):

```cmd
python -m pip freeze > requirements.txt
```

