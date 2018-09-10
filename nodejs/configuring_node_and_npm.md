# Configuring Node and NPM

## Configuring NPM

### Proxy

```bash
> npm config set proxy http://proxy.domain.com:8080
> npm config set https-proxy https://proxy.domain.com:8080
```

### Prefix
The prefix determines where global modules get installed. This should not be within the node program folder, so updating node will not overwrite modules. Prefix defaults to a location within the user's home folder.
```bash
    > npm config set prefix "C:\[location]"
```

**Recommended**: add this location to your `PATH` so that global modules can be used directly from the command line.
