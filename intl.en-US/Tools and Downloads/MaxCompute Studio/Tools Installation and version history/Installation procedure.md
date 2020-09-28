# Installation procedure

## Environment requirements

IntelliJ IDEA can be installed on *Windows*, *Mac*, *Linux*. For more information about the hardware and system environment requirements, click [Requirements for IntelliJ IDEA](https://www.jetbrains.com/help/idea/2016.3/requirements-for-intellij-idea.html) . IntelliJ IDEA-based MaxCompute Studio can also be installed on clients running these operating systems.

MaxCompute Studio has the following requirements on the your environment:

-   A client running Windows, macOS, or Linux.
-   IntelliJ IDEA 14.1.4 or a later version is installed. \(The Ultimate version, PyCharm version, and free [Community version](https://www.jetbrains.com/idea/download/) are supported.\)
-   JRE 1.8 is installed. \(JRE 1.8 has been bound to the latest IntelliJ IDEA.\)
-   JDK 1.8 is installed. \(*Optional*: JDK is required if you need to develop and debug Java UDF.\)

    **Note:** The client supports JDK 1.9 from the 0.28.0 version. The previous version only supports JDK 1.8.


## Installation method

MaxCompute Studio is a plugin of IntelliJ IDEA, which can be installed using either of the following two methods:

-   Online installation using the plugin library \(recommended\)
-   Installation using a local file

## Online installation \(recommended\)

MaxCompute Studio The MaxCompute Studio plugin has been opened for all users on the Internet. You can install MaxCompute Studio using the official IntelliJ IDEA plugin library.

**Procedure**

1.  Open the plugin configuration page on IntelliJ IDEA. \(If you are a Windows/Linux user, choose **File** \> **Settings** \> **\> Plugins.** If you are a macOS user,**choose IntelliJ IDEA** \> **Preferences** \> **Plugins** \).
2.  Click **Browse repositories…** and search for `MaxCompute Studio`.

3.  On the MaxCompute Studio plugin page, click **Install**.

4.  After the installation is confirmed, restart IntelliJ IDEA to complete installation.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8119938751/p1555.png)


## Local installation

MaxCompute Studio MaxCompute Studio can also be installed in a local environment.

**Procedure**

1.  Go to the [MaxCompute Studio plugin page](https://plugins.jetbrains.com/plugin/9193?spm=5176.doc44555.2.1.4hXBG1) to download the plugin package.

2.  Run IntelliJ IDEA.

    -   If you access IntelliJ IDEA for the first time, a welcome page is displayed. Click **Configure** and select **Plugins** from the shortcut menu, as shown in the following figure.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8119938751/p1556.png)

    -   If you have accessed IntelliJ IDEA before, choose **File** \> **Settings** \> **Plugins to** enter the same page, as shown in the following figure.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8119938751/p1557.png)

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9119938751/p1558.png)

3.  On the Plugins page, click **Install plugin from disk…**, as shown in the following figure.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9119938751/p1559.png)

4.  In the displayed window, click the gray icon before a directory for navigation, find the plugin file, select it, and click **OK**.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9119938751/p1561.png)

5.  Return to the Plugins page and click **OK** to install the local plugin.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9119938751/p1562.png)

6.  After the installation is complete, a dialog box is displayed, prompting you to restart IntelliJ IDEA. Click **Restart**.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9119938751/p1563.png)

7.  After IntelliJ IDEA is restarted, the page is displayed as shown in the following figure.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9119938751/p1564.png)


## Next step

Now, you know how to install the MaxCompute Studio plugin. Continue to the next tutorial. In the tutorial, you will learn how to configure a MaxCompute project connection to manage data and resources. For more information, see [Create a MaxCompute project connection](/intl.en-US/Tools and Downloads/MaxCompute Studio/Project space connection management.md).

