---
keyword: common error
---

# Common errors

This topic describes the common errors that you may encounter when you use MaxCompute.

## Error description

A MaxCompute error message is in the following format:

```
Error ID:General description - Context description
```

Example: `ODPS-0130131:Table not found - "myproject" "mytable"`. In this example, error ID `ODPS-0130131` corresponds to the general description `Table not found`. The context description provides error information for you to locate the error.

**Note:** An error code is a prompt to help you locate a problem. For more information about how to locate a problem, see [Logview](/intl.en-US/Development/View Job Running Information/Logview.md).

Format of an error ID:

```
ODPS-MMCCCCX
```

Format description:

-   MM: the module ID, which is a two-digit integer. For example, 00 indicates an exception in the common framework, and 01 indicates an SQL task exception.
-   CCCC: the error code, which is a four-digit integer.
-   X: the severity of an exception, which is a one-digit integer. 1 indicates a minor error, such as an invalid user input. 9 indicates an error of the highest severity, such as an atomicity error.

## Common errors

For more information about the common errors of MaxCompute SQL, MapReduce, and Tunnel tasks, see the following topics:

-   [Common errors of MaxCompute SQL tasks](/intl.en-US/Error Code Appendix/Common SQL errors.md)
-   [Common errors of MaxCompute MapReduce tasks](/intl.en-US/Error Code Appendix/MapReduce Common Errors.md)
-   [Common errors of MaxCompute Tunnel tasks](/intl.en-US/Error Code Appendix/Tunnel Common Errors.md)

