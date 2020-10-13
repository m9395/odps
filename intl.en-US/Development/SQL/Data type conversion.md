---
keyword: [explicit conversion, implicit conversion]
---

# Data type conversion

MaxCompute SQL supports two modes of data type conversions: explicit conversion and implicit conversion.

## Explicit conversion

An explicit conversion uses the CAST function to convert the data type of a value. The following table lists explicit conversion rules supported by MaxCompute. For more information about CAST, see [Other functions](/intl.en-US/Development/SQL/Builtin functions/Other functions.md).

|From/To|BIGINT|DOUBLE|STRING|DATETIME|BOOLEAN|DECIMAL|FLOAT|
|:------|:-----|:-----|:-----|:-------|:------|:------|-----|
|BIGINT|N/A|Y|Y|N|Y|Y|Y|
|DOUBLE|Y|N/A|Y|N|Y|Y|Y|
|STRING|Y|Y|N/A|Y|Y|Y|Y|
|DATETIME|N|N|Y|N/A|N|N|N|
|BOOLEAN|Y|Y|Y|N|N/A|Y|Y|
|DECIMAL|Y|Y|Y|N|Y|N/A|Y|
|FLOAT|Y|Y|Y|N|Y|Y|N/A|

Y indicates that the data type conversion is supported. N indicates that the data type conversion is not supported. N/A indicates that the conversion is not required. An error is reported for an unsupported explicit conversion.

**Example**

```
select cast(user_id as DOUBLE) as new_id from user;
select cast('2015-10-01 00:00:00' as datetime) as new_date from user;
select cast(array(1,2,3) as array<STRING>);
select concat_ws(',', cast(array(1, 2) as array<STRING>));
```

**Usage notes and limits**

-   If the DOUBLE type is converted into the BIGINT type, digits after the decimal point are removed, for example, `cast(1.6 as BIGINT) = 1`.
-   If the STRING type that meets the format of the DOUBLE type is converted into the BIGINT type, the STRING type is first converted into the DOUBLE type and then to the BIGINT type. Therefore, digits after the decimal point are removed, for example, `cast("1.6" as BIGINT) = 1`.
-   If the STRING type that meets the format of the BIGINT type is converted into the DOUBLE type, one digit is retained after the decimal point, for example, `cast("1" as DOUBLE) = 1.0`.
-   The default format yyyy-mm-dd hh:mi:ss is used during the conversion that involves the DATETIME type.
-   Some data types cannot be explicitly converted, but can be converted by using SQL built-in functions. For example, you can use the to\_char function to convert the BOOLEAN type into the STRING type. For more information, see [TO\_CHAR](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md). You can also use the to\_date function to convert the STRING type into the DATETIME type. For more information, see [TO\_DATE](/intl.en-US/Development/SQL/Builtin functions/Date functions.md).
-   If the value of the DECIMAL type exceeds its value range, the `CAST STRING TO DECIMAL` operation may cause an error such as the overflow of the most significant bit or the removal of the least significant bit.
-   MaxCompute supports conversions between complex data types. Implicit conversions between complex data types can be implemented only if their subtypes support implicit conversions. Explicit conversions between complex data types can be implemented only if their subtypes support explicit conversions. For the conversion of the STRUCT type, field names can be inconsistent, but the number of fields must be consistent and the corresponding fields must support implicit or explicit conversions. Examples:
    -   `array<bigINT>` can be implicitly or explicitly converted into `array<STRING>`.
    -   `array<bigINT>` can be explicitly converted into `array<INT>` but cannot be implicitly converted.
    -   `array<bigINT>` cannot be implicitly or explicitly converted into `array<datetime>`.
    -   `struct<a:bigINT,b:INT>` can be implicitly converted into `struct<col1:STRING,col2:bigINT>`, but cannot be implicitly or explicitly converted into `struct<a:STRING>`.

## Implicit conversion and its scope

An implicit conversion is an automatic data type conversion performed by MaxCompute based on the context and predefined rules. The following table lists implicit conversion rules supported by MaxCompute.

|From/To|BOOLEAN|TINYINT|SMALLINT|INT|BIGINT|FLOAT|DOUBLE|DECIMAL|STRING|VARCHAR|TIMESTAMP|BINARY|
|:------|:------|:------|:-------|:--|:-----|:----|------|-------|:-----|:------|:--------|:-----|
|BOOLEAN|Y|N|N|N|N|N|N|N|N|N|N|N|
|TINYINT|N|Y|Y|Y|Y|Y|Y|Y|Y|Y|N|N|
|SMALLINT|N|N|Y|Y|Y|Y|Y|Y|Y|Y|N|N|
|INT|N|N|Y|Y|Y|Y|Y|Y|Y|Y|N|N/A|
|BIGINT|N|N|N|N|Y|Y|Y|Y|Y|Y|N|N|
|FLOAT|N|N|N|N|Y|Y|Y|Y|Y|Y|N|N/A|
|DOUBLE|N|N|N|N|N|N|Y|Y|Y|Y|N|N|
|DECIMAL|N|N|N|N|N|N|N|Y|Y|Y|N|N|
|STRING|N|N|N|N|N|N|Y|Y|Y|Y|N|N|
|VARCHAR|N|N|N|N|Y|Y|Y|Y|N|N|N/A|N/A|
|TIMESTAMP|N|N|N|N|N|N|N|N|Y|Y|Y|N|
|BINARY|N|N|N|N|N|N|N|N|N|N|N|Y|

Y indicates that the data type conversion is supported. N indicates that the data type conversion is not supported. N/A indicates that the conversion is not required. An unsupported implicit conversion causes an exception. If a conversion fails during execution, an exception occurs.

**Note:**

-   MaxCompute V2.0 introduces the methods to define constants of the DECIMAL and DATETIME types. For example, 100BD indicates the value 100 of the DECIMAL type. `2017-11-11 00:00:00` indicates a constant of the DATETIME type. Defining constants allows you to directly apply constants to the VALUES clause and VALUES table.
-   An implicit conversion is automatically performed by MaxCompute based on context. If the types do not match, we recommend that you use the CAST function to perform an explicit conversion.
-   Implicit conversion rules apply to specific scopes. In specific scenarios, only part of the rules take effect.

**Example**

```
select user_id+age+'12345',
           concat(user_name,user_id,age)
  from user;
```

Implicit conversions with different operators:

-   **Implicit conversion with relational operators**

    Relational operators include `=, <>, <, ≤, >, ≥, IS NULL, IS NOT NULL, LIKE, RLIKE, and IN`. The implicit conversion rules for `LIKE, RLIKE, and IN` are different from those for other relational operators. The rules described in this section do not apply to these three operators.

    The following table lists implicit conversion rules when different types of data are involved in relational operations.

    |From/To|BIGINT|DOUBLE|STRING|DATETIME|BOOLEAN|DECIMAL|
    |:------|:-----|:-----|:-----|:-------|:------|:------|
    |BIGINT|N/A|DOUBLE|DOUBLE|N|N|DECIMAL|
    |DOUBLE|DOUBLE|N/A|DOUBLE|N|N|DECIMAL|
    |STRING|DOUBLE|DOUBLE|N/A|DATETIME|N|DECIMAL|
    |DATETIME|N|N|DATETIME|N/A|N|N|
    |BOOLEAN|N|N|N|N|N/A|N|
    |DECIMAL|DECIMAL|DECIMAL|DECIMAL|N|N|N/A|

    **Note:**

    -   If the two values of different types that you want to compare do not support implicit conversions, the relational operation cannot be completed and an error is reported.
    -   For more information about relational operators, see [Operators](/intl.en-US/Development/SQL/Operators.md).
-   **Implicit conversion with special relational operators**

    Special relational operators are `LIKE, RLIKE, and IN`.

    -   Syntax of LIKE and RLIKE:

        ```
        source like pattern;  
        source rlike pattern;
        ```

        **Note:**

        -   The source and pattern parameters of LIKE and RLIKE must be of the STRING type.
        -   Other types can neither be involved in the operation nor be implicitly converted into the STRING type.
    -   Syntax of IN:

        ```
        key in (value1, value2, ...)
        ```

        **Note:**

        -   The data types in the value list next to IN must be consistent.
        -   If you compare data of keys and values among the BIGINT, DOUBLE, and STRING types, convert the data of the BIGINT and STRING types into the DOUBLE type. If you compare the data between the DATETIME and STRING types, convert the data of the STRING type into the DATETIME type. Conversions between other types are not allowed.
-   **Implicit conversion with arithmetic operators**

    Arithmetic operators include `+, -, *, /, %`. The following implicit conversion rules apply:

    -   Only the STRING, BIGINT, DOUBLE, and DECIMAL types can be used in arithmetic operations.
    -   Values of the STRING type are implicitly converted into the DOUBLE type before an operation.
    -   If values of the BIGINT and DOUBLE types are involved in an operation, the value of the BIGINT type is implicitly converted into the DOUBLE type.
    -   The DATETIME and BOOLEAN types cannot be used in arithmetic operations.
-   **Implicit conversion with logical operators**

    Logical operators include `AND, OR, and NOT`. The following implicit conversion rules apply:

    -   Only the BOOLEAN type can be used in logical operations.
    -   Other types cannot be used in logical operations or implicitly converted.

## Implicit conversion for built-in functions

MaxCompute SQL provides a variety of system functions, which can be used to calculate one or more columns of any row and provide any type of data. The following implicit conversion rules apply:

-   If the data type of an input parameter is different from that defined in the function during function calls, the data type of the input parameter is converted into the function-defined data type.
-   The parameters of each built-in function of MaxCompute SQL have different requirements for implicit conversions. For more information, see [Built-in functions](/intl.en-US/Development/SQL/Builtin functions/Mathematical functions.md).

## Implicit conversion with CASE WHEN

For more information about CASE WHEN, see [CASE WHEN expressions](/intl.en-US/Development/SQL/Builtin functions/Other functions.md). The following implicit conversion rules apply:

-   If the return values are only of the BIGINT and DOUBLE types, all the values are converted into the DOUBLE type.
-   If the return values include those of the STRING type, all the values are converted into the STRING type. If a conversion such as conversion from BOOLEAN to STRING fails, an error is reported.
-   Conversions between other types are not allowed.

## Conversion between the STRING and DATETIME types

MaxCompute supports conversions between the STRING and DATETIME types. The conversion format is `yyyy-mm-dd hh:mi:ss`.

|Time unit|String \(not case-sensitive\)|Value range|
|:--------|:----------------------------|:----------|
|Year|yyyy|0001-9999|
|Month|mm|01-12|
|Day|dd|01-28, 29, 30, 31|
|Hour|hh|00-23|
|Minute|mi|00-59|
|Second|ss|00-59|

**Note:**

-   If the first digit of the value range of each time unit is 0, 0 cannot be omitted. For example, `2014-1-9 12:12:12` is an invalid DATETIME format and it cannot be converted from the STRING type to the DATETIME type. It must be written as `2014-01-09 12:12:12`.
-   Only the STRING type that meets the preceding format requirements can be converted into the DATETIME type. For example, `cast("2013-12-31 02:34:34" as datetime)` is used to convert `2013-12-31 02:34:34` of the STRING type into the DATETIME type. Similarly, if the DATETIME type is converted into the STRING type, the default conversion format is `yyyy-mm-dd hh:mi:ss`.

Conversions triggered by the following operations will cause exceptions:

```
cast("2013/12/31 02/34/34" as datetime)  
cast("20131231023434" as datetime)  
cast("2013-12-31 2:34:34" as datetime)
```

The threshold of dd depends on the actual days of a month. If the value exceeds the actual days of the month, the conversion is aborted with an error.

```
cast("2013-02-29 12:12:12" as datetime)      -- An error is returned because February 29, 2013 does not exist. 
cast("2013-11-31 12:12:12" as datetime)      -- An error is returned because November 31, 2013 does not exist.
```

MaxCompute provides the TO\_DATE function to convert the STRING type that does not meet the DATETIME format requirements into the DATETIME type. For more information, see [TO\_DATE](/intl.en-US/Development/SQL/Builtin functions/Date functions.md).

