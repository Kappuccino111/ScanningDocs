
# Understanding the roots of PAPPL!

Want to understand the complete working functionality of PAPPL without wading through tons of code and tedious, linear documentation? This guide is designed to help you achieve just that—in less time! You'll gain a clear understanding of PAPPL's core functionality. So, what are you waiting for? Dive right in!

---

## What is PAPPL?

> **Short Answer**: PAPPL is a C-based framework/library for developing [CUPS](https://www.cups.org/) Printer Applications, which are the modern replacement for traditional printer drivers. It’s designed to support a wide range of printers and drivers across desktops, servers, and embedded environments.

**PAPPL** stands for **Printer Application Programming in C** (some folks just call it "Pap-ple," but hey, we don’t judge!).

At its core, PAPPL helps you package printer drivers into self-contained "Printer Applications," allowing them to be discovered and emulated as network printers (and also supporting USB/local printing). This approach streamlines modern printing environments by offering a simpler interface, better security, and cross-platform compatibility.

**Primary Goal**: Provide a *simple* way to create printer applications, intended to replace traditional printer drivers.

---

## Key Features and Highlights

PAPPL includes a ton of functionality right out of the box:

- **Framework for Printer Applications**: Tools and structure to build complete printer applications.
- **Replaces Traditional Drivers**: Adopts a modern approach using CUPS Printer Applications.
- **Multi-Platform Support**: Compatible with Windows, Linux, and macOS.
- **Embedded HTTP/IPP Server**: Built-in multi-threaded HTTP/IPP Everywhere server.
- **Event Callbacks**: Hooks for various events to interact with local/network clients.
- **Driver Interface**: A familiar raster interface for CUPS Raster developers.
- **Multiple Printer Types**: Works with both raster and vector printers.
- **Automatic File Format Support**: Supports JPEG, PNG, PWG Raster, Apple Raster, and raw files out of the box.
- **Extensibility**: Register custom filter callbacks for additional file formats.
- **Connection Management**: Handles clients, devices, listeners, logs, printers, and resources.
- **Optional Embedded Web Interface**: For easy local or remote printer configuration.
- **Raw Socket & USB Support**: Raw socket printing (port 9100) and USB gadget functionality (Linux).
- **Localization**: Functions for localizing the web interface in multiple languages.
- **Security**: Manage HTML forms securely with CSRF tokens and user authentication.
- **Logging**: Built-in logging utilities for system, device, job, and printer events.
- **Resource Management**: APIs for adding static/dynamic resources to the HTTP server.

**TL;DR**: Think of PAPPL as a nice, modern building kit for turning your printer drivers into friendly, IPP Everywhere-ready "network printers."

> **Note**: While scanning functionality is still under development, you can already see the code scaffolding for future scanning features in PAPPL 1.4.x. Exciting times ahead!

---

## Core Components of PAPPL

PAPPL is neatly divided into several main components:

1. [**System**](#system-pappl_system_t)
2. [**Clients**](#clients-pappl_client_t)
3. [**Devices**](#devices-pappl_device_t)
4. [**Printers**](#printers-pappl_printer_t)
5. [**Jobs**](#jobs-pappl_job_t)
6. [**Error Codes and Structures**](#error-codes-and-structures)
7. [**Callback Functions**](#callback-functions)
8. [**From Scratch Execution Flow**](#from-scratch-execution-flow)

---

## System (`pappl_system_t`)

The **System** component (represented by `pappl_system_t`) is the beating heart of a PAPPL printer application. It manages:

- **Client and Device Connections**
- **Network Listeners**
- **Logging**
- **Printers**
- **Resources**
- **IPP System Service Subset**
- **Embedded Web Interface** (optional)
- **Raw Socket Printing**
- **USB Printer Gadget** (on Linux)
- **State Loading/Saving**

### System Functions

Below is a (nearly) comprehensive list of **System** functions. You can think of these as your master control switches for any PAPPL application.

| **Function**                             | **Description**                                                                                                                                                                                                                       |
|-----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `papplSystemCreate`                     | Creates a new system object. Defines name, spool directory, logging, TLS mode, etc.                                                                                                                                                  |
| `papplSystemDelete`                     | Deletes a system object, cleaning up resources.                                                                                                                                                                                      |
| `papplSystemFindPrinter`                | Finds a printer by name, ID, or device URI.                                                                                                                                                                                          |
| `papplSystemIteratePrinters`            | Iterates through printers managed by the system, calling a callback for each.                                                                                                                                                       |
| `papplSystemLoadState`                  | Loads system/printer states from a file.                                                                                                                                                                                             |
| `papplSystemSaveState`                  | Saves current system/printer states to a file.                                                                                                                                                                                       |
| `papplSystemAddListeners`               | Adds IP and domain socket listeners for incoming connections.                                                                                                                                                                        |
| `papplSystemGetAdminGroup`              | Retrieves the name of the admin group.                                                                                                                                                                                               |
| `papplSystemGetAuthService`             | Gets the authorization service name (e.g., for PAM).                                                                                                                                                                                 |
| `papplSystemGetContact`                 | Fetches the system contact info (usually name, phone, email).                                                                                                                                                                        |
| `papplSystemGetDefaultPrinterID`        | Gets the numeric ID of the default printer.                                                                                                                                                                                          |
| `papplSystemGetDefaultPrintGroup`       | Fetches the default print group name.                                                                                                                                                                                                |
| `papplSystemGetDNSSDName`               | Retrieves the DNS-SD service instance name.                                                                                                                                                                                          |
| `papplSystemGetFooterHTML`              | Gets the HTML used at the bottom of the web interface.                                                                                                                                                                               |
| `papplSystemGetGeoLocation`             | Gets the geographic location as a `geo:` URI.                                                                                                                                                                                        |
| `papplSystemGetHostName`                | Fetches the hostname for the system.                                                                                                                                                                                                 |
| `papplSystemGetHostPort`                | Retrieves the port number used by the system.                                                                                                                                                                                        |
| `papplSystemGetLocation`                | Gets the human-readable system location (e.g., “Main Office”).                                                                                                                                                                       |
| `papplSystemGetLogLevel`                | Fetches the current log level.                                                                                                                                                                                                       |
| `papplSystemGetMaxClients`              | Retrieves the max number of simultaneous network clients.                                                                                                                                                                            |
| `papplSystemGetMaxLogSize`              | Gets the maximum log file size (for file-based logging).                                                                                                                                                                             |
| `papplSystemGetMaxSubscriptions`        | Gets the max number of event subscriptions allowed.                                                                                                                                                                                  |
| `papplSystemGetName`                    | Retrieves the system name (set in `papplSystemCreate`).                                                                                                                                                                              |
| `papplSystemGetNextPrinterID`           | Gets the ID that’ll be assigned to the next new printer.                                                                                                                                                                             |
| `papplSystemGetOptions`                 | Returns the system options that were provided at creation.                                                                                                                                                                           |
| `papplSystemGetOrganization`            | Gets the system’s organization name.                                                                                                                                                                                                  |
| `papplSystemGetOrganizationalUnit`      | Gets the organizational unit name.                                                                                                                                                                                                    |
| `papplSystemGetPassword`                | Retrieves the web interface password hash.                                                                                                                                                                                           |
| `papplSystemGetServerHeader`            | Gets the HTTP “Server:” header value.                                                                                                                                                                                                 |
| `papplSystemGetSessionKey`              | Retrieves the current cryptographic session key.                                                                                                                                                                                     |
| `papplSystemGetTLSOnly`                 | Checks if the system is configured to allow only TLS connections.                                                                                                                                                                    |
| `papplSystemGetUUID`                    | Gets the system’s UUID.                                                                                                                                                                                                              |
| `papplSystemGetVersions`                | Retrieves firmware version info reported to clients.                                                                                                                                                                                 |
| `papplSystemSetAdminGroup`              | Sets the name of the admin group.                                                                                                                                                                                                    |
| `papplSystemSetContact`                 | Configures system contact info (admin name, etc.).                                                                                                                                                                                   |
| `papplSystemSetDefaultPrinterID`        | Sets the ID of the default printer.                                                                                                                                                                                                   |
| `papplSystemSetDefaultPrintGroup`       | Sets the default print group name.                                                                                                                                                                                                    |
| `papplSystemSetPrinterDrivers`          | Sets the list of printer drivers, plus callbacks for auto-add, driver create, and driver matching.                                                                                                                                    |
| `papplSystemSetDNSSDName`               | Updates the DNS-SD service instance name.                                                                                                                                                                                             |
| `papplSystemSetFooterHTML`              | Sets the HTML that appears at the bottom of the web interface.                                                                                                                                                                        |
| `papplSystemSetGeoLocation`             | Sets the system’s geographic location as a `geo:` URI.                                                                                                                                                                               |
| `papplSystemSetHostName`                | Configures the hostname for the system.                                                                                                                                                                                               |
| `papplSystemSetLocation`                | Sets the system’s human-readable location.                                                                                                                                                                                            |
| `papplSystemSetLogLevel`                | Adjusts the current log level.                                                                                                                                                                                                        |
| `papplSystemSetMaxClients`              | Sets the maximum concurrent network clients.                                                                                                                                                                                         |
| `papplSystemSetMaxLogSize`              | Sets the maximum size for the log file.                                                                                                                                                                                               |
| `papplSystemSetMaxSubscriptions`        | Sets the maximum allowed number of event subscriptions.                                                                                                                                                                              |
| `papplSystemSetMIMECallback`            | Assigns a callback for custom MIME type detection.                                                                                                                                                                                   |
| `papplSystemSetNextPrinterID`           | Manually sets the next printer ID that’ll be assigned.                                                                                                                                                                               |
| `papplSystemSetOperationCallback`       | Assigns a custom IPP operation callback.                                                                                                                                                                                              |
| `papplSystemSetOrganization`            | Sets the organization name.                                                                                                                                                                                                           |
| `papplSystemSetOrganizationalUnit`      | Sets the organizational unit name.                                                                                                                                                                                                    |
| `papplSystemSetPassword`                | Sets the web interface’s password hash.                                                                                                                                                                                               |
| `papplSystemSetSaveCallback`            | Assigns a save callback to be used for storing system state.                                                                                                                                                                         |
| `papplSystemSetUUID`                    | Updates the system’s UUID.                                                                                                                                                                                                            |
| `papplSystemSetVersions`                | Sets the firmware version info reported to clients.                                                                                                                                                                                  |
| `papplSystemAddEvent`                   | Adds a notification event to be broadcast to any subscribed clients.                                                                                                                                                                  |
| `papplSystemAddLink`                    | Adds a link to the navigation header in the web UI.                                                                                                                                                                                  |
| `papplSystemRemoveLink`                 | Removes a link from the navigation header.                                                                                                                                                                                            |
| `papplSystemAddMIMEFilter`              | Adds a file-format filter to handle custom document conversions.                                                                                                                                                                      |
| `papplSystemAddResourceCallback`        | Dynamically serves resources (HTML, images, etc.) from a callback.                                                                                                                                                                    |
| `papplSystemAddResourceData`            | Adds a static data resource to the web server.                                                                                                                                                                                       |
| `papplSystemAddResourceDirectory`       | Adds all files from a directory as resources.                                                                                                                                                                                        |
| `papplSystemAddResourceFile`            | Adds a single file as a resource.                                                                                                                                                                                                     |
| `papplSystemAddStringsData`             | Adds in-memory localization (strings) data.                                                                                                                                                                                           |
| `papplSystemAddStringsFile`             | Adds a file-based localization resource.                                                                                                                                                                                              |
| `papplSystemRemoveResource`             | Removes a previously-added resource.                                                                                                                                                                                                  |
| `papplSystemAddTimerCallback`           | Installs a timer callback to run at intervals.                                                                                                                                                                                        |
| `papplSystemRemoveTimerCallback`        | Removes a timer callback.                                                                                                                                                                                                             |
| `papplSystemCleanJobs`                  | Cleans out old, completed jobs.                                                                                                                                                                                                       |
| `papplSystemIsRunning`                  | Checks if the system is currently running.                                                                                                                                                                                            |
| `papplSystemIsShutdown`                 | Checks if the system has been shut down.                                                                                                                                                                                              |
| `papplSystemShutdown`                   | Shuts down the system gracefully.                                                                                                                                                                                                     |
| `papplSystemMatchDriver`                | Matches an IEEE-1284 device ID to a known driver.                                                                                                                                                                                    |
| `papplSystemSetNetworkCallbacks`        | Sets callbacks for network configuration (Wi-Fi, etc.).                                                                                                                                                                              |
| `papplSystemSetAuthCallback`            | Sets an authentication callback for a given scheme.                                                                                                                                                                                  |
| `papplSystemFindLoc`                    | Finds localization info for a given language.                                                                                                                                                                                        |
| `papplSystemFindSubscription`           | Finds an event subscription by ID.                                                                                                                                                                                                    |
| `papplSystemGetPassword`                | (Duplicate reference) Fetches the password hash for the system.                                                                                                                                                                       |
| `papplSystemSetWiFiCallbacks`           | Sets callbacks for handling Wi-Fi events (join, list, status).                                                                                                                                                                        |
| `papplSystemSetEventCallback`           | Sets a system-level event callback.                                                                                                                                                                                                   |
| `papplSystemHashPassword`               | Generates a password hash using a salt.                                                                                                                                                                                               |

### System & Main Loop

Most PAPPL applications use `papplMainloop` to handle common setup tasks—like parsing command-line arguments and creating/running the system. You can provide your own `system_cb` to customize `papplSystemCreate`; otherwise PAPPL does it for you with defaults.

---

## Clients (`pappl_client_t`)

**Clients** represent individual incoming connections—HTTP, IPP, or raw socket. The system object automatically manages their lifecycle.

### What Clients Do

- Manage client requests (e.g., print jobs, IPP commands).
- Provide access to request data (cookies, form data, URI, method, username, etc.).
- Support optional embedded web interface for device configuration.
- Handle authentication/authorization for admin or user tasks.
- Support HTTP/IPP, raw socket (port 9100), USB, SNMP, and DNS-SD discovery.

### How PAPPL Advertises to Clients

1. **DNS-SD/mDNS (Bonjour)**
2. **SNMPv1**
3. **Raw Sockets** (Port 9100)
4. **USB**
5. **Web Interface** (HTTP)

### Client Functions

| **Function**                | **Description**                                                                                            |
|----------------------------|------------------------------------------------------------------------------------------------------------|
| `papplClientGetCSRFToken`  | Retrieves the current CSRF token for secure HTML forms.                                                   |
| `papplClientGetCookie`     | Fetches a named cookie from the client request.                                                           |
| `papplClientGetForm`       | Retrieves form data (POST/GET) from the client request.                                                   |
| `papplClientGetHTTP`       | Accesses the underlying HTTP connection object.                                                           |
| `papplClientGetHostName`   | Gets the hostname from the client request.                                                                |
| `papplClientGetHostPort`   | Gets the port from the client request.                                                                    |
| `papplClientGetJob`        | Returns the `pappl_job_t` associated with the request (if any).                                           |
| `papplClientGetLoc`        | Retrieves localization data for the client's preferred language.                                          |
| `papplClientGetLocString`  | Fetches a localized string from the current language data.                                                |
| `papplClientGetMethod`     | Gets the HTTP method (e.g., GET, POST).                                                                   |
| `papplClientGetOperation`  | Gets the IPP operation code for an IPP request.                                                           |
| `papplClientGetOptions`    | Fetches any options from the HTTP request URI.                                                            |
| `papplClientGetPrinter`    | Returns the `pappl_printer_t` object tied to this request (if any).                                       |
| `papplClientGetRequest`    | Accesses the IPP request message structure.                                                               |
| `papplClientGetResponse`   | Accesses the IPP response message structure.                                                              |
| `papplClientGetSystem`     | Retrieves the `pappl_system_t` associated with the client.                                                |
| `papplClientGetURI`        | Gets the request’s URI.                                                                                   |
| `papplClientGetUsername`   | Retrieves the authenticated username, if available.                                                        |
| `papplClientHTMLAuthorize` | Handles web interface authorization.                                                                      |
| `papplClientHTMLEscape`    | Writes a string to the HTML output, escaping special characters.                                          |
| `papplClientHTMLFooter`    | Sends the HTML footer for the interface.                                                                  |
| `papplClientHTMLHeader`    | Sends the HTML header for the interface, including <head>, <title>, etc.                                  |
| `papplClientHTMLPrinterFooter` | Sends a footer specifically for printer-related pages.                                                  |
| `papplClientHTMLPrinterHeader` | Sends a header specifically for printer-related pages.                                                  |
| `papplClientHTMLPrintf`    | Writes formatted output to the HTML client, escaping where needed.                                        |
| `papplClientHTMLPuts`      | Writes a raw string to the HTML client.                                                                    |
| `papplClientHTMLStartForm` | Begins an HTML form, injecting the CSRF token automatically.                                              |
| `papplClientIsAuthorized`  | Checks if the client is authorized for admin-level tasks.                                                  |
| `papplClientIsEncrypted`   | Returns whether the connection is TLS-encrypted.                                                           |
| `papplClientIsValidForm`   | Validates the HTML form’s variables using the CSRF token.                                                 |
| `papplClientRespond`       | Sends an HTTP response (status code, headers, etc.).                                                       |
| `papplClientRespondIPP`    | Sends an IPP response with appropriate status and attributes.                                              |
| `papplClientRespondIPPUnsupported` | Responds with an IPP Unsupported error for attributes.                                                 |
| `papplClientRespondRedirect`| Issues a redirect response for the client.                                                                |
| `papplClientSetCookie`     | Sets a cookie in the client’s browser.                                                                     |

---

## Devices (`pappl_device_t`)

The **Devices** component manages physical or virtual printers, reached through URI strings (like `usb://...`, `socket://...`, `file://...`).

### What Devices Do

- **Listing**: Finds available devices (via `papplDeviceList`).
- **Opening/Closing**: Manages connections (`papplDeviceOpen` & `papplDeviceClose`).
- **Data Transfer**: Sends (`papplDeviceWrite`, `papplDevicePrintf`, etc.) and receives (`papplDeviceRead`) data.
- **Device Info**: Provides the IEEE-1284 device ID, metrics, and status.
- **Custom URI Schemes**: Register your own protocols with `papplDeviceAddScheme`.

### Device Functions

| **Function**                            | **Description**                                                                                                              |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `papplDeviceAddScheme` / `papplDeviceAddScheme2` | Registers a new device URI scheme (e.g., "usb", "socket"), with callbacks for listing, opening, reading, writing, etc.       |
| `papplDeviceClose`                     | Closes an open device connection.                                                                                            |
| `papplDeviceError`                     | Reports an error on a device, calling any registered error callback.                                                         |
| `papplDeviceFlush`                     | Flushes buffered data to the device.                                                                                        |
| `papplDeviceGetData`                   | Gets device-specific data set by the open callback.                                                                          |
| `papplDeviceGetID`                     | Gets the IEEE-1284 device ID string.                                                                                        |
| `papplDeviceGetMetrics`                | Fetches device metrics (e.g., read/write counts, timing).                                                                    |
| `papplDeviceGetStatus`                 | Retrieves printer status bits.                                                                                               |
| `papplDeviceGetSupplies`               | Retrieves current supply levels (ink, toner, etc.).                                                                          |
| `papplDeviceIsSupported`               | Checks if a given URI scheme is supported.                                                                                  |
| `papplDeviceList`                      | Lists available devices, filtering by device type(s).                                                                        |
| `papplDeviceOpen`                      | Opens a device given its URI.                                                                                                |
| `papplDevicePrintf`                    | Writes a formatted string to the device.                                                                                    |
| `papplDevicePuts`                      | Writes a literal string to the device.                                                                                      |
| `papplDeviceRead`                      | Reads data from the device.                                                                                                 |
| `papplDeviceRemoveScheme`              | Removes a device URI scheme.                                                                                                 |
| `papplDeviceRemoveTypes`               | Removes all schemes associated with certain device types.                                                                    |
| `papplDeviceSetData`                   | Sets device-specific data, typically used in open callback.                                                                  |
| `papplDeviceWrite`                     | Writes raw data to the device.                                                                                               |

### Device Data Structures

- **`pappl_device_t`**: Represents an open device connection.
- **`pappl_devmetrics_t`**: Tracks read/write counts, times, status requests, etc.
- **`pappl_devtype_t`**: Bitfield for device types (USB, network, file, SNMP, etc.).
- **Callback types** like `pappl_devopen_cb_t`, `pappl_devwrite_cb_t`, `pappl_devid_cb_t`, etc.

---

## Printers (`pappl_printer_t`)

The **Printers** component manages printer objects. Each printer has:

- A **device** (from the Devices layer).
- A **driver** (e.g., raster or custom).
- A queue of **Jobs**.

### Printer Functions

| **Function**                          | **Description**                                                                                                   |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| `papplPrinterCreate`                 | Creates a new printer object.                                                                                    |
| `papplPrinterDelete`                 | Deletes a printer.                                                                                                |
| `papplPrinterFindJob`                | Finds a job by ID within a printer’s queue.                                                                      |
| `papplPrinterIterateActiveJobs`      | Iterates over active jobs for that printer.                                                                       |
| `papplPrinterIterateAllJobs`         | Iterates all jobs (active, pending, completed).                                                                   |
| `papplPrinterIterateCompletedJobs`   | Iterates only completed jobs.                                                                                     |
| `papplPrinterGetContact`             | Retrieves contact info for the printer.                                                                           |
| `papplPrinterGetDeviceID`            | Gets the IEEE-1284 device ID string.                                                                              |
| `papplPrinterGetDeviceURI`           | Gets the URI for the printer’s device.                                                                            |
| `papplPrinterGetDNSSDName`           | Gets the DNS-SD service instance name.                                                                            |
| `papplPrinterGetDriverAttributes`    | Retrieves driver IPP attributes.                                                                                  |
| `papplPrinterGetDriverData`          | Gets driver-specific data.                                                                                       |
| `papplPrinterGetDriverName`          | Returns the driver name.                                                                                         |
| `papplPrinterGetGeoLocation`         | Gets the “geo:” location URI.                                                                                    |
| `papplPrinterGetID`                  | Gets the printer’s unique numeric ID.                                                                             |
| `papplPrinterGetImpressionsCompleted`| Number of sides/impressions printed so far.                                                                       |
| `papplPrinterGetLocation`            | The human-readable location (e.g., “Front Desk”).                                                                 |
| `papplPrinterGetMaxActiveJobs`       | Maximum number of active jobs allowed in the queue.                                                               |
| `papplPrinterGetMaxCompletedJobs`    | Maximum number of completed jobs retained in history.                                                             |
| `papplPrinterGetMaxPreservedJobs`    | Maximum number of preserved jobs with actual document data.                                                       |
| `papplPrinterGetName`                | Printer’s friendly name.                                                                                         |
| `papplPrinterGetNextJobID`           | ID that will be used for the next new job.                                                                        |
| `papplPrinterGetNumberOfActiveJobs`  | Current count of active jobs in the queue.                                                                        |
| `papplPrinterGetNumberOfCompletedJobs`| Count of completed jobs.                                                                                          |
| `papplPrinterGetNumberOfJobs`        | Total number of jobs so far.                                                                                      |
| `papplPrinterGetOrganization`        | Gets the organization name for the printer.                                                                       |
| `papplPrinterGetOrganizationalUnit`  | Gets the organizational unit name.                                                                                |
| `papplPrinterGetPath`                | Path of the printer’s page in the web interface.                                                                  |
| `papplPrinterGetPrintGroup`          | Print authorization group name.                                                                                   |
| `papplPrinterGetReasons`             | Retrieves the “printer-state-reasons” bitfield.                                                                   |
| `papplPrinterGetState`               | Retrieves the “printer-state” (idle, processing, stopped, etc.).                                                 |
| `papplPrinterGetSupplies`            | Fetches supply levels like toner or ink.                                                                          |
| `papplPrinterGetSystem`              | Gets the `pappl_system_t` that manages this printer.                                                              |
| `papplPrinterHoldNewJobs`            | Instructs the printer to hold (not start) any newly submitted jobs.                                              |
| `papplPrinterIsAcceptingJobs`        | Returns if the printer is currently accepting jobs.                                                               |
| `papplPrinterIsDeleted`              | Checks if the printer is being deleted.                                                                           |
| `papplPrinterOpenDevice`             | Opens the printer’s associated device.                                                                            |
| `papplPrinterCloseDevice`            | Closes the printer’s device.                                                                                      |
| `papplPrinterOpenFile`               | Opens a file for the printer (e.g., spool file).                                                                  |
| `papplPrinterPause`                  | Pauses a printer (stops processing jobs).                                                                         |
| `papplPrinterReleaseHeldNewJobs`     | Releases any new jobs that were being held.                                                                       |
| `papplPrinterResume`                 | Resumes a paused printer.                                                                                         |
| `papplPrinterSetContact`             | Sets the printer’s contact info.                                                                                  |
| `papplPrinterSetDNSSDName`           | Updates the DNS-SD service instance name.                                                                         |
| `papplPrinterSetDriverData`          | Configures driver data and attributes.                                                                            |
| `papplPrinterSetDriverDefaults`      | Sets default driver options (like media sizes, color modes, etc.).                                                |
| `papplPrinterSetGeoLocation`         | Sets the printer’s `geo:` location URI.                                                                           |
| `papplPrinterSetImpressionsCompleted`| Manually updates impressions count.                                                                               |
| `papplPrinterSetLocation`            | Sets the human-readable printer location.                                                                         |
| `papplPrinterSetMaxActiveJobs`       | Limits how many jobs can be actively processed.                                                                   |
| `papplPrinterSetMaxCompletedJobs`    | Limits how many completed jobs are stored in history.                                                             |
| `papplPrinterSetMaxPreservedJobs`    | Limits how many job files are preserved for reprinting.                                                           |
| `papplPrinterSetNextJobID`           | Manually sets the next job ID.                                                                                    |
| `papplPrinterSetOrganization`        | Sets the organization name.                                                                                       |
| `papplPrinterSetOrganizationalUnit`  | Sets the organizational unit.                                                                                     |
| `papplPrinterSetPrintGroup`          | Sets the print authorization group.                                                                               |
| `papplPrinterSetReadyMedia`          | Configures the default (ready) media sizes/types.                                                                  |
| `papplPrinterSetReasons`             | Sets or clears “printer-state-reasons” bits.                                                                      |
| `papplPrinterSetSupplies`            | Updates supply levels.                                                                                            |
| `papplPrinterSetUSB`                 | Configures USB vendor/product ID for gadget printing.                                                             |
| `papplPrinterAddLink`                | Adds a custom navigation link for the printer’s web UI pages.                                                     |
| `papplPrinterRemoveLink`             | Removes a navigation link.                                                                                        |
| `papplPrinterCancelAllJobs`          | Cancels all active/pending jobs for that printer.                                                                 |

---

## Jobs (`pappl_job_t`)

The **Jobs** component manages individual print jobs:

- **Job Lifecycle**: Created, queued, processed, completed (or canceled).
- **Job Data**: Stores filename, format, attributes, page ranges, user info, etc.
- **Driver Interaction**: Uses the printer’s driver to produce output.

### Job Data Structures

- **`pappl_job_t`**: Represents a single job (ID, state, associated printer, etc.).
- **`pappl_pr_options_t`**: Print options structure (copies, color mode, media, etc.).

### Job Functions

| **Function**                 | **Description**                                                                                  |
|-----------------------------|--------------------------------------------------------------------------------------------------|
| **Creation/Deletion**       |                                                                                                  |
| `papplJobCreateWithFile`    | Creates a new job from a local file. (Internally also used for IPP/HTTP uploads.)                |
| **Information Retrieval**   |                                                                                                  |
| `papplJobGetAttribute`      | Gets a named Job Template attribute.                                                             |
| `papplJobGetCopies`         | Gets the number of copies.                                                                       |
| `papplJobGetCopiesCompleted`| Gets how many copies have been fully printed.                                                    |
| `papplJobGetData`           | Retrieves driver-specific data.                                                                  |
| `papplJobGetFilename`       | Returns the spool file’s name.                                                                   |
| `papplJobGetFormat`         | MIME type of the document (e.g., “application/pdf”).                                             |
| `papplJobGetID`             | Numeric ID of the job.                                                                           |
| `papplJobGetImpressions`    | Total impressions (sides/pages) for the document.                                                |
| `papplJobGetImpressionsCompleted` | How many sides have been printed so far.                                                        |
| `papplJobGetMessage`        | Current status message (e.g., “Printing page 3...”).                                             |
| `papplJobGetName`           | The job’s title/name.                                                                            |
| `papplJobGetPrinter`        | Returns the `pappl_printer_t` the job is queued to.                                              |
| `papplJobGetReasons`        | The “job-state-reasons” bitfield.                                                                |
| `papplJobGetState`          | The “job-state” value (pending, processing, canceled, etc.).                                     |
| `papplJobGetTimeCompleted`  | UNIX timestamp when the job completed.                                                           |
| `papplJobGetTimeCreated`    | UNIX timestamp when the job was created.                                                         |
| `papplJobGetTimeProcessed`  | UNIX timestamp when the job actually started printing.                                           |
| `papplJobGetUsername`       | Username that submitted the job.                                                                 |
| **Information Setting**     |                                                                                                  |
| `papplJobSetData`           | Sets driver-specific data for the job.                                                           |
| `papplJobSetImpressions`    | Sets total impressions for the job.                                                              |
| `papplJobSetImpressionsCompleted` | Updates impressions completed.                                                                  |
| `papplJobSetMessage`        | Sets a custom processing message.                                                                 |
| `papplJobSetReasons`        | Sets/clears bits in “job-state-reasons.”                                                          |
| **Control**                 |                                                                                                  |
| `papplJobCancel`            | Cancels an active or pending job.                                                                |
| `papplJobHold`              | Holds a job from starting to print.                                                              |
| `papplJobRelease`           | Releases a held job.                                                                             |
| `papplJobResume`            | Resumes a suspended job.                                                                         |
| `papplJobRetain`            | Retains a completed job in the queue for reprint.                                                |
| `papplJobSuspend`           | Suspends a job in progress.                                                                      |
| **Options**                 |                                                                                                  |
| `papplJobCreatePrintOptions`| Creates a `pappl_pr_options_t` from the job + printer defaults.                                  |
| `papplJobDeletePrintOptions`| Frees the print options data.                                                                     |
| **File Access**             |                                                                                                  |
| `papplJobOpenFile`          | Opens the spool file associated with the job.                                                    |
| **Filtering**               |                                                                                                  |
| `papplJobFilterImage`       | Applies an image filter to the job data (if needed).                                             |

**Job States** include:
- `IPP_JSTATE_PENDING`, `IPP_JSTATE_PROCESSING`, `IPP_JSTATE_HELD`, `IPP_JSTATE_CANCELED`, `IPP_JSTATE_ABORTED`, `IPP_JSTATE_COMPLETED`, and `IPP_JSTATE_STOPPED`.

---

## Error Codes and Structures

**PAPPL** provides various error handling mechanisms:

1. **General Error Handling**
   - **Logging**: Use `papplLog(...)` for system-wide messages or `papplLogClient`, `papplLogPrinter`, `papplLogJob` for context-specific logging.
   - **Device Errors**: `papplDeviceError` reports device-level errors.

2. **Specific Error Scenarios**
   - **Printer Creation Errors**:
     - `EEXIST`, `EINVAL`, `EIO`, `ENOENT`, `ENOMEM`
   - **Job State Errors** (via “job-state-reasons” bitfield):
     - E.g., `PAPPL_JREASON_ABORTED_BY_SYSTEM`, `PAPPL_JREASON_DOCUMENT_FORMAT_ERROR`, `PAPPL_JREASON_PRINTER_STOPPED`, etc.
   - **Device Communication Errors**:
     - `papplDeviceWrite` returning a negative value, etc.
   - **System State Errors**:
     - `papplSystemLoadState` returning false if loading fails.

3. **Device Status Errors** (`pappl_preason_t` bitfield), e.g.
   - `PAPPL_PREASON_COVER_OPEN`, `PAPPL_PREASON_MARKER_SUPPLY_LOW`, `PAPPL_PREASON_MEDIA_JAM`, `PAPPL_PREASON_OFFLINE`, etc.

4. **System-Wide Logging**
   - **System**: `papplLog(...)`
   - **Clients**: `papplLogClient(...)`
   - **Devices**: `papplLogDevice(...)`
   - **Printers**: `papplLogPrinter(...)`
   - **Jobs**: `papplLogJob(...)`

---

## Callback Functions

**Callback functions** are how PAPPL lets you plug in your own logic. They’re passed to PAPPL functions and called at the appropriate time:

### System Callbacks

- `pappl_auth_cb_t`: For HTTP auth.
- `pappl_devlist_cb_t`: For device discovery.
- `pappl_deverror_cb_t`: Reports device errors.
- `pappl_devopen_cb_t`: Opens a device connection.
- `pappl_devclose_cb_t`: Closes a device connection.
- `pappl_devread_cb_t`: Reads from a device.
- `pappl_devwrite_cb_t`: Writes to a device.
- `pappl_devstatus_cb_t`: Gets device hardware status.
- `pappl_devid_cb_t`: Gets an IEEE-1284 device ID.
- `pappl_devsupplies_cb_t`: Gets supply levels.
- `pappl_mime_cb_t`: Custom MIME detection.
- `pappl_ipp_op_cb_t`: Handles custom IPP operations.
- `pappl_network_get_cb_t` / `pappl_network_set_cb_t`: Get/set network configs.
- `pappl_resource_cb_t`: Dynamic resource callback (serving custom pages/files).
- `pappl_save_cb_t`: Save system state.
- `pappl_timer_cb_t`: Timer callback for recurring tasks.
- `pappl_wifi_join_cb_t`, `pappl_wifi_list_cb_t`, `pappl_wifi_status_cb_t`: For Wi-Fi config.
- `pappl_ml_system_cb_t`: Mainloop system creation callback.
- `pappl_ml_usage_cb_t`: Mainloop usage display callback.
- `pappl_event_cb_t`: Handles system events (job creation, printer state changes, etc.).

### Printer Callbacks

- `pappl_pr_driver_cb_t`: Configures driver data for a named driver.
- `pappl_pr_autoadd_cb_t`: Automatically selects a driver for a discovered device.
- `pappl_pr_printfile_cb_t`: Prints a raw file (for drivers that accept unprocessed data).
- `pappl_pr_rstartjob_cb_t`: Called at the start of a raster job.
- `pappl_pr_rstartpage_cb_t`: Called at the start of each raster page.
- `pappl_pr_rwriteline_cb_t`: Called for each raster line data.
- `pappl_pr_rendpage_cb_t`: Called at the end of a raster page.
- `pappl_pr_rendjob_cb_t`: Called at the end of a raster job.
- `pappl_pr_identify_cb_t`: Used for audible/visual printer identification.
- `pappl_pr_status_cb_t`: Updates printer state and supply levels.
- `pappl_pr_testpage_cb_t`: Prints a self-test page.
- `pappl_pr_usb_cb_t`: Raw USB I/O callback for each byte from the USB host.
- `pappl_pr_create_cb_t`: Called right after a printer is created, for custom setup.
- `pappl_pr_delete_cb_t`: Called right before a printer is deleted, for cleanup.

### Job Callbacks

- `pappl_job_cb_t`: For iterating over jobs.
- `pappl_mime_filter_cb_t`: Filters document data to a new format (e.g., PDF -> raster).

---

## From Scratch Execution Flow

Below is a high-level sequence showing how PAPPL typically runs when you use `papplMainloop`. This is the “happy path” for a standard Printer Application:

1. **Program Startup**
   - Your `main()` calls `papplMainloop(argc, argv, ...)`.
   - `papplMainloop` parses command-line args (e.g., `--help`, `--add`, `--autoadd`, etc.).

2. **System Creation and Initialization**
   - If no custom `system_cb` is provided, PAPPL calls `papplSystemCreate` with defaults.
   - If a `--state-file` is given, `papplSystemLoadState` is called to restore printers/jobs.
   - `papplSystemSetPrinterDrivers(...)` sets up drivers, auto-add callback, etc.
   - If `autoadd` is requested, PAPPL enumerates devices (`papplDeviceList`) and calls your `autoadd_cb` to create printers automatically.

3. **Main Loop Running**
   - `papplSystemRun` starts a multi-threaded HTTP/IPP server.
   - Listens on configured ports/sockets (`papplSystemAddListeners`).
   - Installs signal handlers for graceful shutdown.
   - Periodically does housekeeping (cleans old jobs, calls timer callbacks).

4. **Handling Client Connections**
   - For each incoming connection, a `pappl_client_t` is created.
   - PAPPL parses HTTP/IPP requests.
   - Web interface routes to built-in or your custom resource callbacks.
   - If a file is submitted to print, a new `pappl_job_t` is created.

5. **Job Lifecycle**
   - A spool file is created (`papplJobOpenFile`).
   - If the driver is raster-based, PAPPL calls your raster callbacks (`rstartjob_cb`, etc.).
   - If it’s a raw job, it may call `printfile_cb`.
   - The job runs until completion, cancellation, or error.

6. **Web/IPP Management**
   - Users can visit `http://hostname:port/` for the web interface.
   - They can see printers, jobs, manage settings, etc.
   - IPP clients can query printers, submit print jobs, or request job status.

7. **Shutdown**
   - A user or IPP request calls “Shutdown-System,” or `papplSystemShutdown` is invoked.
   - The main loop stops.
   - `papplSystemSaveState` may store updated state to disk.
   - `papplSystemDelete` frees resources.
   - `papplMainloop` returns control to `main()` with an exit status.

---

## Wrap-Up

That's it for the grand tour of **PAPPL**! I have tried to cover everything from system creation, device management, and printer configuration, to job handling, error codes, and the callback system that ties it all together.

_**Happy Printing!**_
