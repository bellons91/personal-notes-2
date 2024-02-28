---
tags: azure, cloud, azure-certifications/az900, azure-certifications/az204, storage, azure-blob-storage
---

# Azure Blob Storage

Azure Blob Storage is a generic storage solution. You can store **text and binary data** in an unstructured format.

Blob storage can manage thousands of simultaneous uploads.

Blob storage is ideal for:

* Serving images or documents directly to a browser.
* Storing files for distributed access.
* Streaming video and audio.
* Storing data for backup and restore, disaster recovery, and archiving.
* Storing data for analysis by an on-premises or Azure-hosted service.

Objects can be accessed via HTTP or HTTPS, given that every object can be referenced using a specific URL.

**An [[azure-storage-account]] is the top-level container** for all of your Azure Blob storage. The storage account provides a unique namespace for your Azure Storage data.

This service provides a list of endpoints available at  https://{storage-account-name}.blob.core.windows.net

Each Storage Account contains one or more [[azure-blob-container]], and each Blob Container contains

Depending on the usage, you can use a different [[azure-blob-storage-tiers]].

## Security

All data (including metadata) written to Azure Storage is **automatically encrypted using Storage Service Encryption (SSE)**. Data in Azure Storage is encrypted and decrypted transparently using 256-bit AES encryption. Azure Storage encryption is enabled for all new and existing storage accounts and **can't be disabled**.

All Azure Storage resources are encrypted, including blobs, disks, files, queues, and tables. All object metadata is also encrypted. **Storage accounts are encrypted regardless of their performance tier** (standard or premium).

Also, [[azure-rbac]] is available for both data and management operations, like using RBAC roles to manage access to resources and configurations, and using Microsoft Entra to handle access to blob and queue data operations.

You can **assign RBAC roles scoped to a subscription, resource group, storage account, or an individual container or queue** to a security principal or a managed identity for Azure resources.

**Data in transit between an application and Azure can be secured** by using Client-Side Encryption, HTTPS, or [[smb-protocol]] 3.0.

**OS and data disks used by Azure virtual machines can be encrypted** using Azure Disk Encryption.

Encryption relies on keys. You have three options to handle keys:

1. let Microsoft manage and generate keys;
2. create **customer-managed keys** to encrypt and decrypt all data in the storage account. A customer-managed key is used to encrypt all data in all services in your storage account.
3. use a **customer-provided key** on Blob storage operations. A client making a read or write request against Blob storage can include an encryption key on the request for granular control over how blob data is encrypted and decrypted.

| -- | Microsoft-managed keys |Customer-managed keys |Customer-provided keys|
|--|--|--|--|
|Encryption/decryption operations| Azure| Azure| Azure|
|Azure Storage services supported| All |Blob storage, Azure Files| Blob storage|
|Key storage| Microsoft key store| Azure Key Vault Azure |Key Vault or any other key store|
|Key rotation responsibility| Microsoft |Customer |Customer|
|Key usage| Microsoft| Azure portal, Storage Resource Provider REST API, Azure Storage management libraries, PowerShell, CLI| Azure Storage REST API (Blob storage), Azure Storage client libraries|
|Key access| Microsoft only| Microsoft, Customer| Customer only|

## How to operate with Azure Blob Storage using REST APIs

You can use the CLI, .NET SDKs, or REST APIs to work with Blob Storage.

You can define properties (system-wide) and blob-specific metadata.

For REST APIs,the metadata consists of name/value pairs. The total size of all metadata pairs can be up to 8KB in size. Metadata are stored as HTTP headers: you can retrieve them by using both the GET and the HEAD HTTP verb.

```bash
GET/HEAD https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata
```

You can set metadata by using the PUT method. **If you don't set any metadata, the operation clears all the existing ones**.

```bash
PUT https://myaccount.blob.core.windows.net/mycontainer?comp=metadata&restype=container
```

Metadata include:

* **System properties**: System properties exist on each Blob storage resource. Some of them can be read or set, while others are read-only. Under the covers, some system properties correspond to certain standard HTTP headers. The Azure Storage client library for .NET maintains these properties for you.

* **User-defined metadata**: User-defined metadata consists of one or more name-value pairs that you specify for a Blob storage resource. You can use metadata to store additional values with the resource. Metadata values are for your own purposes only, and do not affect how the resource behaves. Their name starts with `x-ms-meta-`.

The standard HTTP headers supported on *containers* include:

* ETag
* Last-Modified

The standard HTTP headers supported on *blobs* include:

* ETag
* Last-Modified
* Content-Length
* Content-Type
* Content-MD5
* Content-Encoding
* Content-Language
* Cache-Control
* Origin
* Range