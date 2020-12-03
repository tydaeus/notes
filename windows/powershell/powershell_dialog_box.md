# Displaying Dialog Boxes from PowerShell

## via Windows Script Host
From https://blogs.technet.microsoft.com/heyscriptingguy/2014/04/04/powertip-use-powershell-to-display-pop-up-window/, API documented in https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/windows-scripting/x83z1d9f(v=vs.84).

```
# 0 causes box to display until an option is clicked
$nSecondsToWait = 0

# Only option is "Ok"
$dialogType = 0x0

$wshell = New-Object -ComObject Wscript.Shell
$buttonClicked = $wshell.Popup("Body Text",$nSecondsToWait,"Title",$dialogType)
```

Dialog types:

* `0x0` - Show OK button.
* `0x1` - Show OK and Cancel buttons.
* `0x2` - Show Abort, Retry, and Ignore buttons.
* `0x3` - Show Yes , No, and Cancel buttons.
* `0x4` - Show Yes and No  buttons.
* `0x5` - Show Retry and Cancel buttons.
* `0x6` - Show Cancel, Try Again, and Continue buttons.

Return values:

* Dialog Timeout - -1
* Ok - 1
* Cancel - 2
* Abort - 3
* Retry - 4
* Ignore - 5
* Yes - 6
* No - 7
* Try Again - 10
* Continue - 11