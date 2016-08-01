# Working with Grunt: Filesets
Grunt provides a standardized infrastracture for configuring which files to work with for multitasks. Grunt also uses node-glob and minimatch to further enrich fileset processing.

## Grunt Options
* `filter` Either a valid fs.Stats method name or a function that is passed the matched src filepath and returns true or false. See examples
* `nonull` If set to true then the operation will include non-matching patterns. Combined with grunt's --verbose flag, this option can help debug file path issues.
* `dot` Allow patterns to match filenames starting with a period, even if the pattern does not explicitly have a period in that spot.
* `matchBase` If set, patterns without slashes will be matched against the basename of the path if it contains slashes. For example, a?b would match the path /xyz/123/acb, but not /xyz/acb/123.
* `expand` Process a dynamic src-dest file mapping, see "Building the files object dynamically" for more information.
* Other properties will be passed to underlying libraries node-glob and minimatch docs.

## Source and Destination specification

### Compact Format
Allows a single src-dest mapping per target, additional properties per target
```js
    bar: {
      src: ['src/bb.js', 'src/bbb.js'],
      dest: 'dest/b.js',
      filter: "isFile",
      nonull: true
    },
```

### Files Object Format
Allows multiple mappings per target, property name is destination and value is src. No additional properties per target.
```js
    bar: {
      files: {
        'dest/b.js': ['src/bb.js', 'src/bbb.js'],
        'dest/b1.js': ['src/bb1.js', 'src/bbb1.js'],
      },
    },
```

### Files Array Format
Supports multiple mappings, and allows additional properties per mapping.
```js
bar: {
      files: [
        {src: ['src/bb.js', 'src/bbb.js'], dest: 'dest/b/', nonull: true},
        {src: ['src/bb1.js', 'src/bbb1.js'], dest: 'dest/b1/', filter: 'isFile'},
      ],
    }
```

## Globbing
Globbing is provided by the node-glob and minimatch libraries
- `*` matches any number of characters, but not /
- `?` matches a single character, but not /
- `**` matches any number of characters, including /, as long as it's the only thing in a path part
- `{}` allows for a comma-separated list of "or" expressions
- `!` at the beginning of a pattern will negate the match