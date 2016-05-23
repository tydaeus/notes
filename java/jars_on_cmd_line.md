# Manipulating JARs on the command line

This is a condensed version of the help from http://www.javaworld.com/article/2857714/learn-java/manipulating-jars-wars-and-ears-on-the-command-line.html

## The `jar` command

The jar command is used to create and manipulate jars. Some options are provided undashed, some dashed.

### Common options

* `c` - create archive (leaves original files intact)
* `f` - specify filename: allows the filename to be provided as part of the command rather than input or output
* `t` - list contents of archive
* `u` - update existing jar with specified file
* `v` - verbose
* `x` - extract contents (leaves original file intact)
* `-C` - change to the specified directory to change dir as part of creation process