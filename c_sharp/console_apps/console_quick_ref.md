# Console Apps in C#

## Pause Before Exit

To achieve the equivalent of a cmd script `pause` statement, use

```C#
static void pause()
{
    Console.WriteLine("Press any key to continue...");
    Console.ReadKey();
}
```