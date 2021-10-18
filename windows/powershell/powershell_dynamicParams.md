# Dynamic Parameters

PowerShell supports defining function parameters dynamically at run-time via the `DynamicParam` block. 

## About `DynamicParam`
The `DynamicParam` block gets run before the `Begin` block (if present). The parameter definitions returned this block determine what dynamic parameters can be set when invoking the script (in current circumstances).

Operation of the `DynamicParam` block appears to get some special modification. Because its behavior can vary depending on the value of other parameters entered on the commandline as their entry occurs, it gets executed during commandline invocation and can potentially be executed multiple times. It appears that only the results and output of the final execution get kept, but external side effects can still occur (thus should be avoided), and any delays in processing the block result in commandline input blocking.

### Defining Dynamic Parameters in `DynamicParam`
Within the `DynamicParam` block, parameters must be defined as `System.Management.Automation.RuntimeDefinedParameter` objects and returned within a `System.Management.Automation.RuntimeDefinedParameterDictionary`, keyed to their name. This definition and return process effectively duplicates the functionality of the standard parameter declaration block to provide the dynamic parameters.

Any static parameters that have been set can be retrieved within the `DynamicParam` block.


``` PowerShell
DynamicParam {
    # 1. Parameter creation will typically be conditional based on system state or the value of static parameters
    # This example omits the condition

    # 2. Any parameter attributes must be created as `System.Attribute` objects for addition to the parameter, as 
    # appropriate PowerShell SDK objects

    # e.g. to create the equivalent of [Parameter(Mandatory, ValueFromPipeline)]:
    $parameterAttribute = [System.Management.Automation.ParameterAttribute]@{
        Mandatory = $True
        ValueFromPipeline = $True
    }

    # e.g. to create the equivalent of [AllowNull()]:
    $allowNullAttribute = [System.Management.Automation.AllowNullAttribute]::new()

    # 3. All parameter attributes must be added to a `[System.Collections.ObjectModel.Collection[System.Attribute]]`
    # to allow them to be added to the parameter definition.
    $attributeCollection = [System.Collections.ObjectModel.Collection[System.Attribute]]::new()
    $attributeCollection.Add($parameterAttribute)
    $attributeCollection.Add($allowNullAttribute)

    # 4. Create the new parameter as a `System.Management.Automation.RuntimeDefinedParameter`; in this example we use the parameterised version of the constructor (name, type, attributes).
    $dynParam1 = [System.Management.Automation.RuntimeDefinedParameter]::new(
        'dynParam1', [Int32], $attributeCollection
    )

    # 5. Create a `System.Management.Automation.RuntimeDefinedParameterDictionary` for holding the in-effect dynamic parameters
    $paramDictionary = [System.Management.Automation.RuntimeDefinedParameterDictionary]::new()

    # 6. Add all in-effect dynamic parameters to the RuntimeDefinedparameterDictionary, keyed to their name
    # Unknown: what is effect if same parameter is added with incorrect/multiple key? Undefined?
    $paramDictionary.Add('dynParam1', $dynParam1)

    # 7. Return the dictionary containing all in-effect dynamic parameters once they've been added
    return $paramDictionary
}

# Once we have a DynamicParam block, all other processing must occur in Begin, Process, or End block
Process {

    # 8. Access the dynamic params via `$PSBoundParameters`
    # Best practice: verify that param has been set as well as checking its value
    if ($PSBoundParameters.dynParam -and $PSBoundParameters.dynParam -gt 5) {
        #...
    }
}
```

## Issues with Dynamic Parameters
Because the `DynamicParam` block gets run while commandline entry is being performed, it can potentially impact user input performance.

Dynamic parameters can be more difficult to discover and less visible in Get-Help.

## Sources / See Also
* https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters?view=powershell-5.1#dynamic-parameters
* https://adamtheautomator.com/powershell-parameter-validation/
* https://powershellmagazine.com/2014/05/29/dynamic-parameters-in-powershell/