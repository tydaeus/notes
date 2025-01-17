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

## Update Package(s)

``` cmd
python -m pip install --upgrade <packagename>
```

Omit `<packagename>` to update all installed packages.

## Uninstall Package(s)

``` cmd
python -m pip uninstall <package1> [package2...]
```

## List Packages

``` cmd
python -m pip list
```

## List Available Package Versions
This is something pip hasn't been very good at historically.

Experimental and somewhat sensible command: `pip index versions <package>`.

Historically viable command: `pip install <package>==invalid`; this relies on attempting to install an invalid version of the package resulting in an error message listing valid versions.