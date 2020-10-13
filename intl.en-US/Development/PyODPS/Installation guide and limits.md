---
keyword: install PyODPS
---

# Installation guide and limits

PyODPS is MaxCompute SDK for Python. It provides the DataFrame framework and basic operations on MaxCompute objects for you to analyze data in MaxCompute by using Python. This topic describes how to install PyODPS and its limits.

## Prerequisites

The following requirements are met before you install PyODPS:

-   The setuptools version is 3.0 or later.
-   The Requests version is 2.4.0 or later.

## Procedure

1.  Install pip. We recommend that you use pip, a Python package manager, to install PyODPS. For more information, see [Installation](https://pip.pypa.io/en/stable/installing/). The following installation commands are for your reference:

    ```
    pip install setuptools>=3.0
    pip install requests>=2.4.0
    pip install greenlet>=0.4.10  # Optional. It accelerates Tunnel-based data upload.
    pip install cython>=0.19.0  # Optional. We recommend that you do not install Cython if you use a Windows operating system.
    ```

    **Note:**

    -   We recommend that you use Alibaba Cloud images to accelerate data download. For more information about Alibaba Cloud images, see [Index of /pypi/](https://mirrors.aliyun.com/pypi/).
    -   If you use a Windows operating system, make sure that you have installed Visual C++ and Cython of correct versions. Otherwise, you cannot accelerate Tunnel-based data upload. For more information about the versions of Visual C++ and Cython, see [WindowsCompilers](https://wiki.python.org/moin/WindowsCompilers).
2.  Run the following command to install PyODPS:

    ```
    pip install pyodps
    ```

3.  Run the following command to check whether the installation is successful:

    ```
    python -c "from odps import ODPS"
    ```

4.  If the Python version is not the default, you can run the following command to switch to the default version after you have installed pip:

    ```
    /home/tops/bin/python2.7 -m pip install setuptools>=3.0
    ```


## Limits

-   MaxCompute SQL statements have limits. For more information, see [MaxCompute SQL limits](/intl.en-US/Development/SQL/MaxCompute SQL limits.md).
-   PyODPS nodes in DataWorks have limits. For more information, see [Use PyODPS in DataWorks](/intl.en-US/Development/PyODPS/Platform instructions/Use PyODPS in DataWorks.md).
-   Restricted by the sandbox, some programs that are debugged locally by using the pandas computing backend cannot be debugged in MaxCompute.

## What to do next

We recommend that you install the following tools to accelerate Tunnel-based data upload:

-   Greenlet 0.4.10 or later
-   Cython 0.19.0 or later

