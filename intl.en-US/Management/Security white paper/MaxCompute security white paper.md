---
keyword: security white paper
---

# MaxCompute security white paper

## Legal disclaimer

Alibaba Cloud reminds you to carefully read and fully understand the terms and conditions of this legal disclaimer before you read or use this document. If you have read or used this document, it shall be deemed as your total acceptance of this legal disclaimer.

1.  You shall download and obtain this document from the Alibaba Cloud website or other channels authorized by Alibaba Cloud, and use this document for your own legal business activities only. The content of this document is considered confidential information of Alibaba Cloud. You shall strictly abide by the confidentiality obligations. No part of this document shall be disclosed or provided to any third party for use without the prior written consent of Alibaba Cloud.
2.  No part of this document shall be excerpted, translated, reproduced, transmitted, or disseminated by any organization, company, or individual in any form or by any means without the prior written consent of Alibaba Cloud.
3.  The content of this document may be changed due to product version upgrades, adjustments, or other reasons. Alibaba Cloud reserves the right to modify the content of this document without notice. The updated versions of this document will be occasionally released through channels authorized by Alibaba Cloud. You shall pay attention to the version changes of this document as they occur and download and obtain the most up-to-date version of this document from channels authorized by Alibaba Cloud.
4.  This document serves only as a reference guide for your use of Alibaba Cloud products and services. Alibaba Cloud provides the document in the context that Alibaba Cloud products and services are provided on an **as is**, **with all faults**, and **as available** basis. Alibaba Cloud makes every effort to provide relevant operational guidance based on existing technologies. However, Alibaba Cloud hereby makes a clear statement that it in no way guarantees the accuracy, integrity, applicability, and reliability of the content of this document, either explicitly or implicitly. Alibaba Cloud shall not bear any liability for any errors or financial losses incurred by any organizations, companies, or individuals arising from their download, use, or trust in this document. Alibaba Cloud shall not, under any circumstances, bear responsibility for any indirect, consequential, exemplary, incidental, special, or punitive damages, including lost profits arising from the use or trust in this document, even if Alibaba Cloud has been notified of the possibility of such a loss.
5.  By law, all the content of the Alibaba Cloud website, including but not limited to works, products, images, archives, information, materials, website architecture, website graphic layout, and web page design, are intellectual property of Alibaba Cloud and/or its affiliates. This intellectual property includes, but is not limited to, trademark rights, patent rights, copyrights, and trade secrets. No part of the Alibaba Cloud website, product programs, or content shall be used, modified, reproduced, publicly transmitted, changed, disseminated, distributed, or published without the prior written consent of Alibaba Cloud and/or its affiliates. The names owned by Alibaba Cloud include, but are not limited to, **Alibaba Cloud**, **Aliyun**, **HiChina**, and other brands of Alibaba Cloud and/or its affiliates, which appear separately or in combination, as well as the auxiliary signs and patterns of the preceding brands, or anything similar to the company names, trade names, trademarks, product or service names, domain names, patterns, logos, marks, signs, or special descriptions that third parties identify as Alibaba Cloud and/or its affiliates.
6.  Please contact Alibaba Cloud directly if you discover any errors in this document.

## Secure isolation

MaxCompute is designed to handle security issues in multi-tenant scenarios. It integrates the authentication system of Alibaba Cloud to authenticate users by using symmetric AccessKey pairs. MaxCompute verifies the signature in each HTTP request, and stores and isolates data of different users in Apsara Distributed File System. This allows MaxCompute to meet the requirements for multi-tenant collaboration, data sharing, data confidentiality, and data security.

MaxCompute runs all computing tasks in individual sandboxes. The sandbox architecture has multiple layers from the kernel layer to the KVM virtualization layer. The sandboxes use an authentication mechanism to guarantee data security and prevent server faults caused by misoperations or malicious operations.

## Network isolation

MaxCompute is a big data platform provided by Alibaba Cloud to process huge volumes of data. It complies with security isolation standards to ensure user data security. MaxCompute supports Virtual Private Clouds \(VPCs\), which allow you to isolate data.

Traffic among the networks is controlled in the following ways:

-   Classic networks, VPCs, and the Internet are isolated from each other. Users can only access endpoints and virtual IP addresses \(VIPs\) of their own networks.
-   Projects that do not have VPC IDs or IP address whitelists configured are accessible to the three types of networks by using domain names.
-   Projects that have VPC IDs configured are only accessible to the specified VPCs.
-   Projects that have IP address whitelists configured are accessible to the hosts whose IP addresses are added to the IP address whitelists.
-   If a request is sent by a proxy, the request is allowed or denied based on the VPC ID or last-hop IP address.

## Authentication

-   Identity authentication

    You can create an AccessKey pair in the Alibaba Cloud console. An AccessKey pair consists of an AccessKey ID and AccessKey secret. The AccessKey ID is public and uniquely identifies a user, whereas the AccessKey secret is private and used to authenticate user identities.

    Before the client sends a request to MaxCompute, it generates a string to be signed in the format specified by MaxCompute and then generates a signature for the request by using the AccessKey secret. After MaxCompute receives the request, it finds the AccessKey secret based on the AccessKey ID and then generates a signature. If the signature is the same as that sent by the client, the request is valid. Otherwise, MaxCompute rejects the request and returns an HTTP 403 error.

-   Permission control

    You can use an Alibaba Cloud account or a RAM user to access MaxCompute resources. Different RAM users can be created in one Alibaba Cloud account. MaxCompute checks the permissions of your Alibaba Cloud account or RAM user each time you access its resources.

    -   When you access a resource by using an Alibaba Cloud account, MaxCompute checks whether the account is the resource owner. In most cases, a resource is accessible only to its owner.
    -   When you access a resource as a RAM user, MaxCompute checks whether the Alibaba Cloud account of the RAM user is the resource owner and whether the RAM user has been granted permissions on that resource.
    **Note:** If an Alibaba Cloud account \(not the resource owner\) and its RAM users are granted permissions on the resource, they can also access the resource.

    MaxCompute uses an ACL-based authorization mechanism to control access from RAM users.

    ACL-based authorization is an object-based authorization mechanism. An access control list \(ACL\) contains access permissions on an object. It takes effect only if the object exists. If the object is deleted, the ACL of the object is also deleted. ACL-based authorization is similar to the authorization mechanism that is used by the GRANT and REVOKE statements defined in SQL-92. You need to execute statements to grant or revoke permissions on an object.

    To manage permissions, you must define the effect \(grant or revoke\), object \(such as a table or resource\), subject \(user or role\), and operation \(such as read, write, or delete\). For example, grant the read permission on table 1 to user zinan.tang.

    MaxCompute also supports other authorization mechanisms in the following scenarios:

    -   Cross-project resource sharing

        If you are the owner or administrator \(with the admin role\) of a project, and another user wants to access resources in your project. If the user belongs to your project team, we recommend that you grant permissions to the user by using the authorization management feature. Otherwise, you can share resources with the user across projects by using packages.

        Packages are usually used when cross-project user authorization is required. They work in the following way:

        The administrator of project A packs up all objects required by the user in project B and grants the administrator of project B permissions to install the package. The administrator of project B then installs the package in project B and determines whether to grant the permissions of the package to other users in project B.

        The following section describes how to use a package:

        -   For a package creator

            ```
            -- Create a package.
            create package <pkgname>;
            ** Note:
            -- Only a project owner is permitted to perform this operation.
            -- The package name cannot exceed 128 characters in length.
            
            -- Add objects to the package.
            add project_object to package package_name [with privileges privileges];
            remove project_object from package package_name;
            project_object ::= table table_name |
            instance inst_name |
            function func_name |
            resource res_name
            privileges ::= action_item1, action_item2, ...
            ** Note:
            -- The entire project cannot be added as an object to a package. 
            -- You need to specify the permissions allowed on the objects in the [withprivileges privileges] part. If you do not specify permissions, the object is read-only. Only the read, describe, and select operations are allowed on read-only objects. An object and its permissions are inseparable and cannot be changed after you add them to a package. If you want to change them, delete the object and add it again.
            -- Grant the permissions on the package to another project.
            allow project <prjname> to install package <pkgname> [using label <number>];
            -- Revoke the permissions on the package from another project.
            disallow project <prjname> to install package <pkgname>;
            
            -- Delete a package.
            delete package <pkgname>;
            
            -- View the list of packages.
            show packages;
            
            -- View details of a package.
            describe package <pkgname>;
            ```

        -   For a user of a package

            ```
            -- Install a package.
            install package <pkgname>; 
            -- Note:
            -- Only a project owner is permitted to perform this operation.
            -- The name of the package you want to install (pkgName) must be in the format of <projectName>.<packageName>.
            -- Uninstall a package.
            uninstall package <pkgname>; 
            -- The name of the package you want to uninstall (pkgName) must be in the format of <projectName>.<packageName>.
            -- View the list of created and installed packages.
            show packages;
            -- View details of a package.
            describe package <pkgname>;
            ```

            An installed package is an independent object in MaxCompute. To obtain resources in a package shared by the user of another project, you must have the read permission on that package. If you do not have the read permission on the package, you can submit an application to the project owner or administrator to obtain the permission. The project owner or administrator grants the permission to you by using ACLs.

            For example, the following ACL rules allow account odps\_test@aliyun.com to access a package:

            ```
            use prj2;
            install package prj1.testpkg;
            grant read on package prj1.testpackage to user
            aliyun$odps_test@aliyun.com;
            ```

    -   Column-level access control

        Label Security enables fine-grained mandatory access control \(MAC\) for a project. It allows the project administrator to control user access to column data with different levels of sensitivity.

        Label Security classifies both data and users who need to access the data into different levels. The data is classified into the following levels based on its sensitivity:

        -   Level 0: unclassified
        -   Level 1: confidential
        -   Level 2: sensitive
        -   Level 3: highly sensitive
        MaxCompute adopts the preceding data levels. Project owners must define their own standards to determine the data sensitivity levels and access permissions for each level of data. By default, the data sensitivity level and access permission level of all users is 0.

        Label Security allows project owners to label table columns and views with different sensitivity levels.

        By default, the sensitivity level of a new view is 0. The sensitivity levels of views and base tables are independent of each other.

        Label Security applies the following default security policies based on the levels of data and users:

        -   No-ReadUp: Users are not allowed to read data that has a higher sensitivity level than their own, unless they are explicitly authorized.
        -   Trusted-User: Users are allowed to write data into columns regardless of their sensitivity levels. The default sensitivity level of a new column is 0.
        **Note:**

        -   Traditional MAC systems use sophisticated security policies to prevent unauthorized data operations in projects. For example, the No-WriteDown policy only allows a user to write data into columns with a higher sensitivity level than the user's level. By default, MaxCompute does not support the No-WriteDown policy. This simplifies project owners' work to manage data sensitivity levels. Project owners can set the `SetObjectCreatorHasGrantPermission` parameter to false to implement a policy similar to No-WriteDown.
        -   If you want to control data transmission across projects, you can enable project protection for your project. After the setting takes effect, users can only access data within their own projects and cannot share it with other projects.
        By default, Label Security is disabled. The project owner can enable it as needed. After Label Security is enabled, the preceding default security policies take effect. Users must have the select permission and the required level to access sensitive data in the tables.

        -   Project protection

            Users who are authorized to access data in multiple projects can transmit data across these projects. If a project contains highly sensitive data that cannot be shared with other projects, the project owner can set projectProtection to true.

            Configuration command:

            ```
            set projectProtection=true;
            -- This command allows the data only to be written into the project but not to be read across projects.
            -- By default, projectProtection is set to false. You need to manually set it to true to enable project protection.
            ```

        -   Data transmission across projects after project protection is enabled

            If project protection is enabled for your project but you want to transmit data to another project, set the project to which data transmits as a trusted project so data transmission to that project is allowed. If multiple projects are set as trusted projects for each other, they form a trusted project group. Data can be transmitted within the project group but cannot go outside.

            Commands to manage trusted projects:

            ```
            list trustedprojects;
            -- List all trusted projects added to the current project. 
            add trustedproject <projectname>;
            -- Add a trusted project to the current project. 
            remove trustedproject <projectname>;
            -- Remove a trusted project from the current project.
            ```

        -   Resource sharing and data protection

            MaxCompute supports both package-based resource sharing and project protection. However, their functions are mutually exclusive.

            Resource sharing takes precedence over project protection. If a data object is shared with users in another project, the data object is not limited by project protection.

            To prevent the leaks of sensitive data, set `projectProtection` to true and verify the following items:

            -   No trusted projects are added. If a trusted project is added, check whether it has potential risks.
            -   No exception policies are configured. If an exception policy is configured, assess the potential risks, especially data leaks due to TOC2TOU.
            -   No data is shared by using packages. If packages are used to share data, make sure that they do not contain sensitive data.
-   Access control for RAM users

    MaxCompute supports RAM authentication.

    Resource Access Management \(RAM\) is a resource access control service provided by Alibaba Cloud. It allows you to create RAM users under an Alibaba Cloud account and grant them permissions to access resources.


## Data security

MaxCompute passed compliance audits on the security, availability, and confidentiality principles of Alibaba Cloud systems under the AICPA Trusted service standards. These audits are conducted by third-party auditors. For audit reports, see [SOC 3 report](https://www.alibabacloud.com/zh/trust-center/soc).

Alibaba Cloud provides a flat storage system in which linear address space is divided into chunks. Each chunk is duplicated into three copies. Each copy is stored on different nodes in the cluster to ensure the reliability of data.

In the data storage system, there are three key components: master, chunk server, and client. Write operations in MaxCompute are processed and executed by the client in the following process:

1.  The client determines the location of the chunk requested by the write operation.
2.  The client sends a request to the master to query the chunk servers where the three chunk replicas are stored.
3.  The master returns the addresses of the chunk servers, and the client then sends the write request to the chunk servers.
4.  If the write operation succeeds in all three chunk replicas, the client returns a success message. Otherwise, the client returns a failure message.

The master takes into account the disk usage of all chunk servers in the cluster, distribution of chunk servers in different switch racks, power supply status, and machine load to make sure that the three replicas of a chunk are distributed to different chunk servers in different racks. This effectively prevents a single point of failure caused by the failure of a chunk server or rack.

If a data node or its hard disks are faulty, the total number of valid replicas of some chunks may become less than three. In this case, the master replicates data between chunk servers to make sure that each chunk in the cluster has three valid replicas.

All data operations in MaxCompute, such as the add, modify, and delete operations, are synchronized to the three replicas. The three-replica mechanism guarantees data reliability and consistency.

After you delete data, the storage space is reclaimed by ADFS. Before the storage space is released, it is not accessible to any users, and ADFS erases data from it. This provides maximum protection for your data.

## Encryption before transmission

MaxCompute provides RESTful APIs to transmit data over HTTPS.

## Log audits

MaxCompute audits logs generated by different users. It stores logs and metadata in a metadata warehouse.

The metadata includes static data, operation logs, and security information. You can query the metadata and analyze the running status of MaxCompute.

-   Static data is written permanently into the data warehouse.
-   Operation logs record task running processes and are stored in only one partition.
-   Security information originates from Table Store and includes whitelists and ACLs.

## Access control - IP address whitelist

MaxCompute supports multiple levels of access control to guarantee security, such as the multi-tenant security authentication mechanism. Only users who have acquired authorized AccessKey ID and AccessKey secret are allowed to access data based on the granted permissions. This section describes how to configure access control policies by using IP address whitelists.

You can determine the IP addresses that need to add to IP address whitelists by using the following methods:

-   If you access MaxCompute by using the odpscmd client, obtain the IP address of the odpscmd client.
-   If you use an application system, such as DataWorks or Data Integration, to access MaxCompute, obtain IP addresses of the servers where DataWorks or Data Integration is deployed. The IP addresses of the default servers are automatically added to the whitelist.
-   If you use a proxy server to access MaxCompute, obtain the IP address of the last-hop proxy server.
-   If you access MaxCompute from an ECS instance, obtain the NAT IP address.

Multiple IP addresses are separated with `commas (,)`. You can configure the IP addresses of the following types:

-   Individual IP addresses.
-   An IP address range. The start IP address and the end IP address are separated with a hyphen `(-)`.
-   An IP address with a subnet mask.

```
-- Individual IP addresses
10.32.180.8,10.32.180.9,10.32.180.10
-- An IP address range  
10.32.180.8-10.32.180.12
-- An IP address with a subnet mask
10.32.180.0/23
```

This section describes how to configure an IP address whitelist for a project.

Run the following command on the client as the project owner to add an IP address whitelist:

```
setproject odps.security.ip.whitelist=101.132.236.134,100.116.0.0/16,101.132.236.134-101.132.236.144;
```

**Note:**

-   Only IP addresses in the whitelist, such as the outbound IP addresses of the odpscmd client or SDK, can access the project.
-   The IP address whitelist takes effect in five minutes after you execute the command.
-   If you have blocked your access to your own project due to misoperations, [submit a ticket](https://workorder-intl.console.aliyun.com/) to Alibaba Cloud for help.

Run the following command to disable the IP address whitelist:

```
setproject odps.security.ip.whitelist=;
```

Impact

-   Before you add an IP address whitelist, MaxCompute does not restrict access to the project.
-   After you add the IP address whitelist, only IP addresses and IP address ranges in the whitelist are allowed to access the project. You can achieve fine-grained access control by using the IP address whitelist together with the authentication mechanism based on AccessKey ID and AccessKey secret.

