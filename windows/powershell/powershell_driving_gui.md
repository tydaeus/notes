# Driving GUI Processes with PowerShell
PowerShell can use .Net Windows API functions to allow it to interact with Windows GUI apps.

## Focusing on a Specified Window

``` PowerShell
# Import APIs needed to focus on a window
$user32ImportSig = '
    [DllImport("user32.dll")] public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow);
    [DllImport("user32.dll")] public static extern int SetForegroundWindow(IntPtr hwnd);
'
# Set the imported APIs to be accessible from `[Win32.NativeMethods]` handle
Add-Type -MemberDefinition $user32ImportSig -Name NativeMethods -NameSpace Win32


# Start the process that will open the window we're interested in
# FUTURE: example for how to retrieve a process that we didn't just start
# Must use '-PassThru' or else we won't get the process returned
$process = Start-Process $myExePath -PassThru
# Must use sleep to wait a moment for the process to start, otherwise we can't grab it
# FUTURE: example for how to wait until the process's window is available
Start-Sleep -Seconds 2


$windowHandle = $process.MainWindowHandle
# set the window to be visible at normal size
# 4 = normal size; see https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-showwindow for full list of consts
[Win32.NativeMethods]::ShowWindowAsync($windowHandle, 4) | Out-Null
# bring the window to the front
[Win32.NativeMethods]::SetForegroundWindow($windowHandle) | Out-Null
```


###### Sources
* https://community.idera.com/database-tools/powershell/powertips/b/tips/posts/bringing-window-in-the-foreground