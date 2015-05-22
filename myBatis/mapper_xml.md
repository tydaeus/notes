#MyBatis Mapper XML

###Creating Custom Actions
1. Add a new method to the mapper java interface
2. Add a new tag to the mapper .xml file to represent the new action being added, of type `select`, `insert`, `update`, or `delete`, with its `id` property set to the name of the newly added java method

###Referencing Parameters
Parameters passed to the java mapper interface method can be referenced within the mapper xml via `#{paramName}` if the parameter within the method is annotated `@Param("paramName")`. The `#{}` operator auto-generates PreparedStatements and sets values safely. To directly inject an unmodified value into the SQL, use the `${}` operator &ndash; this is *unsafe*.

A single parameter can be passed without the `@Param` annotation; this parameter is referenced as `_parameter`.

**Note:** Regardless of whether passing multiple or single parameter, it appears that auto-generated statements null-check with `<if test="_parameter != null" >`. This may be in error?

###Convenience Tags
These tags make it easier to write mapper xml

######typeAlias

Allows you to specify a short form for a java type. **Must be applied in config.xml** E.g:
```xml
<typeAlias type="com.someapp.model.User" alias="User" />
```

######sql
Allows you to define an sql snippet for re-use elsewhere in the mapper
```xml
<sql id="Base_Column_List" >
    INV_PARCEL_PK, MAILER_FK, ADDRESSEE_FK, BARCODE, DESCRIPTION, PARCEL_SEQ, PARCEL_LABEL, 
    INV_FORM_FK, INV_PARCEL_STATUS_FK, MAILING_DATE, EXPR_DATE, DEPARTMENT_FK, LOCATION_FK, 
    INV_PARCEL_PK_I, MAILER_FK_I, ADDRESSEE_FK_I, INV_FORM_FK_I, INV_PARCEL_STATUS_FK_I, 
    DEPARTMENT_FK_I, LOCATION_FK_I, DELETED, ORIGINAL_PK, USER_ID, MODIFIED, ENTRY_DATE
</sql>
```

######include
Includes a previously defined sql snippet
```xml
<include refid="Base_Column_List" />
```

