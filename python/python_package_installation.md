# Python Package Installation

## Pip
Pip is the default package manager installed with standard distributions of Python.

Install a package for the current user(e.g. Numpy):

```cmd
python -m pip install -U numpy --user
```

Capture the current package's install environment into requirements.txt (conventional name):

```cmd
python -m pip freeze > requirements.txt
```

Install from requirements.txt:

```cmd
python -m pip install -r requirements.txt
```