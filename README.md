[![CircleCI](https://circleci.com/gh/SAP/cloud-mta-build-tool.svg?style=svg&circle-token=ecedd1dce3592adcd72ee4c61481972c32dcfad7)](https://circleci.com/gh/SAP/cloud-mta-build-tool)
[![Go Report Card](https://goreportcard.com/badge/github.com/SAP/cloud-mta-build-tool)](https://goreportcard.com/report/github.com/SAP/cloud-mta-build-tool)
![GitHub license](https://img.shields.io/badge/license-Apache_2.0-blue.svg)
![pre-alpha](https://img.shields.io/badge/Release-pre--alpha-orange.svg)

> <b>Disclaimer</b>: The Multi-target Application Archive Builder tool is under heavy development and is currently in a `pre-alpha` stage.
                   Some functionality is still missing and the APIs are subject to change; use at your own risk.



## Description

The Multi-target Application Archive Builder is a standalone command-line tool that builds a deployment-ready
multi-target application (MTA) archive `.mtar` file from the artifacts of an MTA project according to the project’s MTA
development descriptor (`mta.yaml` file) or from module build artifacts according to the MTA deployment descriptor (`mtad.yaml` file).

### Multi-Target Application

Before using this package, be sure you are familiar with the multi-target application concept and terminology.
For background and detailed information, see the [Multi-Target Application Model](https://www.sap.com/documents/2016/06/e2f618e4-757c-0010-82c7-eda71af511fa.html) guide.                   

## Usage

### Supported commands

| Command | Usage &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       | Description                                                    
| ------  | --------       |  ----------                                                    
| version | `mbt -v`     | Prints the Multi-Target Application Archive Builder tool version.                                        | x
| help    | `mbt -h`     | Prints all the available commands.                             
| assemble    | `mbt assemble`     | Creates an MTA archive `.mtar` file from the module build artifacts according to the MTA deployment descriptor (`mtad.yaml` file). Runs the command in the directory where the `mtad.yaml` file is located. <br>**Note:** Make sure the path property of each module's `mtad.yaml` file points to the module's build artifacts that you want to package into the target MTA archive. 
| init    | `mbt init`     | Generates the `Makefile.mta` file according to the MTA descriptor (`mta.yaml` file). <br> The `make` command uses the generated `Makefile.mta` file to package the MTA project. <br> Use the `mbt init` command with the following flags:<br><ul><li>`-s (--source)` is the path to the MTA project; the current path is set as the default.<br> Example: `mbt init -s C:/TestProject` <li>`-t (--target)` is the path to the generated Makefile folder; the current path is set as the default. <br> Example: `mbt init -t C:/TestFolder`<li>`-m (--mode)` is the mode of the Makefile generation; supported values are "default" and "verbose"<br> Example: `mbt init -m default`   
<br>
For more information, see the command help output available via either of the following:

- `mbt [command] --help` 
- `mbt [command] -h`


### Makefile
The `Makefile.mta` file that is generated by the `mbt init` command is the actual project "builder". It provides the verbose build manifest, which can be changed according to the project needs. It is responsible for:
- Building each of the modules in the MTA project.
- Invoking the MBT commands in the right order.
- Archiving the MTA project.<br><br>
Use the `make` command to package the MTA project with the following parameters:



| Parameter        | Type | Mandatory&nbsp;/<br>Optional        | Description                 | Examples&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    
| -----------  | ------ | -------       |  ----------                              |  --------------------------------------
| `p`  | string     | Mandatory     | The name of the deployment platform. <br>The supported deployment platforms are: <ul><li>cf <li>neo<li>xsa                                      |`make -f Makefile.mta p=cf`
| `t`    | string     | Optional  | The folder for the generated MTAR file. The default value is the current folder. If this parameter is not provided, the MTAR file is saved in the `mta_archives` subfolder of the current folder. If the parameter is provided, the MTAR file is saved in the root of the folder provided by the argument.                              | `make -f Makefile.mta p=cf t=C:\temp`
| `mtar`    | string     |   Optional  | The file name of the generated archive file. If this parameter is omitted, the file name is created according to the following naming convention: <br><br> `<mta_application_ID>_<mta_application_version>.mtar` <br><br> If the parameter is provided, but does not include an extension, the `.mtar` extension is added. | `make -f Makefile.mta p=cf mtar=myMta`<br><br> `make -f Makefile.mta p=cf mtar=myMta.mtar`
| `strict`    | Boolean     | Optional    | The default value is `true`. If set to `true`, the duplicated fields and fields that are not defined in the `mta.yaml` schema are reported as errors. If set to `false`, they are reported as warnings. | `make -f Makefile.mta p=cf strict=false`


#### Quick start example

```go
// Generate Makefile.mta file.
mbt init 

// Execute MTA project build.
make -f Makefile.mta p=cf

```


### Milestone 1  - (Q1-Q2 - 2019)

 - [x] Supports project-assembly-based deployment descriptors.
 - [x] Supports the building of HTML5 applications (non repo).
 - [x] Supports the building of node applications.
 - [ ] Partially supports build parameters (first phase):
    - [ ] Supports build dependencies.
    - [ ] Supports the copying of build results from other modules.
    - [ ] Supports the build results from a different location.
    - [x] Supports target platforms.
 - [x] Supports the generation of a default `Makefile` file.
 - [ ] Supports the generation of an `mtad.yaml` file from an `mta.yaml` file.
 - [ ] Supports the building of `XSA` and `CF` (Cloud Foundry) targets.

### Milestone 2 - (Q2 - 2019)

  - [ ] Supports the generation of verbose `Makefile` files.
  - [ ] Supports MTA extensions.
  - [ ] Supports the building of Java and Maven applications.
  - [ ] Supports ZIP builds.
  - [ ] Supports fetcher builds.
  - [ ] Supports build parameters:
    - [ ] Supports build options.
    - [ ] Supports `ignore` files and folders.
    - [ ] Supports the definition of timeouts.
    - [ ] Supports the naming of build artifacts.
  - [ ] Supports multi-schema.
  - [ ] Supports the enhancing of schema validations.
  - [ ] Supports semantic validations.
  - [ ] Partially supports the advanced `mta.yaml` (3.1 > 3.2) schema.


 ### Milestone 3 - (Q3 - 2019)

  - [ ] Supports parallel execution for the default `Makefile` file.
  - [ ] Supports incremental builds; in other words, one module at a time.

 ### Milestone 4 - (Q3 - 2019)

 - [ ] Supports the extensibility framework.
 - [ ] Fully supports the advanced `mta.yaml` (3.1 > 3.2) schema.

## Download and Installation

 There are two supported ways to install the multi-target application archive builder (MBT) tool:

 **Manually**:
  1. [Download](https://github.com/SAP/cloud-mta-build-tool/releases) the **latest** binary file according to your operating system.
  2. Extract the archive file to the folder where you want to install the tool.
  3. Add the binary file to your `~/bin` path according to your operating system:     
        - Darwin / Linux
          - Copy the binary file to the `~/usr/local/bin/` folder, for example: `cp mbt /usr/local/bin/`
        - Windows
          -  Copy the binary file `mbt.exe` to the `C:/Windows/` folder.

**Use npm**:
```
npm install -g mbt
```

## Contributions

Contributions are greatly appreciated.
See the [CONTRIBUTING.md](./.github/CONTRIBUTING.md) file for details.

## Known Issues

No known major issues.

## Support

Please follow our [issue template](https://github.com/SAP/cloud-mta-build-tool/issues/new/choose) on how to report an issue.

 ## License

Copyright (c) 2019 SAP SE or an SAP affiliate company. All rights reserved.

This file is licensed under the Apache 2.0 License [except as noted otherwise in the LICENSE file](/LICENSE).
