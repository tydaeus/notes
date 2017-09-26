# cmd.exe Scripting

Misc notes about windows scripting with .cmd/.bat files.
Use https://ss64.com for a real reference; this is just a cheat sheet.

## Output
Use `echo` to output to the console.

Redirect stdout to stderr on echo to output to stderr:
```cmd
echo ERR: Something bad happened! 1>&2
```

## Comments
Comments are supported in batch scripting, but only poorly. **Never** use `%` or any control characters in a comment -- the parser will attempt to parse the comment as a command.

### The `REM` command
The `rem` command indicates a remark, to be ignored by the parser. However, this command does adversely impact readability, and has some performance issues due to the parser still reading the entire line.

### Labels as Comments
By convention, labels starting with an additional colon are also used as comments. Processing time for this can be significantly faster than for `rem` comments.

```cmd
:: this is used as a comment
```

Labels as comments cannot be used within parens (`(`, `)`) used for flow control; use `rem` in these cases.

### Plaintext as Comments
Include a goto that skips past a block, then use that block as a comment.

```cmd
goto :endOfComments
Here are some comments.

:endOfComments
```

## Verbosity

### `@` Operator
The `@` operator, prefixed to a command, results in this command not being displayed as it is executed

### `Echo` toggle
The statement `Echo off` will turn off command display until `Echo on` is invoked. This is customarily written as `@Echo off` to prevent the toggle operation from getting displayed. In general, it's best to begin scripts with `@ECHO off`, except during debugging.

## Escaping

### ^ escaping
The `^` (caret) escape character can be used to escape special characters, including the end of line (enabling multiline commands).

### "" escaping
`"` (double quotes) can be used within a value to preserve spaces, and must be used to wrap filenames that have spaces within them.

<h2 id="variables">Variables</h2>

Note that in cmd scripting, variables are handled somewhat differently from [parameters](#parameters).

### Accessing
Enclose a variable within `%` to retrieve its value, e.g. `%X%`.

### Setting
Set session variables via the `set` command.

```cmd
set x=foo
```

Note that everything to the right of the equal sign will get assigned to the variable, except for redirection characters.

```cmd
set x=foo bar baz.
echo %X%
>foo bar baz.
```

Redirection characters can get included in the variable by wrapping **both sides** of the assignment in quotes.

```cmd
set "x=foo & bar"
echo %X%
>foo
>'bar' is not recognized as an internal or external command,
>operable program or batch file.

echo "%X%"
>foo & bar
```

#### Setting a Var Based on Command Output
There is no cmd equivalent to the Linux backtick operator. However, you can use `for`'s command output processing option to 

```cmd
for /F "delims=" %%A in ('COMMAND') do set VARNAME=%%A
```

### Scope
Variables are global within their command prompt session by default. Within the command prompt, system level variables are initialized from the system environment at the start of execution.

#### setlocal
The `setlocal` script command initializes an inner scope. Variables set after `setlocal` and before `endlocal` will be localized between them. This is generally a good practice.

Scopes can be further nested via additional setlocal/endlocal pairings.

Running within a local scope has additional benefits, such as easily allowing directory changes internally without changing the current working directory for the user or other scripts.

Note that local scope is available only for scripts, not for commandline usage.

###### Delayed Expansion
Calling `setlocal enabledelayedexpansion` causes variables to be expanded at execution time instead of at parse time.

Wihout delayed expansion, variables get replaced by their values only once, *before* commands are executed.

Delayed expansion enables using `!` instead of `%` for specifying latest possible expansion.

##### exporting from local scope

Combine endlocal with variable set command to provide a variable to the outer scope.

Example cribbed shamelessly from ss64:
```cmd
ENDLOCAL & SET _return1=%_item%& SET _return2=%_price%
```

<h3 id="parameters">Parameters</h3>

Note that parameters in command scripting are processed differently from [variables](#variables).

Parameters passed on the command line, or passed along to a subroutine, are accessible on special variables `%1`-`%9`. `%*` refers to all arguments (up to argument 255), but only 1-9 are directly accessible by number.

Parameter `%0` contains the full path file name of the script/program being invoked

#### Parameter Extension
Parameters can get processed into alternate forms via the `~` operator with specific letter combination.

Common processing (using argument 1 as our sample var):

* `%~1` removes any surrounding quotes
* `%~d1` provides the drive letter only (e.g. `C:`)
* `%~p1` provides the path only, including trailing `\`
* `%~dp1` provides the drive letter and path. Use `%~dp0` for figuring out the path to the script currently running
* `%~f1` provides the fully qualified path name
* `%~$PATH:1` searches PATH and expands %1 to the fully qualified name of the first match

### Variable Replacement
Command scripting includes a built-in command to perform simple replacement operations on a variable.

```cmd
%variable:StrToFind=NewStr%
:: replaces instances of "StrToFind" with "NewStr"
```

Note that to replace a variable string or replace with a variable string, you will need to enableDelayedExpansion and use the `!` operator as well as the `%` operator.

## Script Completion

Use `exit /B` to exit the current script without exiting the command prompt. An exit code can be optionally specified.

## Running Other Scripts
Run another script by using `call [scriptname]` if you want your current script to pause until the other script completes. Use `start [scriptname]` if you don't want blocking.

## pushd and popd
`pushd` pushes the current directory onto a stack and sets the working directory to the specified directory. `popd` pops the last directory of the stack and uses it as current working directory.

This allows storing the current directory prior to changes, then returning to it.

`pushd` will also temporarily map the specified directory to an available drive letter if it is a network directory rather than a local drive. This is the main way to use a network dir as the current working directory. Note that some odd behaviors may occur.

## Error Checking
The `ERRORLEVEL` variable is automatically populated with the exit code of the last executed command. Due to its shared nature, be sure to check or store it immediatebly, before it gets overwritten.

By convention, ERRORLEVEL 0 indicates success, while any other value indicates an error occurred.

Use `IF %ERRORLEVEL% EQU 0` to check for success, `IF %ERRORLEVEL% NEQ 0` to check for failure.

Note that syntax `IF ERRORLEVEL n` checks for errorlevel >= n, to preserve compatibility with old (Windows 95) scripts. This should probably be avoid in modern scripts.