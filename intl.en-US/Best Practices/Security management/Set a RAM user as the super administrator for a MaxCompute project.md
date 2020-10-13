---
keyword: [RAM user, super administrator]
---

# Set a RAM user as the super administrator for a MaxCompute project

This topic describes how to set a RAM user as the super administrator for a MaxCompute project, and provides suggestions on how to manage members and permissions.

## Background information

To ensure data security, the Alibaba Cloud account of a project is used only by authorized personnel. Common users can only log on to MaxCompute as RAM users. A project owner must be the Alibaba Cloud account, and some operations can only be performed by the project owner, such as setting a project flag and configuring cross-project resource sharing by using packages. If you use a RAM user, make sure that it has been granted the super administrator role.

The built-in management role Super\_Administrator has been added to MaxCompute. This role has permissions on all types of resources in a project and project management permissions. For more information about permissions, see [Management role permissions](/intl.en-US/Management/Security management/Manage roles/Management role permissions.md).

A project owner can grant the Super\_Administrator role to a RAM user. As a super administrator, the RAM user has the permissions needed to manage the project, such as common project flag setting permissions and permissions on managing all resources.

## Authorization methods

We recommend that you grant the Super\_Administrator role to a RAM user that has the permissions to create a project. This way, the RAM user can manage both DataWorks workspaces and MaxCompute projects that are associated with these DataWorks workspaces.

**Note:**

-   For information about how to authorize a RAM user to create projects, see [Grant permissions to the RAM user]().
-   To ensure data security, we recommend that you clarify the responsibilities of owners of RAM users. Make sure that each RAM user belongs to one developer.
-   Only one RAM user can be granted the Super\_Administrator role in a project. You can grant the Admin role to other RAM users that require basic management permissions.

After you select a RAM user and use the RAM user to create a project, the project owner is still the Alibaba Cloud account, who can grant the Super\_Administrator role to the RAM user in the following ways:

-   Grant the Super\_Administrator role on the MaxCompute client.

    Assume that user bob@aliyun.com is the owner of the project\_a project, and user Allen is a RAM user under bob@aliyun.com.

    1.  Run the following commands to grant the Super\_Administrator and Admin roles as user bob@aliyun.com:

        ```
        -- Open project_a.
        use project_a;
        -- Add the RAM user Allen to project_a.
        add user ram$bob@aliyun.com:Allen;
        -- Grant the Super_Administrator role to Allen.
        grant super_administrator TO ram$bob@aliyun.com:Allen;
        -- Grant the Admin role to Allen.
        grant admin TO ram$bob@aliyun.com:Allen;
        ```

    2.  Run the following command to view the permissions as the authorized RAM user:

        ```
        show grants;
        ```

        If the Super\_Administrator role is in the command output, the authorization succeeded.

-   Grant the Super\_Administrator role in the DataWorks console.
    1.  Log on to the DataWorks console and choose [Workspace Management](https://setting-cn-shanghai.data.aliyun.com/).
    2.  Optional. Add a RAM user as a project member. Skip this step if the RAM user is already a project member.
        1.  In the left-side navigation pane, click **User Management**.
        2.  In the upper-right corner, click **Add Member**.
        3.  In the **Add Member** dialog box, select the members you want to add from the **Accounts to be added** section and click the rightwards arrow to add them to the **Added account** section.

            **Note:** In the note block, click **Refresh** to synchronize the RAM users under the current Alibaba Cloud account to the **Account to be added** section.

        4.  Select the required roles and click **Confirm**.
    3.  Grant the Super\_Administrator role to the RAM user.
        1.  In the left-side navigation pane, click **Maxcompute Management**.
        2.  In the navigation tree, click **Custom user roles**.
        3.  Find the role that you want to grant to the user and click **Member management** in the Operation column. In the Member management dialog box, select the members you want to add from the **Account to be added** section and click the rightwards arrow to add them to the **Added account** section.

            ![Members](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9431752061/p77506.png)

        4.  Click **Confirm**.
    4.  Run the following command to view the permissions as the authorized RAM user:

        ```
        show grants;
        ```

        If the Super\_Administrator role is in the command output, the authorization succeeded.


## Usage notes

-   Member management
    -   MaxCompute supports the Alibaba Cloud account and RAM users. To ensure data security, we recommend that you only add RAM users under the project owner as project members.

        The Alibaba Cloud account is used to control RAM users, such as revoking or updating their credentials. This ensures data security in the case of personnel transfers and resignations.

        **Note:** If you use DataWorks to manage project members, you can add only RAM users under the project owner as project members.

    -   RAM users can be added by the Alibaba Cloud account and the super administrator. If you want to add RAM users to a project as the super administrator, wait until the RAM users are created by the Alibaba Cloud account.
    -   We recommend that you only add the users who need to develop data, namely, users who need to run jobs, in the current project as project members. For users who require data interactions, you can use packages to share resources across projects. This reduces the complexity of member management because fewer members are added to the project.
    -   If an employee who has a RAM user is transferred to another position or resigns, the RAM user with the Super\_Administrator role needs to remove the RAM user of the employee from the project, and then notify the project owner to revoke its credentials. If an employee who has a RAM user with the Super\_Administrator role is transferred to another position or resigns, the Alibaba Cloud account must be used to remove the RAM user and revoke its credentials.
-   Permission management

    -   We recommend that you manage permissions by role. Permissions are associated with roles, and roles are associated with users.
    -   We recommend that you use the principle of least privilege to avoid security risks caused by excessive permissions.
    -   If you need to use cross-project data, we recommend that you share resources by using packages. In this way, resource providers only need to manage packages, which avoids the extra costs caused by the management of additional members.
    **Note:** A RAM user who has been granted the Super\_Administrator role has the permissions to query and manage all resources in a project. Therefore, no additional permissions need to be granted to the RAM user.

-   Permission auditing

    You can use the view provided by the MaxCompute metadata service to audit permissions. For more information, see [Metadata views](/intl.en-US/Management/Information schema/Metadata views.md).

-   Cost management

    For more information, see [View billing details](/intl.en-US/Pricing/View billing details.md). RAM users can query the billing details only after the Alibaba Cloud account grants them the permissions to access Billing Management. For information about how to grant permissions, see [Grant permissions to a RAM role](/intl.en-US/RAM Role Management/Grant permissions to a RAM role.md). The following permissions are required:

    -   AliyunBSSFullAccess: the permissions to manage Billing Management.
    -   AliyunBSSReadOnlyAccess: the access and read-only permissions on Billing Management.
    -   AliyunBSSOrderAccess: the permissions to view, pay for, and cancel orders in Billing Management.
    **Note:** Permissions on Billing Management are independent of the Super\_Administrator role of a MaxCompute project. You must grant these permissions separately to the user.

-   Resource usage management
    -   If you use subscription computing resources of MaxCompute, you can view the usage of computing resources and manage all the computing resources on MaxCompute Management. For more information, see [Use MaxCompute Management](/intl.en-US/Management/Resource and job management/MaxCompute Management.md).
    -   If you use pay-as-you-go computing resources of MaxCompute, you can view the usage of computing resources in the view provided by the MaxCompute metadata service. For example, TASKS\_HISTORY allows you to view the execution details of audit jobs, such as the time, content, and resource consumption. For more information, see [TASKS\_HISTORY](/intl.en-US/Management/Information schema/Metadata views.md).

        **Note:** The view provided by the metadata service only retains data generated in the last 15 days. If you need to store data for a longer period of time, we recommend that you regularly read and save the data locally.


