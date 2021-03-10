# About this manual
This section provides information about the Installation Manual, the target audience and typical
installation scenarios.
## What to expect
This manual explains how to install a helpLine system including the necessary preparation and
configuration tasks.
* In Preparing the installation (on page 7), you will find the hardware and software requirements as
well as instructions on how to prepare your environment and the destination system. Furthermore, if
you plan to use AD FS with helpLine, you will find detailed information here.
* Section Installing helpLine (on page 39) explains the installation of helpLine including the setup of
the SQL database server.
* A helpLine system configuration very much depends on the individual customer requirements. In
Configuring helpLine (on page 53) you learn how to choose and configure the different system
components after the installation has been completed successfully.

Note: 
> For information on additional components, such as start configurations or Add-ons, please refer to the corresponding installation manuals.

## Who should read this manual?
Note: 
> Since helpLine is typically customized according to your individual requirements, the installation should always be done in close contact with your helpLine Customer Project Manager.

This manual provides information for:
* IT or helpLine administrators, who install and set up helpLine systems.
* You should participate in the helpLine Administrator Training before installing and configuring a
helpLine system. Please contact your Customer Project Manager for details.
## Typical installation scenarios
A typical helpLine system requires at least:
* a helpLine application server,
* an SQL database server,
* a web server for running the helpLine web modules (Portal, WebDesk, MobileDesk).

These servers do not have to run on dedicated machines. Depending on your individual performance
requirements, the following basic installation scenarios are possible:

**One machine running all servers**  
In this scenario, the helpLine application server, the database server, and the web server are installed on
the same machine.

**Dedicated machine for the database server**  
In this scenario, the helpLine application server and the web server run on the same machine, while the
database server runs on a dedicated machine.

**Dedicated machines for helpLine application server, database server, and web server**  
In this scenario, a dedicated machine is provided for each server.
## Related information
* helpLine System Requirements
* Start configuration installation manuals
* AddOn manuals