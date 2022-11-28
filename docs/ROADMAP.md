This page contains the rough outline of the CARP Roadmap for overall planing. Detailed planing takes place in the [CARP Development Trello Board](https://trello.com/b/gNTm2KK4/carp-development).

## Roadmap

* MUBS as an inspiration for
   * authentication
   * user sign up
   * collections
   * simple functions [DW]
   * a Flutter API [JB]

* Bind carp.sensing-flutter together w. carp.webservices
   * Flutter API for carp.webservices 
   * authentication
   * REST upload
   * file upload
   * backend support for file upload [DW]

* Study management
   * backend
   * front-end

* REAFEL project
   * Flutter app using CARP
   * ..


## Projects

CARP should support the following projects:

* MUBS -- Behavioral Activation (BA) app for mentally ill patients. [Darius]
* REAFEL -- A system for Cardiovascular Disease (CVD) management [Raju & Devender]
* ICAT / ubiCAT -- assessment of cognitive functioning from a web app (ICAT) and wearable app (ubiCAT) [Pegah]
* REACT -- collection of wearable sensor data from fitbit (or similar) [Henning]'
* BHRP
   * software architecture - multi-study, deployment, ...
   * SENS patch

## Major milestones / epics:

### Generic

* Authentication
* Multi-study setup (multiple studies by multiple researchers involving different/same patients)
* Multi-device setup (multiple data sources (devices) for a study)
* Application-specific data storage
* Open mHealth data storage
* Web app frontend for study management
* Flutter API to CARP
* Deployment @Computerome
* CARP website
* Flutter API for sensing
* Flutter API for open mHealth

### Project-specific

* REACH -- integrate to fitbit (other?) (we're awaiting a RAD from Henning)
* MUBS -- application-specific data + limited sensor data (e.g. location)
* REAFEL -- collection of CVD data (HR, pulse, ... and maybe ECG as a file?), context (sensor) data, and EMA data
