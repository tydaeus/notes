# Python Virutal Environments - venv
From https://www.infoworld.com/article/3239675/virtualenv-and-venv-python-virtual-environments-explained.html

Using virtual environments allows you to maintain multiple effectively different Python installations without their conflicting with each other -- e.g. to allow for a program to run in a pristine Python environment, or with only specific Pip packages, instead of all Python programs running from the same environment.

## Create Virtual Environment
``` bat
python -m venv <VENV DIR>
```

The virtual environment will be created in the directory specified by `VENV DIR`; this will include a Python interpreter, pip, setuptools, and any packages installed to that virtual environment. Bear in mind that each virtual environment maintains its own utilities and packages, separate from all others and the base python environment, so it will need to be maintained separately.

The `VENV DIR` should not contain any persistent custom code.

## Activate Virtual Environment
``` bat
<VENV DIR>\Scripts\activate.bat
```

This will activate the environment within the session (shell) the script was run within.

## Use Virtual Environment
Use pip to install desired packages within the virtual environment. Update pip via `python -m pip install -U pip`; `pip install -U pip` may not be able to update all files.

Run scripts within the environment via `python <SCRIPT>`.

## Deactivate Virtual Environment
Deactivate the virtual environment by running `deactivate` (`Scripts\deactivate.bat` for cmd prompt); this will return you to the normal shell environment.

## Remove Virtual Environment
Delete the folder containing the virtual environment to remove it.

## Update Virtual Environment
To update the python install within a virtual environment by anything less then a point release, run `python -m venv <VENV DIR> --upgrade` (do not activate the environment).

For point releases, you will need to delete the virtual environment and create a new one.
