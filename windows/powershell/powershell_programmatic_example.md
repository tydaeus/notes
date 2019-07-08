
Build a new MyClass object and populate its properties with the corresponding values of the containing function's (not shown) parameters, with environment variables prefixed with MyClassDefault taking precedent over parameter default values.

```PowerShell
$result = [MyClass]::new()

$parameterKeys = (Get-Command -Name $MyInvocation.InvocationName).Parameters.Keys

foreach ($key in $parameterKeys) {
    # use passed value if present
    if ($PSBoundParameters.ContainsKey($key)) {
        $result.$key = Get-Variable $key -ValueOnly
    }
    else {
        $envVar = Get-ChildItem "env:MyClassDefault$key" -ErrorAction 'Ignore'
        # no passed value, use environment variable as default if defined
        if ($envVar) {
            $result.$key = $envVar.Value
        }
        # no passed value, no environment variable, use default
        else {
            $value = Get-Variable $key -ValueOnly
            if ($value) {
                $result.$key = $value
            }
        }
    }
}
```


``` PowerShell
# Get a combined list of properties and note properties
$properties = $object | Get-Member | Where { ($_.MemberType -eq 'NoteProperty') -or ($_.MemberType -eq 'Property') }

# iterate through the property list
foreach ($property in $properties) {
    # get the property name and value
    $propertyName = $property.Name
    $propertyValue = $object.$propertyName

    # allow separate handling of null-valued properties
    if ($propertyValue -eq $Null) {
        #...
    }
    # check type of property (won't work on null valued properties)
    elseif ($propertyValue.GetType() -eq [Object[]]) {
        #...
    }
```
