---
tags: azure, cloud, azure-certifications/az204
---

# Configuring Azure App Services

## Application settings

In Azure App Services, configurations are stored as *environment variables*.

App settings are always encrypted when stored (*encrypted-at-rest*).

If you are using [[deployment slots]], you can mark a setting as *swappable*.

Note: **the key format changes for Linux-based containers**. Instead of using `Key:SubKey` you have to use `Key__SubKey`.

## General settings

There are some settings that define the underlying infrastructure of the App Service.

Depending on the values, the application might require to scale up to a higher pricint tier.

Examples are:

* **Stack settings**: language and SDK version;
* **Platform settings**: used to configure the hosting platform.
  * **Bitness**: 32- or 64-bit;
  * **WebSocket protocol**: useful for SignalR;
  * **[[always-on]]**: disabled by default;
  * **Managed pipeline version**: The IIS pipeline mode. Set it to *Classic* if you have a legacy app that requires an older version of IIS.
  * **HTTP version**: Set to 2.0 to enable support for HTTPS/2 protocol.
  * **[[general/arr-affinity]]**:  In a multi-instance deployment, ensure that the client is routed to the same instance for the life of the session. You can set this option to *Off* for stateless applications.
* **Debugging**: Enable remote debugging for ASP.NET, ASP.NET Core, or Node.js apps. This option turns off automatically after 48 hours.
* **Incoming client certificates**: require client certificates in mutual authentication. [[tls]] mutual authentication is used to restrict access to your app by enabling different types of authentication for it.

## Path mappings

You can configure **handler mappings,** and **virtual application** and **directory mappings**.

The Path mappings page displays different options based on the OS type.

### Windows apps

For Windows apps, you can customize the IIS handler mappings and virtual applications and directories.

Handler mappings let you add custom script processors to handle requests for specific file extensions.

**Each app has the default root path (/) mapped to *D:\home\site\wwwroot***, where your code is deployed by default. If your app root is in a different folder, or if your repository has more than one application, you can edit or add virtual applications and directories.

You can configure virtual applications and directories by specifying each virtual directory and its corresponding physical path relative to the website root (D:\home). To mark a virtual directory as a web application, clear the Directory check box.

### Linux app + containeraized apps

Containerized apps include **all Linux apps and also the Windows and Linux custom containers** running on App Service.

You can use a storage account as a storage mount. The mount can be a [[azure-files]] or [[azure-blob-storage]].

## Diagnostic logging

There are some built-in deiagnostics associated with App Services.

### Application logging

Logs messages **generated by your application code**. The messages are generated by the web framework you choose, or from your application code directly using the standard logging pattern of your language.

Each message is assigned one of the following categories: Critical, Error, Warning, Info, Debug, and Trace.

**Available for Windows and Linux** platform.

Logs are **stored in the App Service file system and/or Azure Storage blobs**.

For Windows: the Filesystem option is for temporary debugging purposes, and **turns itself off in 12 hours**. The Blob option is for long-term logging, and needs a blob storage container to write logs to.

For Linux: you can specify the Quota (MB) the application logs. You can also specify the Retention Period, set the number of days the logs should be retained.

### Web server logging

Raw HTTP request data in the W3C extended log file format.

Each log message includes data like the HTTP method, resource URI, client IP, client port, user agent, response code, and so on.

**Available only for Windows** platform.

Logs are **stored in the App Service file system or Azure Storage blobs**.

### Detailed error logging

Copies of the .html error pages that would have been sent to the client browser. For security reasons, detailed error pages shouldn't be sent to clients in production, but App Service can save the error page each time an application error occurs that has HTTP code 400 or greater.

**Available only for Windows** platform.

Logs are **stored in the App Service file system**.

### Failed request tracing

Detailed tracing information on failed requests, **including a trace of the IIS components** used to process the request and the time taken in each component. One folder is generated for each failed request, which contains the XML log file, and the XSL stylesheet to view the log file with.

### Deployment logging

Helps determine why a deployment failed. Deployment logging happens automatically and there are no configurable settings for deployment logging.

**Available for Windows and Linux** platform.

Logs are **stored in the App Service file system**.

### Where to find these logs

If you configure the Azure Storage blobs option for a log type, you need a client tool that works with Azure Storage.

For logs stored in the App Service file system, the easiest way is to download the ZIP file in the browser at:

* Linux/container apps: `https://<app-name>.scm.azurewebsites.net/api/logs/docker/zip`
* Windows apps: `https://<app-name>.scm.azurewebsites.net/api/dump`

For Linux/container apps, the ZIP file contains console output logs for both the docker host and the docker container.

**For a scaled-out app**, the ZIP file contains one set of logs for each instance.

In the App Service file system, these log files are the contents of the /home/LogFiles directory.