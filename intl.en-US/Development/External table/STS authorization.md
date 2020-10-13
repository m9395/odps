---
keyword: OSS access permission
---

# STS authorization

This topic describes how to customize the permissions of MaxCompute to access Object Storage Service \(OSS\) and Tablestore in the Resource Access Management \(RAM\) console. MaxCompute uses RAM and Security Token Service \(STS\) of Alibaba Cloud to secure data access.

## STS authorization for OSS

To access OSS data by using external tables in MaxCompute, you must authorize the account used to run MaxCompute jobs. You can grant permissions in the following methods:

-   If MaxCompute and OSS are under the same Alibaba Cloud account, log on to the RAM console and [click here to perform one-click authorization](https://ram.console.aliyun.com/overview).
-   Customize authorization.
    1.  Create a role. Log on to the [RAM console](https://ram.console.aliyun.com/overview). In the left-side navigation pane, click [RAM Roles](https://ram.console.aliyun.com/roles) and create a role such as AliyunODPSDefaultRole or AliyunODPSRoleForOtherUser. If MaxCompute and OSS are not under the same account, log on with the OSS account for authorization.
    2.  Modify the policy content of the role.

        ```
        -- If MaxCompute and OSS are under the same account, use the following policy configuration:
        {
        "Statement": [
        {
         "Action": "sts:AssumeRole",
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "odps.aliyuncs.com"
           ]
         }
        }
        ],
        "Version": "1"
        }
        -- If MaxCompute and OSS are not under the same account, use the following policy configuration:
        {
        "Statement": [
        {
         "Action": "sts:AssumeRole",
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "ID of the Alibaba Cloud account that owns MaxCompute@odps.aliyuncs.com"
           ]
         }
        }
        ],
        "Version": "1"
        }
        ```

    3.  Edit the AliyunODPSRolePolicy authorization policy for the role. You can also customize other permissions.

        ```
        {
        "Version": "1",
        "Statement": [
        {
         "Action": [
           "oss:ListBuckets",
           "oss:GetObject",
           "oss:ListObjects",
           "oss:PutObject",
           "oss:DeleteObject",
           "oss:AbortMultipartUpload",
           "oss:ListParts"
         ],
         "Resource": "*",
         "Effect": "Allow"
        }
        ]
        }
        ```

    4.  Grant the permissions in the AliyunODPSRolePolicy authorization policy to the role.

## STS authorization for Tablestore

To access Tablestore data by using external tables in MaxCompute, you must authorize the account used to run MaxCompute jobs. You can grant permissions in the following methods:

-   If MaxCompute and Tablestore are under the same Alibaba Cloud account, log on to the RAM console and [click here to perform one-click authorization](https://ram.console.aliyun.com/?spm=5176.100239.blogcont281191.24.uJg9dR#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunODPSDefaultRole%22,%20%22TemplateId%22:%20%22DefaultRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fram.console.aliyun.com%2F%22,%20%22Service%22:%20%22ODPS%22%7D).
-   Customize authorization.
    1.  Create a role.

        Log on to the [RAM console](https://account.alibabacloud.com/login/login.html). In the left-side navigation pane, click RAM Roles and create a role such as AliyunODPSDefaultRole or AliyunODPSRoleForOtherUser. If MaxCompute and Tablestore are not under the same account, log on with the Tablestore account for authorization.

    2.  Modify the policy content of the role.

        ```
        -- If MaxCompute and Tablestore are under the same account, use the following policy configuration:
        {
        "Statement": [
        {
         "Action": "sts:AssumeRole",
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "odps.aliyuncs.com"
           ]
         }
        }
        ],
        "Version": "1"
        }
        -- If MaxCompute and Tablestore are not under the same account, use the following policy configuration:
        {
        "Statement": [
        {
         "Action": "sts:AssumeRole",
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "UID of the Alibaba Cloud account that owns MaxCompute@odps.aliyuncs.com"
           ]
         }
        }
        ],
        "Version": "1"
        }
        ```

        **Note:** You can click the profile picture in the upper-right corner to go to the Account Management page. On the page that appears, view the UID of your Alibaba Cloud account.

    3.  Edit the AliyunODPSRolePolicy authorization policy for the role.

        ```
        {
        "Version": "1",
        "Statement": [
        {
         "Action": [
           "ots:ListTable",
           "ots:DescribeTable",
           "ots:GetRow",
           "ots:PutRow",
           "ots:UpdateRow",
           "ots:DeleteRow",
           "ots:GetRange",
           "ots:BatchGetRow",
           "ots:BatchWriteRow",
           "ots:ComputeSplitPointsBySize"
         ],
         "Resource": "*",
         "Effect": "Allow"
        }
        ]
        }
        -- You can also customize other permissions.
        ```

    4.  Grant the permissions in the AliyunODPSRolePolicy authorization policy to the role.

