# Displaying Dialog Boxes from PowerShell

## via `System.Windows.MessageBox`
From https://mcpmag.com/articles/2016/06/09/display-gui-message-boxes-in-powershell.aspx, API documented in https://docs.microsoft.com/en-us/dotnet/api/system.windows.messagebox?view=net-5.0.
System.Windows.MessageBox provides a relatively easy way to display standardized message dialogs.

``` PowerShell
# Prereq: add the PresentationFramework assembly to the PowerShell session
Add-Type -AssemblyName PresentationFramework

# Display a simple message requiring "Ok" to be clicked to continue.
[System.Windows.MessageBox]::Show('Something happened.')

# Display a message with title, the "Yes", "No", and "Cancel" buttons, and the error icon
[System.Windows.MessageBox]::Show('Are you sure you want to do this?','Decide','YesNoCancel','Error')
```

The most robust invocation for typical PowerShell invocation would be `Show([string]Message, [string]Title, [System.Windows.MessageBoxButton]ButtonSet, [System.Windows.MessageBoxImage]Icon)`, with the return value in the form of a `[System.Windows.MessageBoxResult]`.

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



## Build a Custom Dialog by Drawing a Windows Form
From https://community.idera.com/database-tools/powershell/powertips/b/tips/posts/creating-simple-custom-dialog; similar example at https://docs.microsoft.com/en-us/powershell/scripting/samples/creating-a-custom-input-box; more robust usage at https://smsagent.blog/2017/08/24/a-customisable-wpf-messagebox-for-powershell/. 

``` PowerShell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form1 = New-Object System.Windows.Forms.Form
$button2 = New-Object System.Windows.Forms.Button
$button1 = New-Object System.Windows.Forms.Button

$System_Drawing_Size = New-Object System.Drawing.Size
$System_Drawing_Size.Width = 256
$System_Drawing_Size.Height = 44
$form1.ClientSize = $System_Drawing_Size
$form1.FormBorderStyle = 3
$form1.TopMost = $True
$form1.Text = "Your Choice"
$form1.ControlBox = $False
$form1.StartPosition = 1

$System_Drawing_Size = New-Object System.Drawing.Size
$System_Drawing_Size.Width = 75
$System_Drawing_Size.Height = 23
$button2.Size = $System_Drawing_Size
$button2.UseVisualStyleBackColor = $True
$button2.Text = "Don''t do it"
$System_Drawing_Point = New-Object System.Drawing.Point
$System_Drawing_Point.X = 174
$System_Drawing_Point.Y = 12
$button2.Location = $System_Drawing_Point
$button2.DialogResult = 2

$System_Drawing_Size = New-Object System.Drawing.Size
$System_Drawing_Size.Width = 75
$System_Drawing_Size.Height = 23
$button1.Size = $System_Drawing_Size
$button1.UseVisualStyleBackColor = $True
$button1.Text = "Do it"

$System_Drawing_Point = New-Object System.Drawing.Point
$System_Drawing_Point.X = 92
$System_Drawing_Point.Y = 12
$button1.Location = $System_Drawing_Point
$button1.DataBindings.DefaultDataSourceUpdateMode = 0
$button1.DialogResult = 1

$form1.Controls.Add($button2)
$form1.Controls.Add($button1)

$form1.ShowDialog()
```