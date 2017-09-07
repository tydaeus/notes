# System Commands
Here are some of the cmd-accessible system commands that can help with construction batch scripts.

## Taskkill
Taskkill allows for quitting other processes. Most usefully:

```cmd
taskkill /F /FI "IMAGENAME eq imagename.exe"
```

Allows for force quitting any currently running processes whose name exactly match imagename.exe; does not produce errors if no such processes are currently running.