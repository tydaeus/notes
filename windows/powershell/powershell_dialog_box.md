## Display a Dialog Box

### via Windows Script Host
From https://blogs.technet.microsoft.com/heyscriptingguy/2014/04/04/powertip-use-powershell-to-display-pop-up-window/, API documented in https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/windows-scripting/x83z1d9f(v=vs.84).

```
# 0 causes box to display until an option is clicked
$nSecondsToWait = 0

# Only option is "Ok"
$dialogType = 0x0

$wshell = New-Object -ComObject Wscript.Shell
$buttonClicked = $wshell.Popup("Body Text",$nSecondsToWait,"Title",$dialogType)
```
