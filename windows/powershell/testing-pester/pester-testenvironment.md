# Pester Test Environment
Pester automatically creates and maintains some handy test environment settings.


## Pester TestDrive

Pester automatically creates and maintains a PSDrive called `TestDrive`. This can either be accessed through the `TestDrive:` provider, or its literal path on the hard drive can be referenced from the `$TestDrive` variable. The `$TestDrive` path will need to be used when working with any .NET methods or non-PowerShell commands.

**Note**: Use `TestDrive:\` as the base of `TestDrive` paths. Omitting the `\` works fine with some commands while failing with others.

TestDrive scoping is configured to match the nesting structure of the tests, so a file created within a given Describe or Context block will go out of scope and be deleted when that block ends. However, a file created in a parent block and then modified within a child block will retain those changes in other child blocks.

## Pester TestRegistry

Pester automatically creates and maintains a registry provider stored within HKCU, called `TestRegistry`, similar to the TestDrive.

Scoping is handled similar to TestDrive, with keys being deleted when the block they were created within exits.