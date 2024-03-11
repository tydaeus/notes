# Console Apps in C#

## Write to Console
* `Console.Write(string)`
* `Console.WriteLine(string)`

## Pause Before Exit

To achieve the equivalent of a cmd script `pause` statement, use

```C#
static void pause()
{
    Console.WriteLine("Press any key to continue...");
    Console.ReadKey();
}
```