---
keyword: mathematical function
---

# Mathematical functions

This topic describes the mathematical functions supported by MaxCompute SQL.

## ABS

-   Syntax

    ```
    Double abs(Double number)
    Bigint abs(Bigint number)
    Decimal abs(Decimal number)
    ```

-   Description

    Calculates the absolute value of number.

-   Parameter

    number: If number is of the DOUBLE, BIGINT, or DECIMAL type, a value of the same type is returned.

    -   If number is of the BIGINT type, the return value must be of the BIGINT type.
    -   If number is of the DOUBLE type, the return value must be of the DOUBLE type.
    -   If number is of the DECIMAL type, the return value must be of the DECIMAL type.
    -   If number is of the STRING type, it is implicitly converted to a value of the DOUBLE type before calculation.
    -   If number is of a data type other than the preceding four types, an error is returned.
    **Note:** If number of the BIGINT type exceeds the upper limit, a value of the DOUBLE type is returned. However, the precision may be lost.

-   Return value

    The type of the return value depends on that of the input value, which can be DOUBLE, BIGINT, or DECIMAL. If the input value is NULL, NULL is returned.

-   Examples

    ```
    abs(null)=null
    abs(-1)=1
    abs(-1.2)=1.2
    abs("-2")=2.0
    abs(122320837456298376592387456923748)=1.2232083745629837e32
    ```

    The following example shows the usage of a complete ABS function in SQL statements. Other built-in functions, except window functions and aggregation functions, are used in a similar way.

    ```
    -- Calculate the absolute value of the id field in tbl1.
    select abs(id) from tbl1;
    ```


## ACOS

-   Syntax

    ```
    Double acos(Double number)
    Decimal acos(Decimal number)
    ```

-   Description

    Calculates the arccosine of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. The value ranges from -1 to 1. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is retuned. The value ranges from 0 to π. If the input value is NULL, NULL is returned.

-   Examples

    ```
    acos("0.87")=0.5155940062460905
    acos(0)=1.5707963267948966
    ```


## ASIN

-   Syntax

    ```
    Double asin(Double number)
    Decimal asin(Decimal number)
    ```

-   Description

    Calculates the arcsine of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. The value ranges from -1 to 1. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. The value ranges from -π/2 to π/2. If the input value is NULL, NULL is returned.

-   Examples

    ```
    asin(1)=1.5707963267948966
    asin(-1)=-1.5707963267948966
    ```


## ATAN

-   Syntax

    ```
    Double atan(Double number)
    ```

-   Description

    Calculates the arctangent of number.

-   Parameter

    number: a value of the DOUBLE type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE type is returned. The value ranges from -π/2 to π/2. If the input value is NULL, NULL is returned.

-   Examples

    ```
    atan(1)=0.7853981633974483
    atan(-1)=-0.7853981633974483
    ```


## CEIL

-   Syntax

    ```
    Bigint ceil(Double value)
    Bigint ceil(Decimal value)
    ```

-   Description

    Rounds up value and returns the nearest integer.

-   Parameter

    value: The value must be of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the BIGINT type is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    ceil(1.1)=2
    ceil(-1.1)=-1
    ```


## CONV

-   Syntax

    ```
    String conv(String input, Bigint from_base, Bigint to_base)
    ```

-   Description

    Converts a number from one number system to another.

-   Parameters
    -   input: the integer you want to convert, which is of the STRING type. Implicit conversions between the BIGINT and DOUBLE types are supported.
    -   from\_base and to\_base: decimal numbers. The values can be 2, 8, 10, or 16. Implicit conversions between the STRING and DOUBLE types are supported.
-   Return value

    A value of the STRING type is returned. If any input value is NULL, NULL is returned. The conversion process runs at 64-bit precision. An error is returned if an overflow occurs. If the input value is a negative value that begins with an en dash \(-\), an error is returned. If the input value is a decimal, it is converted to an integer before the conversion of number systems. The decimal part is left out.

-   Examples

    ```
    conv('1100', 2, 10)='12'
    conv('1100', 2, 16)='c'
    conv('ab', 16, 10)='171'
    conv('ab', 16, 16)='ab'
    ```


## COS

-   Syntax

    ```
    Double cos(Double number)
    Decimal cos(Decimal number)
    ```

-   Description

    Calculates the cosine of number, which is a radian value.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    cos(3.1415926/2)=2.6794896585028633e-8
    cos(3.1415926)=-0.9999999999999986
    ```


## COSH

-   Syntax

    ```
    Double cosh(Double number)
    Decimal cosh(Decimal number)
    ```

-   Description

    Calculates the hyperbolic cosine of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## COT

-   Syntax

    ```
    Double cot(Double number)
    Decimal cot(Decimal number)
    ```

-   Description

    Calculates the cotangent of number, which is a radian value.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## EXP

-   Syntax

    ```
    Double exp(Double number)
    Decimal exp(Decimal number)
    ```

-   Description

    Calculates the exponential value of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    The exponential value of number is returned. A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## FLOOR

-   Syntax

    ```
    Bigint floor(Double number)
    Bigint floor(Decimal number)
    ```

-   Description

    Rounds down number and returns the nearest integer.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the BIGINT type is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    floor(1.2)=1
    floor(1.9)=1
    floor(0.1)=0
    floor(-1.2)=-2
    floor(-0.1)=-1
    floor(0.0)=0
    floor(-0.0)=0
    ```


## LN

-   Syntax

    ```
    Double ln(Double number)
    Decimal ln(Decimal number)
    ```

-   Description

    Returns the natural logarithm of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type.

    -   If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
    -   If the input value is NULL, NULL is returned. If the input value is negative or 0, an error is returned.
-   Return value

    A value of the DOUBLE or DECIMAL type is returned.


## LOG

-   Syntax

    ```
    Double log(Double base, Double x)
    Decimal log(Decimal base, Decimal x)
    ```

-   Description

    Returns the logarithm of x whose base number is base.

-   Parameters
    -   base: the base value, which is of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
    -   x: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
-   Return value

    The logarithm value of the DOUBLE or DECIMAL type is returned.

    -   If any input value is NULL, NULL is returned.
    -   If any input value is negative or 0, an error is returned.
    -   If the value of base is 1, which causes division by 0, an error is returned.

## POW

-   Syntax

    ```
    Double pow(Double x, Double y)
    Decimal pow(Decimal x, Decimal y)
    ```

-   Description

    Returns the yth power of x, that is, `x^y`.

-   Parameters
    -   x: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
    -   y: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If any input value is NULL, NULL is returned.


## RAND

-   Syntax

    ```
    Double rand(Bigint seed)
    ```

-   Description

    Returns a random number of the DOUBLE type with a random number specified by seed. The value ranges from 0 to 1.

-   Parameter

    seed: a random number of the BIGINT type. This parameter is optional and specifies the start number of a random number sequence.

-   Return value

    A value of the DOUBLE type is returned.

-   Examples

    ```
    select rand() from dual;
    select rand(1) from dual;
    ```


## ROUND

-   Syntax

    ```
    Double round(Double number, [Bigint Decimal_places])
    Decimal round(Decimal number, [Bigint Decimal_places])
    ```

-   Description

    Returns a number rounded to the specified decimal place.

-   Parameters
    -   number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
    -   Decimal\_places: the decimal place, which is a constant of the BIGINT type. The value is rounded to the decimal point. If it is of another type, an error is returned. If this parameter is not specified, the number is rounded to the ones place. The default value is 0.

        **Note:** Decimal\_places can be a negative value. The negative value is counted from the decimal point to the left and the decimal part is excluded. If Decimal\_places exceeds the length of the integer part, 0 is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If any input parameter is NULL, NULL is returned.

-   Examples

    ```
    round(125.315)=125.0
    round(125.315, 0)=125.0
    round(125.315, 1)=125.3
    round(125.315, 2)=125.32
    round(125.315, 3)=125.315
    round(-125.315, 2)=-125.32
    round(123.345, -2)=100.0
    round(null)=null
    round(123.345, 4)=123.345
    round(123.345, -4)=0.0
    ```


## SIN

-   Syntax

    ```
    Double sin(Double number)
    Decimal sin(Decimal number)
    ```

-   Description

    Calculates the sine of number, which is a radian value.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## SINH

-   Syntax

    ```
    Double sinh(Double number)
    Decimal sinh(Decimal number)
    ```

-   Description

    Calculates the hyperbolic sine of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## SQRT

-   Syntax

    ```
    Double sqrt(Double number)
    Decimal sqrt(Decimal number)
    ```

-   Description

    Calculates the square root of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. It must be greater than 0. If it is less than 0, an error is returned. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## TAN

-   Description

    ```
    Double tan(Double number)
    Decimal tan(Decimal number)
    ```

-   Description

    Calculates the tangent of number, which is a radian value.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## TANH

-   Syntax

    ```
    Double tanh(Double number)
    Decimal tanh(Decimal number)
    ```

-   Description

    Calculates the hyperbolic tangent of number.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If the input value is NULL, NULL is returned.


## TRUNC

-   Syntax

    ```
    Double trunc(Double number[, Bigint Decimal_places])
    Decimal trunc(Decimal number[, Bigint Decimal_places])
    ```

-   Description

    Truncates the input value of number and right-fills it with zeros from the specified position.

-   Parameters
    -   number: a value of the DOUBLE or DECIMAL type. If the input value is of the STRING or BIGINT type, it is implicitly converted to a value of the DOUBLE type before calculation. If the input value is of another data type, an error is returned.
    -   Decimal\_places: the decimal place, which is a constant of the BIGINT type. This parameter indicates the position where the number is truncated. If it is of another data type, it is converted to the BIGINT type. If this parameter is not specified, the number is truncated to the ones place.

        **Note:** Decimal\_places can be a negative value, which indicates that the number is truncated from the decimal point to the left and the decimal part is left out. If Decimal\_places exceeds the length of the integer part, 0 is returned.

-   Return value

    A value of the DOUBLE or DECIMAL type is returned. If any input value is NULL, NULL is returned.

    **Note:**

    -   If a value of the DOUBLE type is returned, the return value may not be displayed properly. This issue exists in all systems. For more information, see `trunc(125.815,1)` in this example.
    -   The number is filled with zeros from the specified position.
-   Examples

    ```
    trunc(125.815)=125.0
    trunc(125.815,0)=125.0
    trunc(125.815,1)=125.80000000000001
    trunc(125.815,2)=125.81
    trunc(125.815,3)=125.815
    trunc(-125.815,2)=-125.81
    trunc(125.815,-1)=120.0
    trunc(125.815,-2)=100.0
    trunc(125.815,-3)=0.0
    trunc(123.345,4)=123.345
    trunc(123.345,-4)=0.0
    ```


## Additional functions of MaxCompute V2.0

If a new data type, such as TINYINT, SMALLINT, INT, FLOAT, VARCHAR, TIMESTAMP, or BINARY, needs to be used in a MaxCompute V2.0 SQL statement, you must insert a SET command before the SQL statement to enable the new data type.

-   Session level: To use a new data type, you must insert `set odps.sql.type.system.odps2=true;` before the SQL statement, and commit and execute them together.
-   Project level: The project owner can set the project as needed. It takes 10 to 15 minutes for the settings to take effect. Run the following command:

    ```
    setproject odps.sql.type.system.odps2=true;
    ```

    For more information about `setproject`, see [Project operations](/intl.en-US/Development/Common commands/Project operations.md). For the precautions you must take when you enable data types at the project level, see [Date types](/intl.en-US/Development/Data types/Data type editions.md).


## LOG2

-   Syntax

    ```
    Double log2(Double number)
    Double log2(Decimal number)
    ```

-   Description

    Calculates the logarithm of number with the base number of 2.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type.

-   Return value

    A value of the DOUBLE type is returned. If the input value is 0 or NULL, NULL is returned.

-   Examples

    ```
    log2(null)=null
    log2(0)=null
    log2(8)=3.0
    ```


## LOG10

-   Syntax

    ```
    Double log10(Double number)
    Double log10(Decimal number)
    ```

-   Description

    Calculates the logarithm of number with the base number of 10.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type.

-   Return value

    A value of the DOUBLE type is returned. If the input value is 0 or NULL, NULL is returned.

-   Examples

    ```
    log10(null)=null
    log10(0)=null
    log10(8)=0.9030899869919435
    ```


## BIN

-   Syntax

    ```
    String bin(Bigint number)
    ```

-   Description

    Calculates the binary code of number.

-   Parameter

    number: a value of the BIGINT type.

-   Return value

    A value of the STRING type is returned. If the input value is 0, 0 is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    bin(0)='0'
    bin(null)='null'
    bin(12)='1100'
    ```


## HEX

-   Syntax

    ```
    String hex(Bigint number) 
    String hex(String number)
    String hex(BINARY number)
    ```

-   Description

    Converts an integer or a string into a hexadecimal number.

-   Parameter

    number: If number is of the BIGINT type, a hexadecimal number is returned. If number is of the STRING type, a string in hexadecimal format is returned.

-   Return value
    -   If the input value is not 0 or NULL, a value of the STRING type is returned.
    -   If the input value is 0, 0 is returned.
    -   If the input value is NULL, an error is returned.
-   Examples

    ```
    hex(0)=0
    hex('abc')='616263'
    hex(17)='11'
    hex('17')='3137'
    hex(null): indicates that an exception occurs and the execution failed.
    ```


## UNHEX

-   Syntax

    ```
    BINARY unhex(String number)
    ```

-   Description

    Converts a hexadecimal string into a string.

-   Parameter

    number: the hexadecimal string.

-   Return value

    A value of the BINARY type is returned. If the input is 0, an error is returned. If the input is NULL, NULL is returned.

-   Examples

    ```
    unhex('616263')='abc'
    unhex(616263)='abc'
    ```


## RADIANS

-   Syntax

    ```
    Double radians(Double number)
    ```

-   Description

    Converts an angle to a radian value.

-   Parameter

    number: a value of the DOUBLE type.

-   Return value

    A value of the DOUBLE type is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    radians(90)=1.5707963267948966
    radians(0)=0.0
    radians(null)=null
    ```


## DEGREES

-   Syntax

    ```
    Double degrees(Double number) 
    Double degrees(Decimal number)
    ```

-   Description

    Converts a radian value to an angle.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type.

-   Return value

    A value of the DOUBLE type is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    degrees(1.5707963267948966)=90.0
    degrees(0)=0.0
    degrees(null)=null
    ```


## SIGN

-   Syntax

    ```
    Double sign(Double number)
    Double sign(Decimal number)
    ```

-   Description

    Obtains the sign of an input parameter.

-   Parameter

    number: a value of the DOUBLE or DECIMAL type.

-   Return value

    A value of the DOUBLE type is returned.

    -   If the input value is a positive value, 1.0 is returned.
    -   If the input value is a negative value, -1.0 is returned.
    -   If the input value is 0, 0.0 is returned.
    -   If the input value is NULL, NULL is returned.
-   Examples

    ```
    sign(-2.5)=-1.0
    sign(2.5)=1.0
    sign(0)=0.0
    sign(null)=null
    ```


## E

-   Syntax

    ```
    Double e()
    ```

-   Description

    Calculates the value of `e`.

-   Return value

    A value of the DOUBLE type is returned.

-   Example

    ```
    e()=2.718281828459045
    ```


## PI

-   Syntax

    ```
    Double pi()
    ```

-   Description

    Calculates the value of π.

-   Return value

    A value of the DOUBLE type is returned.

-   Example

    ```
    pi()=3.141592653589793
    ```


## FACTORIAL

-   Syntax

    ```
    Bigint factorial(Int number)
    ```

-   Description

    Calculates the factorial of number.

-   Parameter

    number: a value of the INT type. The value ranges from 0 to 20.

-   Return value

    A value of the BIGINT type is returned. If the input value is 0, 1 is returned. If the input value is NULL or a value that does not fall into the range from 0 to 20, NULL is returned.

-   Example

    ```
    factorial(5)=120 --5! =5*4*3*2*1=120
    ```


## CBRT

-   Syntax

    ```
    Double cbrt(Double number)
    ```

-   Description

    Calculates the cube root of number.

-   Parameter

    number: a value of the DOUBLE type.

-   Return value

    A value of the DOUBLE type is returned. If the input value is NULL, NULL is returned.

-   Examples

    ```
    cbrt(8)=2
    cbrt(null)=null
    ```


## SHIFTLEFT

-   Syntax

    ```
    Int shiftleft(Tinyint|Smallint|Int number1, Int number2)
    Bigint shiftleft(Bigint number1, Int number2)
    ```

-   Description

    Shifts a value left by a specific number of places \(<<\).

-   Parameters
    -   number1: an integer of the TINYINT, SMALLINT, INT, or BIGINT type.
    -   number2: an integer of the INT type.
-   Return value

    A value of the INT or BIGINT type is returned.

-   Examples

    ```
    shiftleft(1,2)=4  //Shifts the binary value of 1 two places to the left (1<<2, 0001 shifted to be 0100).
    shiftleft(4,3)=32  //Shifts the binary value of 4 three places to the left (4<<3, 0100 shifted to be 100000).
    ```


## SHIFTRIGHT

-   Syntax

    ```
    Int shiftright(Tinyint|Smallint|Int number1, Int number2)
    Bigint shiftright(Bigint number1, Int number2)
    ```

-   Description

    Shifts a value right by a specific number of places \(\>\>\).

-   Parameters
    -   number1: an integer of the TINYINT, SMALLINT, INT, or BIGINT type.
    -   number2: an integer of the INT type.
-   Return value

    A value of the INT or BIGINT type is returned.

-   Examples

    ```
    shiftright(4,2)=1 //Shifts the binary value of 4 two places to the right (4>>2, 0100 shifted to be 0001).
    shiftright(32,3)=4 //Shifts the binary value of 32 three places to the right (32>>3, 100000 shifted to be 0100).
    ```


## SHIFTRIGHTUNSIGNED

-   Syntax

    ```
    Int shiftrightunsigned(Tinyint|Smallint|Int number1, Int number2)
    Bigint shiftrightunsigned(Bigint number1, Int number2)
    ```

-   Description

    Shifts an unsigned value right by a specific number of places \(\>\>\>\).

-   Parameters
    -   number1: an integer of the TINYINT, SMALLINT, INT, or BIGINT type.
    -   number2: an integer of the INT type.
-   Return value

    A value of the INT or BIGINT type is returned.

-   Examples

    ```
    shiftrightunsigned(8,2)=2  //Shifts the binary unsigned value of 8 two places to the right (8>>>2, 1000 shifted to be 0010).
    shiftrightunsigned(-14,2)=1073741820  //Shifts the binary value of -14 two places to the right (-14>>>2, 11111111 11111111 11111111 11110010 shifted to be 00111111 11111111 11111111 11111100).
    ```


## FORMAT\_NUMBER

-   Syntax

    ```
    string format_number(float|double|decimal expr1, expr2)
    ```

-   Description

    Converts a number to a string in a specified format.

-   Parameters
    -   expr1: a numeric expression to format.
    -   expr2: the number of decimal places. It can be of the INT type. If can also be expressed in the format of `#,###,###. ##`.
        -   If expr2 is greater than 0, the value is rounded to the specified place after the decimal point.
        -   If expr2 is equal to 0, the value has no decimal point or fractional part.
        -   If expr2 is less than 0 or greater than 340, an error is returned.
-   Return value

    A value of the STRING type is returned.

-   Examples

    ```
    select format_number(5.230134523424545456,3);-- The return value is 5.230.
    select format_number(12332.123456, '#,###,###,###. ###');//The return value is 12,332.123.
    ```


