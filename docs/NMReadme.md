# Notification Module  

[![Build status](https://tfs.pmcs-helpline.com/tfs/Helpline/Common/_apis/build/status/NotificationModule)](https://tfs.pmcs-helpline.com/tfs/Helpline/Common/_build/latest?definitionId=944) [![@sw/notificationmodule package in sw feed in Azure Artifacts](https://tfs.pmcs-helpline.com/tfs/Helpline/_apis/public/Packaging/Feeds/d5fbb08a-6bc6-47a4-9397-710bb9ab2537/Packages/3d012c14-6c30-4828-a5df-95c595a8757b/Badge)](https://tfs.pmcs-helpline.com/tfs/Helpline/Common/_packaging?_a=package&feed=d5fbb08a-6bc6-47a4-9397-710bb9ab2537&package=3d012c14-6c30-4828-a5df-95c595a8757b&preferRelease=true)

## Vision 🚀

```
“Provide our users with reliable and unified notifications about relevant events within the enterprise service platform”​
```

## Demo ✨

You can find a demo page [here](https://notification-demo-ui.labs.sabio.de/).

Credentials:

*Username: Patrick*

*Password: password*

You can also quickly setup your own quick demo for any version of the Notification Module in Maestro [here](https://maestro.labs.sabio.de/).

## Getting Started 🐱‍🏍

### Client 📖
#### 1. Access the artifacts feed 🎁

**@notificationmodule/client** is a private library. To download it you need to add a *.nprmc* file with the NPM credentials.

You can generate the credentials on the [TFS feed](https://tfs.pmcs-helpline.com/tfs/Helpline/Common/_packaging?_a=feed&feed=sw).

*your-project/.npmrc*
```
@notificationmodule:registry=https://tfs.pmcs-helpline.com/tfs/Helpline/_packaging/sw/npm/registry/
always-auth=true

HERE THE NPM CREDENTIALS
```

#### 2. Include the package 📦

To include the Notification Module execute the following command in your project directory:

```
yarn add @sw/notificationmodule@^1
```
Make sure to load the mandatory file e.g. for angular in the angular.json:

```json
"scripts": [
    "node_modules/@sw/notificationmodule/dist/elements/notification-module.js"
],
"styles": [
    "node_modules/@sw/notificationmodule/dist/elements/notification-module.css"    
]
```

If your application is not using the [SW control library](https://code-blue-controls-library.labs.sabio.de/home) you also must include the mandatory style file e.g. for angular in the angular.json:

```json
"styles": [
    "node_modules/@sw/notificationmodule/dist/elements/sw-angular-controls.css"    
]
```
Add this polyfill to your polyfill.ts

```js
// Used for browsers with partially native support of Custom Elements
import '@webcomponents/custom-elements/src/native-shim';
```

#### 3. Use it 🎉

When using it inside an Angular application you can use it in HTML file like this:

```html
<sw-notification-module [attr.service-url]="'THE_URL_TO_THE_NOTIFICATION_MODULE_SERVER'"
                        [attr.lang]="'THE_LANGUAGE_OF_THE_USER'"
                        [attr.id-token]="'THE_OPENIDCONNECT_ID_TOKEN'"
                        [attr.user-id]="'THE_USER_IDENTIFIER'"></sw-notification-module>
```

When using it in e.g. a plain web application you can also use it like this:

```html
<sw-notification-module></sw-notification-module>
```
```js
const element = document.querySelector('sw-notification-module');
element.setAttribute('service-url', 'THE_URL_TO_THE_NOTIFICATION_MODULE_SERVER');
element.setAttribute('lang', 'THE_LANGUAGE_OF_THE_USER');
element.setAttribute('id-token', 'THE_OPENIDCONNECT_ID_TOKEN');
element.setAttribute('user-id', 'THE_USER_IDENTIFIER');
```

#### Microsoft Internet Explorer Support

If you need to support Microsoft Internet Explorer, make sure to install these dependencies:
```json
"dependencies": {
    ...    
    "core-js": "3.3.3",
    "custom-event-polyfill": "1.0.7",
    "ie11-custom-properties": "3.0.6",
    ...
}
```
and import them in your polyfill.ts

```js
// Used for browsers without a native support of Custom Elements
import '@webcomponents/custom-elements/custom-elements.min';

// IE 11 Support
import 'core-js';
import 'custom-event-polyfill';
import 'ie11-custom-properties';
```

In case you encounter a zone.js SecurityError in the console of Microsoft Internet Explorer, follow these steps to solve it:
https://github.com/angular/zone.js/issues/1001

Features for the Notification Module that are not supported in Microsoft Internet Explorer:
- Desktop Notifications

### Server 📡

#### 1. Get the binaries 🧾

You can get the latest binaries by executing the following command in the directory you want to copy them:
```
\\lab-build01\ArtifactRepository\.cli\buildcli.cmd get -p NotificationModule.Deployment -n -s "https://tfs.pmcs-helpline.com/tfs/Helpline/Common/_git/NotificationModule"
```

Optionally you can also specify to just get the binaries for a specific runtime by appending `-f Deployment/<RuntimeIdentifier>` to the command.
Available runtimes are:
| Identifier | Description        |
| ---------- | ------------------ |
| win-arm    | Windows on ARM     |
| win-x64    | Windows x86 64-Bit |
| linux-arm  | Linux ARM          |
| linux-x64  | Linux x64 64-Bit   |

#### 2. Setup the server 💻

After getting the binaries you can find the setup script to install the Notification Module in the folder Deployment/&lt;RuntimeIdentifier&gt;. For Windows Deployments you can find a "setup.cmd" and for Linux a "setup<span></span>.sh" script.

You can pass in the following arguments for both `install` and `uninstall` commands:
| Argument            | Description                                                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `-s` \| `--silent`  | Runs the setup without any user interaction.                                                                                            |
| `-f` \| `--force`   | Ignores checks for the Installation location. WARNING: On uninstall this will remove all files in the installation path without asking! |
| `--verbose`         | Enables verbose logging to the console/terminal.                                                                                        |
| `--log false`       | Disable logging to a file.                                                                                                              |
| `--log-path <path>` | The path to the log file that is created by the setup.                                                                                  |
| `--help`            | Display information about a specific Verb.                                                                                              |
| `--version`         | Display version information.                                                                                                            |

##### Install

If you want to install the Notification Module by using a Wizard-like experience, just execute the script with the argument `install` and the setup will ask you all information it needs.

You can also specify all the information, if you want to run the setup automatically using the `--silent` switch, with the following arguments:
| Argument                         | Description                                                                                                                                                         |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-p` \| `--path`                 | The path to which the Notification Module should be installed to. This path should be a non-existant or empty directory.                                            |
| `-T` \| `--db-type`              | The database engine to use. Can be one of the following values: MSSql, Postgresql.                                                                                  |
| `-d` \| `--db-connection-string` | The connection string to the Notification Module database.                                                                                                          |
| `--db-server`                    | The database server of the Notification Module database.                                                                                                            |
| `--db-name`                      | The database name of the Notification Module database.                                                                                                              |
| `--db-user`                      | The user id that should be used to connect to the Notification Module database.                                                                                     |
| `--db-password`                  | The password that should be used to connect to the Notification Module database.                                                                                    |
| `--db-port`                      | The port of the database server of the Notification Module database.                                                                                                |
| `--db-schema`                    | The database schema to use for the Notification Module database.                                                                                                    |
| `--skip-db-migration`            | Skips the database creation/migration.                                                                                                                              |
| `--https-port`                   | The port for https protocol.                                                                                                                                        |
| `--certificate-pfx-path`         | The pfx file path of the certificate for the https port.                                                                                                            |
| `--certificate-pfx-password`     | The pfx file password of the certificate for the https port.                                                                                                        |
| `-u` \| `--service-user`         | The user under which the service should run.                                                                                                                        |
| `--oidc-authority`               | The url for the authority of the OpenIDConnect server instance instance.                                                                                            |
| `--oidc-audience`                | The client audience to use.                                                                                                                                         |
| `--oidc-claim-type`              | The claim that should be used to retrieve a unique user identifier from the OpenIDConnect token                                                                     |
| `--authorized-systems`           | The names and secrets of system that are authrized to use notification module.Multiple values can be provided (e.g. --authorized-systems sys1:secret1 sys2:secret2) |
| `--allowed-origins`              | The url addresses of sites that can access notification module API.Multiple values can be provided(e.g. --allowed-origins http://site1.com https://site2.com)       |

The following options are only available for Windows Builds:
| Argument                           | Description                                                                                        |
| ---------------------------------- | -------------------------------------------------------------------------------------------------- |
| `--service-user-password`          | The password for the provided service user.                                                        |
| `--integrated-security`            | Use integrated security. Only valid for MSSql and provided service user.                           |
| `-t` \| `--install-type`           | The type of installation. Can be one of the following values: Service, IISApplication, IISWebSite. |
| `-S` \| `--certificate-store`      | The store of the certificate to use for the IIS Website or Windows Service.                        |
| `--cert-allow-invalid`             | Allow invalid certificates.                                                                        |
| `-c` \| `--certificate-thumbprint` | The thumbprint of the certificate to use for the IIS Website or Windows Service.                   |
| `--set-cert-permission`            | Set read permissions for the specified certificate from the store for service-user.                |


There is also a "setup-silent" script in which you can type in all the information for faster reinstallation at a later point.

**Important:** Adjust the appsettings.json file in the chosen installation path after installation finished so it fits your requirements.

##### Uninstall

If you want to uninstall a Notification Module Installation, just call the setup script with the "uninstall" parameter.

##### Exit Codes

All exit codes are grouped into the following binary masks:

| Mask         | Description           |
| ------------ | --------------------- |
| `0x----01--` | General error         |
| `0x----02--` | Windows service error |
| `0x----03--` | IIS App Pool error    |
| `0x----04--` | IIS Web Site error    |
| `0x----05--` | IIS Application error |
| `0x----06--` | Systemd service error |
| `0x--01----` | Invalid prerequisite  |
| `0x--02----` | Preparation failed    |
| `0x--03----` | Installation failed   |
| `0x--04----` | Uninstallation failed |

All possible exit codes that the setup can return:
| Code (DEC) | Code (HEX)   | Description                                                                |
| ---------- | ------------ | -------------------------------------------------------------------------- |
| -1         | `0xFFFFFFFF` | An unknown error occurred                                                  |
| 0          | `0x00000000` | The setup finished without errors                                          |
| 65792      | `0x00010100` | A required parameter is missing                                            |
| 65793      | `0x00010101` | A parameter value is invalid                                               |
| 65794      | `0x00010102` | Retrieving the certificates from the given certificate store failed        |
| 65795      | `0x00010103` | A certificate with the given thumbprint was not found in the store         |
| 65796      | `0x00010104` | The notification was not found                                             |
| 65797      | `0x00010105` | The setup was not executed as Administrator/root                           |
| 67072      | `0x00010600` | The installation directory could not be retrieved from the systemd service |
| 131584     | `0x00020200` | Starting the Windows service failed                                        |
| 131585     | `0x00020201` | Stopping the Windows service failed                                        |
| 131840     | `0x00020300` | Starting the IIS App Pool failed                                           |
| 131841     | `0x00020301` | Stopping the IIS App Pool failed                                           |
| 132608     | `0x00020600` | Starting the systemd service failed                                        |
| 132609     | `0x00020601` | Stopping the systemd service failed                                        |
| 196864     | `0x00030100` | Copying the Notification Module Binaries failed                            |
| 196865     | `0x00030101` | Migrating/Creating the database failed                                     |
| 196866     | `0x00030102` | Updating the application settings failed                                   |
| 197120     | `0x00030200` | Creating the Windows service failed                                        |
| 197376     | `0x00030300` | Creating the IIS App Pool failed                                           |
| 197632     | `0x00030400` | Creating the IIS Web Site failed                                           |
| 197633     | `0x00030401` | Updating the IIS Web Site failed                                           |
| 197888     | `0x00030500` | Creating the IIS Application failed                                        |
| 197889     | `0x00030501` | Updating the IIS Application failed                                        |
| 198144     | `0x00030600` | Creating the systemd service failed                                        |
| 198145     | `0x00030601` | Updating the systemd service failed                                        |
| 262400     | `0x00040100` | Removing the Notification Module Binaries failed                           |
| 262656     | `0x00040200` | Removing the Windows service failed                                        |
| 262657     | `0x00040201` | The Notification Module Windows service was not found                      |
| 262912     | `0x00040300` | Removing the IIS App Pool failed                                           |
| 263168     | `0x00040400` | Removing the IIS Web Site failed                                           |
| 263424     | `0x00040500` | Removing the IIS Application failed                                        |
| 263680     | `0x00040600` | Removing the systemd service failed                                        |

#### 3. Access the Swagger UI 📊

You can find the Swagger UI of the Notification Module under:

```
https://{YOUR_CONFIGURED_NOTIFICATION_MODULE_URL}:{PORT}/api/notification/index.html
```

#### 4. Send events 🪁

To be able to send a new event to the Notification Module you must first request a token (JWT). All further requests must include the token.


## Architecture Documentation 📚

See our [architecture documenation](docs/arc42/01.%20Introduction%20and%20Goals.md) based on the [arc42](https://arc42.de/) template.

## Responsible Team 👩🏼‍🚀

| Name            | Role                  |
| --------------- | --------------------- |
| Nico Graichen   | Product Owner         |
| Marius Schulze  | Architect / Developer |
| Marc Schmidt    | Developer             |
| Nima Hamidian   | Developer             |
| Philip Koep     | Developer             |
| Jens Schmitz    | Developer             |
| Patrick Pfüller | Developer             |
