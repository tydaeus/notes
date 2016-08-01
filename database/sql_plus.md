#Using SQL*PLus

## Connecting
Connect via `sqlplus username{/password}@(hostname.network|tnsname)`. You will be prompted for your password if you use the format without the password.

## Running Scripts

### `START` the Script
The `START` command retrieves a script and runs the contained commands. According to the documentation, the `.sql` extension can be omitted from the file name, but this is not always the case in practice.
```SQL
START file_name
```

#### Run a Script at Start

```sh
sqlplus username{/password}@(hostname.network|tnsname) @filename
```

### Using `EDIT`
The `EDIT` command within sqlplus will use the system default editor, or your defined editor:
```SQL
DEFINE _EDITOR = vim
```
