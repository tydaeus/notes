# Python Logging
Python's `logging` module provides standard logging functionality.

## Log Levels / Functions

These are the logging levels, in increasing order of severity. The `logging` module and created `Logger` objects provide convenience functions for logging at each of these levels, and a `log` function that requires a log level constant as its first argument.

1. `debug`
2. `info`
3. `warning`
4. `error`
5. `critical`

## Logging via `logging` Module

The `logging` module provides a 'root' logger allowing for rapid setup and configuration. Any of the logging functions can be called on `logging` to use this logger.

### `basicConfig`
`logging.basicConfig` configures the 'root' logger, thereby modifying both any direct uses through `logging` or indirect uses through `Logger` instances.

Use the `basicConfig` function before using this default logger if you wish to configure it, e.g: `logging.basicConfig(filename='myapp.log', level=logging.INFO, format='%(asctime)s %(levelname)s:%(message)s')`.

``` Python
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s.%(msecs)03d %(name)s [%(levelname)s]: %(message)s ',
    datefmt='%H:%M:%S'
)
```

Keyword arguments (fully documented at https://docs.python.org/3/library/logging.html#logging.basicConfig):
* `filename`
* `level` - loglevel constant from `logging` to determine minimum severity at which messages get output
* `format` - format string used to format the output messages; available attributes are documented at https://docs.python.org/3/library/logging.html#logrecord-attributes; common attributes:
    - `asctime` - datetime stamp
    - `name` - name of the logger
    - `levelname` - logging level the message was recorded at
    - `message` - the message content
    - `msecs` - milliseconds portion of timestamp
* `datefmt` - how to format the date in messages (if `format` includes the date via `asctime`)

## Logging via `Logger` Object

Create a `Logger` object via `logging.getLogger(loggerName)` to allow for having multiple differentiated logs. Most typically `logger = logging.getLogger(__name__)` will be used, so that each logger is named after the module that uses it, and automatically captures the package hierarchy it is created within.

**Important**: always use the `logging.getLogger()` factory function to generate loggers, not the `Logger` constructor.

Loggers are organized hierarchically based on their names, where levels are separated by `.`. E.g. given logger `base` and logger `base.child`, `base.child` is a child of `base`. Messages are passed up the hierarchy unless handled at a lower level. As a result, the configuration set by `basicConfig` will be used by logger objects unless alternate configuration is set for the lower level loggers.

### Logger Configuration Methods
* `logger.setLevel(loggingLevelConstant)` - sets minimum severity to be logged
* `logger.addHandler(handler)` and `logger.removeHandler(handler)` allow for adding and removing logging handlers
* `logger.addFilter(filter)` and `logger.removeFilter(filter)` allow for adding and removing logging filters


### Logger Usage Methods
* `logger.debug`, `info`, `warning`, `error`, and `critical` log at the specified level and expect a format string followed by the arguments to insert. Specifying keyword argument `exc_info` is used to determine whether to include exception information.
* `logger.exception` logs at error level with a stack trace dump; use only when catching exceptions



## Sources
* https://docs.python.org/3/howto/logging.html