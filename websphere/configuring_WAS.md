#Configuring Websphere

###Java Environment Variables
Java environment variables are set at the time the JVM starts, and can be read via:
```java
System.getProperty("<PropertyName>");
```
or in Spring Expression Language(SpEL):
```java
@Value("#{systemProperties['propertyName']}")
String propertyValueHolder;
```

######Via Admin Console
1. Nav to `Servers > Server Types > WebSphere application servers`
2. Select the server being configured
3. Nav to `Server Infrastructure > Java and Process Management > Process definition`
4. Nav to `Additional Properties > Java Virtual Machine`
5. Nav to `Additional Properties > Custom properties`
6. Click the "New..." button
7. Specify the Name, Value, and optionally, Description of the variable.